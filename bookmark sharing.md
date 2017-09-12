# Bookmark Sharing
It's a vital in Project Scavange to consider where the search results would come from. In many cases it could come from crawling the web (as in Argonjay and to a lesser extent Entrigue), but as librarians and geographers well know many datasets require more curation. 

Geographers in particular have solved the difficulties around curating "metadata catalogues" by using a federated system like GeoNetwork. In this scheme agencies would maintain their own metadata, and use standard "harvesting" protocols to copy their data into larger catalogues where more people can find it. 

Bookmark sharing plays a role in this by getting more people involved in curating content that could get federated up into larger catalogues. That is if the Odysseus experience could make the bookmarking experience so trivial that it would actually get used. 

**NOTE** While bookmark sharing is an important browser feature to get a start to curating content, the particulars more have to do with how the experience the browser wants to provide for bookmarks than the Project Scavange experience. As such the experience I'll describe here will stick to the essentials for Project Scavange, whilst leaving possibilities open for how bookmarks are organized and presented. 

## User Experience (Project Scavange vital parts)
The bookmarks manager would have a button/link to publish a subset of your bookmarks to IPFS, with options for using IPNS to publish any changes to that subset and/or whether the shared bookmarks should password protected. If it's not password protected, the page would suggest you tell EntrigueDB about your shared bookmarks using a TerpreT control to categorize it. 

Once some shared bookmarks are shared with someone (i.e. they opened it's page in a browser), it would be convenient for the browser to offer the option (but not the obligation) to incorporate those bookmarks into their own. 

### Privacy Measures
A user's bookmarks, much like their search/browser history is sensitive information that deserves protection. As such many of the features in the experience I described exist solely to protect this privacy whilst still providing a base for pages to get organized and curated. 

The most important of these is that users must be given a choice before any bookmarks are shared, which takes the form of them selecting a particular subset before hitting the share button as well as given a choice of whether future bookmarks organized similarly should also be shared. It also takes the form of password protecting shared bookmarks to (try to) limit who has access to it. Finally on IPFS making shared bookmarks not live has an additional privacy advantage: the page can be published without tying it to your identity. 

However if you do decide to make some of your bookmarks fully public, it would be helpful for you to make it discoverable through Entrigue. But still you are the one decide if you want it to be findable. 

### Experience in Odysseus
Odysseus would organize it's "favourites" into "tags". Some of these would be freeform, others would come from SKOS vocabularies. Most of these vocabularies would likely come from Argonjay (in which case the freeform tags would be translated into vocabulary terms), but they may come from elsewhere. 

The share link would be in the topright corner, and link to a two-element form. Upon submission this form takes you to the shared bookmarks page. From here there'd be a special tag listing all shared bookmarks, and another listing all live ones.

Because SKOS lists rules for inferring tags, the add/edit bookmark popover would include a semi-editable form field for inferred tags. This also gives me a chance to automatically add a tag representing which shared bookmarks it'll appear in to give the user a chance to remove it from them. 

## Implementation
Shared bookmarks are trivially implemented with IPFS and RDFa/SKOS/VOID, that is if you don't want a central server. 
