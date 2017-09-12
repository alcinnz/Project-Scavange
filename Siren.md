# Siren Recommendations
Sometimes people don't know what they're looking for. Sometimes they do. If you properly seperate these usecases the former's "discovery" and the latter's "search", and Google seems to strive for a combination of both. 

Currently there aren't many good tools that fully address discovery, and what does exist is biased by advertising and/or invades your privacy. And then there's those who follow the "ofcourse you can trust us" fallacy which caused the privacy problems we face today. 

Essentially the experience would be presenting the user with a bunch of links that may interest them. Some of these would be based on other pages they have viewed, and others would be relatively greenfield to avoid placing the user in a filter bubble. 

## Technology
Minhash would be used to group hunters into groups by how similar their bookmarks are, falling back to more general groups. Once in some groups, the browser sends a random 1% of the pages they visit to others in their group via IPFS pubsub, who then filter out the pages they've visited and presents to their user as recommendations. 

Adding a label of "people who've bookmarked similar pages recently visited:" gives a clear highlevel description of this algorithm so users can understand what's influencing them. 
