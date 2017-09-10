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
