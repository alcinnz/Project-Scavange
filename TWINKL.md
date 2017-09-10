# The Web Index for Networked Knowledge & Links
Unlike the other components, TWINKL is not user-facing and is relatively complex. However it's an important component to take the burden out of making search engines peer-to-peer. 

TWINKL will be a peer-to-peer readonly database engine, and in comparison to PostgreSQL or even SQLite what I describe below is relatively simple. More specifically it'll be the SPARQL Query Language implemented ontop of IPFS. 

## Benefits Outside of Project Scavange
The SPARQL protocol (which TWINKL is unable to implement ontop of IPFS) has a couple of problems:

1. Sending queries to a remote endpoint incurs significant network lag
2. The remote endpoint may temporarily or permantly cease functioning
3. Running a SPARQL endpoint incurs significant computational costs

To solve the first two problems, early adopters of linked data technology have downloaded RDF graphs to query them locally. IPFS will do this automatically and on-demand. At the sametime publishing a TWINKL endpoint to IPFS will naturally incur less costs than a static webserver. 

## Performance goals
By far the most expensive operation involved in TWINKL is to fetch a database "page", as that operates over the network. All other costs are inconsequential. 

By far the most dominant operation in SPARQL is in resolving which values a certain query variable might hold. In terms of SQL this is comparable to a self-join operation (presuming the database has a single subject-predicate-object relation), which is the most expensive operation in any SQL database. So solving this performance problem is sure to have great gains.

## Data Structure

The datamodel behind SPARQL and Linked Data is a set of triplets (which can also be viewed as a graph). Making that set ordered and use a tree-based implementation would allow irrelevant pages to be largely ignored when running an intersection. The ordering elsewhere significantly helps optimize range queries and ordering. There are two major tree-based implementations of an ordered set: a Trie and a BTree. 

Of the two the trie would be more compact, as it would effectively prefix-compress the data (which would provide very good but not perfect compression for Linked Data), whereas BTrees prefer fixed-length keys so we know how many branches a node can have. Here that either means we have poor compression, or a seperate "heap-space" we can't afford. 

So a TWINKL database would consist of 6 tries for different permutations of subject, predicate, object triplets. The catch however is that since the ordering of tries is lexicographic, a BTree may be used in place of a trie node to query floating point values (which are difficult to get to order lexicographically). Other literal values may have their own TWINKL-specific little-endian notation. 

This will be encoded in IPFS with links to other blocks being the branches, and the optional content flagging completion of a triplet, value, or value component. Differentiating between these helps to optimize several filter functions from core SPARQL. 

## Query Algorithms

Essentially the query algorithm consists of multiple layers of loops around the datastructure. 

### Layer 0 - Data Access

```
interface Index:
  Iter<String> asc()
  Iter<String> desc()
  String flags()
  Index lookup(String key)
  Boolean has(String key)

classes Memory, IPFS implements Index

classes Union implements Index:
  String asc():
    if @a is undefined: @a = @A.asc()
    if @b is undefined: @b = @B.asc()
    if @a == @b:
      yield @a
      @a = @b = undefined
    if @a < @b:
      yield @a
      @a = undefined
    if @a > @b:
      yield @b
      @b = undefined

  String desc():
    essentially the same as @asc()

  String flags():
    return @A.flags() + @B.flags()

  Index lookup(String key):
    if @A.has(key) and @B.has(key):
      return new Union(@A.lookup(key), @B.lookup(key))
    elif @A.has(key):
      return @A.lookup(key)
    elif @B.has(key):
      return @B.lookup(key)
```

### Layer 1 - Locate keys

```
class Cursor:
  Stack<Index> path = [root]
  string cur_key
  
  String asc(String target):
    for key in @path.peek().asc():
      @cur_key = key
      if key >= target: return key

    @path.pop()
    @asc(target)
  String desc(String target):
    essentially same as @asc()

  String flags(): @path.peek().flags()
  fetch():
    @path.push(@path.peek().lookup(@cur_key))
```

### Layer 2 - Intersection/join

TODO incorporate range queries into this

```
Iter<String> joinAsc(Cursor[] cursors, terminal = 'v'):
  if target is undefined: target = cursors[0].asc()
  # Locate next target that might match
  for cursor in cursors:
    next = cursor.asc()
    if next > target:
      target = next
      repeat from start

  # Check it out
  fullmatch = true
  for cursor in cursors:
    if cursor.key != target:
      fullmatch = false
      cursor.fetch()

  # Determine whether to yield the value, or to explore more
  if fullmatch:
    if all(terminal in cursor.flags() for cursor in cursors):
      yield target
    else: # TODO What's this condition?
      cursors[0].fetch()

  # Load next value
  target = undefined
  repeat from start
```

### Layer 3 - Graph patterns

Chain the join() iterators together using:

```
Iterator<String> chain(Object context, Iterator<String> outer, Iterator<String> inner):
  for value in outer:
    context[outer.variable] = value
    yield from inner
```

### Layer 4 - Pipeline/Final adjustments

While SPARQL's syntax centers around graph patterns, which are implemented via the above loops, SPARQL offers various constructs straight from SQL. Those would be implemented as a pipeline of iterators around the ones described above. 

#### OFFSET/LIMIT

```
Iter<Object> offset(Int offset, Iter<Object> source):
  offset times: source.next()
  yield from source

Iter<Object> limit(Int limit, Iter<Object> source):
  limit times: yield source.next()
```

#### FILTER/PROJECT/LET

```
Iter<Object> filter(Expression condition, Iter<Object> source):
  for result in source:
    if condition(result): yield result

Iter<Object> project((String label, Expression ex)[] fields, Iter<Object> source):
  for result in source:
    yield {label : ex(result) forlabel, ex in fields}

Iter<Object> let(string label, Expression ex, Iter<Object> source):
  for result in source:
    result[label] = ex(result)
    yield result
```

#### AGGREGATE/DISTINCT

```
# Requires source to be sorted
Iter<Object> aggregate(AggregateFunc[] funcs, String[] fields, Iter<Object> source):
  next():
    local result = source.next()
    return [result[field] for field in fields]
  key = next()
  funcs.all(.reset())
  
  until next failed:
    cur_key = next()
    if cur_key != key:
      for func in funcs:
        result[func.variable] = func.finalize()
      yield result
      funcs.all(.reset())
    funcs.all(.step(result)

Iter<Object> SortedDistinct(Iter<Object> source):
  prev = undefined
  for result in source:
    if result != prev: yield result
    prev = result

Iter<Object> SlowDistinct(Iter<Object> source):
  prevs = set()
  for result in source:
    if result not in prevs:
      prevs.add(result)
      yield result
```

#### SORT

To be used when it can't be incorporated into the graph pattern matching. Insertion sort is selected so that the sorting can be done in parallel with the networking.

```
Iter<Object> sort(Expression[] fields, String presorted, Iter<Object> source):
  String key = undefined
  Object[] sorted = []
  for result in source:
    if result[presorted] != key:
      yield from sorted
      sorted = []
      key = result[presorted]
    insert result into proper place in sorted by cmp
  Int cmp(Object a, Object b):
    for field in fields:
      cmp = field(a).cmp(field(b))
      if cmp != 0:
        return cmp
    return 0
  yield from sorted
```
