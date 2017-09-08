# Entrigue
Combined search helps to substantially reduce everyday obstacles which encourage the one searchbox approach, but it doesn't fully address the less everyday obstacles of setting the combined search up. For this hunters need to be able to easily discover new search engines without interrupting their workflow. 

That is to encourage the proliferation of search engines targetting specific audiences, we need one targetting an audience who don't know where to search. 

*This document describes how users will interact with this "Entrigue" search engine.*

## Within Combined Search

When users search for certain *categorical* keywords, a search result low down titled 'Search Elsewhere For "_" _/_', with links to "subtopics" and possibly search engines in it's body. If any search engines are particularly dominant, they may be added to the results above this catchall result. As for any dominant subtopics, they will be added to the "Suggested searches". 

Clicking a result for a search engine (either in the catchall body or the main results) will perform a search on that search engine, whereas clicking a "topic" would bring the user to the Entrigue search interface. 

## Main interface

Entrigue's homepage will show a word cloud (where the words/"topics" are colour coded into "themes"). Clicking a topic would turn it black and rearrange (with an excited and brief animation) the words sized to just the results categorized under the selected topics. Clicking it again will deselect it. Search engines would also be added to the word cloud (sized to number of results) with the addition of favicons. Clicking one will either link to the site or zoom/rotate the viewport to turn it into a reasonably sized and level searchbox. 

For keyboard accessibility typing would underline all matches and `space`/`enter` will select the remaining match. 

In the footer would be links to about and privacy measures, as well as tags for "safe search" and languages. 

Additionally their may be a toggle button to switch to a view with the topics organized into columns for their themes. Each of these columns would have their background colour coded. 

## Entrigue DB
Also in the footer would be a toggle button labelled "Suggest Correction". This would turn the background black and switch to a monospace variant of the font. In this mode clicking a topic would take you to a editing interface for that topic/search engine. To help contributors there'd be comments on all these pages. In case of vandalization, it will be important to be able to revert records to previous versions. Another part of curation would involve flipping a configuration switch. 

To edit which topics a search engine is organized under, a special interface would be required to search all the (potentially numerous) topics for one to add. A similar interface would be required to specify which tags infer which other tags. 

A crawler would have a special account on this site to import any OpenSearch descriptors and VOID RDF metadata it finds. A messaging interface would be used to tell this crawler which sites it should check. 

The branding on this site should be colourful to reflect the wordcloud on the mainsite. 

## General Feel
The goal in this whole design is to get hunters from no idea where to look to some idea in less than a second. Making the interface somewhat fun can aid this as it encourages people to explore the possibilities, but fun and delight are not the main goals. 

The whole interface springs out of this, with font design being the main area available to introduce branding. 
