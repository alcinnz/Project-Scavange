# TWINKL Extensions
The core TWINKL algorithm described in [TWINKL] achieves a lot, however external plugins may be used to optimize certain filters or add new ones. This document describes how and why some of these might exist. 

## NGrams Index
Enabling the ngrams plugin on personal TWINKL triplestores would construct a new trie under the root called "ngrams". Alternatively if that index exists in the database, the plugin would be loaded automatically. 

This trie would map all sequences of three characters present in some text literal to all text literals that include said ngram. From here many filter functions would be extended with optimization handlers to use the trie to constrain the results more to what might pass the filters. 

## Discrete Global Grid System
I'd model DGGS geospatial data as being the union of all objects (denoted as a special literal) for a predicate/subject. From here it just takes a query on whether one starts with the other to perform an intersection test. Other geospatial operators may be done similarly. 

## GeoTWINKL
This would be a compatible implementation of GeoSPARQL built on the Discrete Global Grid System. Essentially it would just involve adding an understanding of a special ontology, as well as format conversions. 
