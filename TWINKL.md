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
