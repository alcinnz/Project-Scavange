# Combined Search
Browsers like Firefox and Opera have recently made strides in encouraging the use of different search engines for different purposes. However they reasonably don't want to require users to select a search engine before hitting `enter`, so they need to choose a default one to use in that case. 

While I don't want to force the selection of a search engine either this copout still puts pressure on users to select a default search engine and to use it predominantly. This won't do for project Scavange, so instead we'll fetch the results from all the registered search engines and merge sort them into a single display, with proper credit given. While this can't work well with the major search engines (including DuckDuckGo), it's not that valuable for them anyways. 

## User Experience

An icon resembling "🔎+" will show in the addressbar for sites with an OpenSearch Description. Clicking this will bookmark said description under special tags (Odysseus doesn't do bookmark folders) indicating that it will be autocompleted and searched through this tool.  Clicking the icon again would remove the site from these tags (stopping it from being integrated into the browser chrome). 

With hardly any extra chrome this makes configuring your search engines an offhand thought, instead of an involved process it is elsewhere. 

To search those search engines, users will simply type something into the addressbar and hit enter/select an autocompletion. This would then render an infinitely scrolling webpage with the title and blurbs of search results from all the "installed" search engines. Under each result would be a colourful bar indicating the sources for that result, with the same info appearing in a tooltip. A legend consisting of the search engine logos in colourful boxes would appear in the topright corner. Also in those boxes would be checkboxes appearing on hover to include or exclude certain search engines from the results (normally no background indicates not included). Appearing on hover beneath this legend would be additional boxes for including/excluding search engines by tag. 

### Backwards compatibility

A yellow magnifying glass would indicate that the site has a searchbox and/or has an OpenSearch descriptor that isn't complete enough to support combined search. Clicking it would, as expected, "install it". However instead of being configured within the familiar "favourites" interface, these search engines would have a dedicated settings pane (also accessible via rightclick on the addressbar). This settings pane would predominantly consist of a listbox whereby a default search engine can be selected and the search engines can be reordered. 

The combined search would be included in this list (rather than it's constituent parts), and autocompletions can be toggled on and off. Depending on which is selected from this list, different pages will be rendered for search.

This is essentially how search works in all other browsers, and while it doesn't have the thoughtless configuration the multi-search does I may be pushed into supporting it. This would be so users can use Google or DuckDuckGo until Project Scavange is at a point where they're comfortable with it. 

## Ranking Scheme

OpenSearch+RSS can implicitly or explicitly specify rankings on a search engine specific scale. Upon normalizing these to be a fractional value I can lookup all results within a given page of a search engine, average the ranking for that page between all included search engines (implying 0 for the search engines that don't have that page in their results), and sort them thusly.

If a result showed up in a previous page in the combined search, that should be excluded from later pages. And because the combined lengths of all the search results pages can be wildly inconsistant, infinite scrolling can reduce any confusion that arises. 
