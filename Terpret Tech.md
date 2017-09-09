# Terpret Technology

This document describes the technology and algorithms which would power the Mac OS X Maverick's esque experience described in [Terpret UX].

## DOM hierarchy

```
- Control {appearance: searchbox;}
  * Tokens
    - new predicate button {arrow style}
    * predicate dropdowns
      * options
    - value
    - X button
  - input
  - dropdown
    * autocompletions

- means singular element
* means multiple adjacent elements
```

## Autocompletion

It's quite trivial to add a token to the query, or to lookup the predicates that can a path component can be changed to. The main challenge in implementing TerpreT thereby is figuring out which autocompletions to show.

A trie (whether it's on IPFS pages or in-memory) nicely handles the dominant interaction of appending text to the entry. It also suggests a way to adjust the UI to handle a large quanity of autocompletions: incorporate "partial autocompletions". 
An inmemory trie would handle the casual keywords. And the tries kept in IPFS would be a plain-text encoding of as much of the tree as would fit, mapping the keys to values. 

Interestingly this usage would mean the trie could also be served over HTTP or most any transport technology. And it bears mentioning that the trie would not just be downloaded for privacy protection, it would be a vital network optimization.
