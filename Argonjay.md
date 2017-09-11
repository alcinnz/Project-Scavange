# Argonjay
Argonjay would contribute to Project Scavange by encouraging some consistancy in the use of tags within shared bookmarks. Without this there'd be little incentive for me to integrate Argonjay into Odysseus's bookmarks manager. Also Argonjay is the sort of search engine I want to encourage with Project Scavange, one specifically suited to a domain - here SKOS vocabularies. 

## User Experience
Argonjay is designed to preserve a great experience between saving bookmarks and sharing them. A good experience sharing bookmarks requires collaborators to use roughly the same terms for organizing their bookmarks. For saving bookmarks a great experience (particularly on elementary) prohibits me from asking for configuration before the browser and/or bookmarks can be used, as it is not vital information for the browser to work. 

Accepting these constraints the answer is clear: Let the surfer enter free-form tags, but recommend (SKOS) vocabularies to resolve those free-form tags into something a bit more consistant. Or if Argonjay isn't integrated into the bookmarks manager, enter some desired terms into a classic search interface. In the search results, a vocabulary would be rendered with a title and the available terms arranged into a tree at right-angles off from their parent terms. 

## Technology

A [Scrapy](https://scrapy.org/) script would be used to harvest SKOS vocabularies, from which would be constructed a trie mapping the various label for a SKOS concept to 1) the SKOS vocabulary it's part of and 2) the number of terms in that vocabulary. 

To query this all the given terms would be looked up in the trie, with the number of hits on a vocabulary (descending) being the primary sort order, followed by number of terms in that vocabulary (asc). 

The rendering of a vocabulary would involve a traversal over the tree of terms whereby the sizes of children informs where the parent positions them, and in turn the size of the parent. Another pass could then be used to properly center those items and generate the necessary SVG. 
