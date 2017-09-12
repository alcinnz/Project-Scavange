# Project-Scavange
The goal: Encourage the success of search engines targetted to specific domains, over *the one searchbox to rule them all* (as in **Google**, Bing, DuckDuckGo, Yacy, etc). 

Being targetted to a specific domain means any application can provide better experience for that specific task than one that provides a mediocre experience for all tasks. Not only does this approach reliably make for nicer and clearer user experiences, but it also makes those experiences easier to implement. Heck this simplicity/focus may be what it takes to adopt peer-to-peer technology into our seach engines without ruining the user experience (just like LBRY or Alexandria did). 

To some extent this *is already* how people search for stuff online. If you're shopping for something you'll go to eBay (or for kiwis, TradeMe) instead of Google. But it's the question of what it takes to make this the dominant search paradigm that interests Project Scavange. 

## Principles
* Many simple and focused tools provides a better experience than a single complex tool (even if you hide that complexity).
* It helps people to communicate to computers if they know how it understands them. 
* Some people ("hunters") want data, others ("gatherers") have lots of data. It's our job to bridge these personas. 
* Search and reading/browsing history is highly personal and deserves the best privacy protection.
* In order to adiquitely protect privacy, software **must** adhear to The Four Freedoms.

## Underlying Technology
* Linked Data
* SKOS
* OpenSearch
* IPFS
* [Scrapy](https://scrapy.org/)

## Components
* ---Hunters---
* **Siren**^ For when people have no idea what they want to find. 
* **Combined Search**^ So users don't need to select a default search engine
* **Entrigue** So users can find the ideal search engine for their needs without interrupting their search workflow.
* **Entrigue DB** To curate the catalogue of search engines findable through Entrigue.
* **Terpret** Reusable search control to better communicate the interpretation of user queries.
* **TWINKL** SPARQL Language implemented upon IPFS.
* **Argonjay** Search for SKOS vocabularies, to encourage some consistancy in how shared bookmarks are tagged. 
* **Bookmark Sharing/Publishing**^ So casual gatherers (most everyone) can easily get others started.
* *A metadata catalogue like GeoNetwork* For the serious gatherers. 
* ---Gatherers---

^ indicates this is a browser feature to be incorporated into Odysseus where I'll showcase and test the Odysseus experience. 

## Contributing

If you know of others doing similar work to the above components, please issue a pull request to this repository to add a link. Also if you want to work with me on one of these components please get in contact with alcinnz@lavabit.com. If you work on a search technology that could work great alongside Project Scavange, please start a wikipage on this repository to list it until I can get Entrigue DB started.
