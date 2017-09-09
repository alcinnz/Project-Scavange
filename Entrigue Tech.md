# Entrigue Technology
This document describes the technology and algorithms used in achieving the experience described in [Entrigue UX]. 

## Entrigue in Combined Search
In order to make this work without uploading partial searches to a central server, the OpenSearch descriptions used by Entrigue must use a custom extension specifying some ES6 that yields each result (both for autocompletion and proper search). This ES6 would in turn query an IPFS-based index to resolve all entered topics. A trie would be a natural index for this purpose.

## Entrigue Mainsite
Much of the magic would come from d3-cloud (which in turn works by drawing text to an invisible canvas so it can read the precise shape of the text out of it, before constructing bitmaps to hittest newly inserted text into. Some more magic would come from ensuring the browser animates the correct words into place. Beyond that Entrigue only consists of querying some index structure. 

## EntrigueDB
Django admin site makes it trivial to develop the sort of CRUD interface needed by EntrigueDB, though it is targetted to a website's staff and would require rebranding to make it work. Nevertheless it provides a great experience and allows me to focus on modelling the data using Django's ORM (which also is also better suited to specifying the relationships that dominate the model than SQL). 

Add in [Reversion](https://github.com/etianen/django-reversion) and [Django ThreadedComments](https://github.com/HonzaKral/django-threadedcomments) and I'd have everything I need for the contrib site.

A bot account and [Scrapy](https://scrapy.org/) script would be all that's needed for the site to discover records itself, and build up a value to entice people to contribute to.

## Indices

The topics would include two indices: a trie to lookup IDs for a key, and a BTree to lookup metadata for an ID. These indices are trivially straightforward. 

It's more involved is to design an efficient index to lookup search engines and related tags for a query. The dominant need here is to avoid fetching and updating pages (as long as a page isn't updated it doesn't need to be fetched again). The secondary goal is to keep it obscure as to what queries would involve fetching certain pages, though IPFS already obfuscates that somewhat. 

### Querying the main index (Python-esque Psuedocode)

```
def query(topicIDs):
  query(makeBitmask(topic), root)

def query(q, node):
  for branch in node:
    if not branch.label & q: continue
    if branch is a leaf: yield branch.node
    else: yield from query(q, branch.node)
```

### Updating the main index (Python-esque Psuedocode)

Turning this into a (daily) batch update allows me to consolidate changes so that they have the minimum effect on the index. At the same time it complicates the algorithm as it now needs to handle both inserts and deletes.

```
def update(to_remove, to_add):
  unchanged = {root}
  for each page containing an item from to_remove (use query()):
    add any leaf pages to to_add
    remove from unchanged
    add any branch pages to unchanged
    repeat this for the ancestor pages

  def split(items, branches):
    n_splits = roof(items/AVG_ITEMS)
    choose n_splits random pivots from items
    group other items by min XOR distance to a pivot
    repeat previous steps until convergence
    
    cur_node = new Node()
    if there's multiple nodes:
      construct branches for each grouping
      split any branches that are too large
      add those branches to cur_node
    otherwise: add items to cur_node
    merge(cur_node, branches)
  
  def merge(node, new_branches):
    for each branch in node:
      if branch.label is superset= of any new_branches*.label:
        node.label = that_branch.label
        node.branches += that_branch.branches
      if branch.label is a subset of any new_branches*.label:
        merge(branch.node, those_branches)
    add any remaining new_branches to node. 
  
  split(to_add, unchanged)
```

The split algorithm is taken from AI (whilst being used in a very non-AI context). It is called k-means clustering and is used to detect k natural groupings in a dataset. 
