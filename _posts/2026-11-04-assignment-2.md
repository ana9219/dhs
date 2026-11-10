---

layout: single
title: "Assignment 2"
date: 2026-11-04
excerpt: "By Sara Alremeithi and Alyaa Alzahmi"
header:
  teaser: /assets/images/road.jpg

---

<iframe src="{{ site.baseurl }}/assets/maps/interactive_map.html"
        width="100%"
        height="600px"
        style="border:none;">
</iframe>

<iframe
  src="https://drive.google.com/file/d/1eE7pSG-tvWnXI1EYgecy-7HWye6rtyRl/preview"
  width="100%"
  height="600"
  title="1 the Road PDF">
</iframe>

## Introduction 
 1 the Road is a novel published by Ross Goodwin in 2018, created using artificial intelligence during a road trip from New York to New Orleans. Instead of writing the novel himself, Goodwin used artificial intelligence to generate text based on information collected during the journey. A camera at the back of the car captured images of the surroundings, a GPS identified nearby locations, a microphone recorded conversations inside the car, and a clock provided the time. This information was then used to generate descriptions of places, conversations, and other parts of the novel. The novel mentions many real locations and businesses, but the writing does not always follow a clear storyline. At times, it was difficult to understand how certain locations fit into the journey because the writing was often fragmented and shifted between places without clear explanation or connection. To better understand how these geographic references are interpreted, we created a three-layered map of the locations identified in the novel. We then compared the layers, two based on automated methods and one based on manual annotation.

## Annotations

### Manual Annotations
In order to create the map, we first needed to annotate the locations. As mentioned previously, this was done in 3 different ways. The first method was done during one of our classes, where the students were split into groups, and each group was responsible for annotating an assigned section of the novel using [Recogito](https://recogito.pelagios.org/).
<img src="{{ '/assets/images/recogito.png' | relative_url }}" alt="Manual annotations in Recogito" width="100%">
Our group worked on section 1; we went through the text and identified potential locations, then checked whether we could find them in Recogito’s search function, but in many cases we couldn’t. Therefore, we used [Google Maps](https://www.google.com/maps) to find them and entered their coordinates in the comments/tag section of Recogito. Even then, we faced some issues. For example, when searching Google Maps for “BP Gas Station” in Maryland, several stations in the same area appeared, making it difficult to identify which branch the text referred to.
<img src="{{ '/assets/images/bp-gas-station.png' | relative_url }}" alt="BP gas stations in Maryland" width="100%">
*Multiple BP gas stations in Maryland made it difficult to identify the location mentioned in the novel.*
 When faced with issues like these, we had to look back at the last annotated location or look at the context to be able to identify which one would be the closest. Another issue related to this is that sometimes we would find the location on Google Maps of those places that we couldn't on Recogito, but its name had changed; this made it difficult for us to decide whether to keep the annotation or not. 
These issues are relevant to the argument Kenric McDowell makes about Foursquare’s role in the novel. Foursquare is a digital platform that provides information about places, which the AI used in 1 the Road to identify nearby locations during the journey. The places that Foursquare identifies are mostly restaurants, shops, hotels, and gas stations.
He argues that the places represented in the novel were shaped by the information available through Foursquare. Similarly, the places we could identify while annotating depended on the information available through Recogito and Google Maps. Therefore, our final map was shaped not only by the places mentioned in the novel but also by the information available through the tools we used.

### Automated Annotations
The two automated methods were provided by our professor, Dr. David Wrisley. These included a spaCy/geopy approach that identified place names and looked up their coordinates, and an LLM-assisted approach that created a TEI transcription and also used spaCy and geopy to identify and locate places.
## Data cleaning
 Once the text was fully annotated, we downloaded the file in CSV format and uploaded it to Google Sheets to clean it up. Since the coordinates were put in the comments/tags section, they had to be moved to the designated latitude and longitude columns.
<img src="{{ '/assets/images/data-cleaning.png' | relative_url }}" alt="Cleaning the coordinates in Google Sheets" width="100%">

## Creating the Map
After cleaning the data, we uploaded all three datasets to Posit Cloud to create the map. We widened the map view to show locations outside the New York to New Orleans route and chose colours from [ColorBrewer](https://colorbrewer2.org/) to distinguish the three layers. We also ran the code that slightly moved overlapping points to make them easier to see, and it also added clickable popups with information about each location
After loading the datasets, we got the following results:.

Manual (Recogito): 246 rows, 245 mappable, 1 without usable coordinates
Automatic (spaCy + geopy): 341 rows, 248 mappable, 93 without usable coordinates
Automatic (LLM): 374 rows, 281 mappable, 93 without usable coordinates

Our manual dataset, which was downloaded on the 3rd of November 2026, contained 246 rows, 245 of which are mappable. The spaCy + geopy dataset contained 341 rows, but only 248 could be mapped, just three more than our manual dataset. The LLM dataset contained 374 rows, of which 281 could be mapped, 36 more than our manual dataset, and 33 more than the spaCy + geopy. Despite having more entries, both automated datasets had 93 rows without usable coordinates. 
Finally, we saved the map as an HTML and were ready to analyse it.
<iframe
  src="{{ '/assets/maps/interactive_map.html' | relative_url }}"
  width="100%"
  height="600"
  title="Three-layer geocoding map">
</iframe>

## Analysis

### Overall analysis:
Looking at the map, we noticed that the manual geocoding mostly follows the car’s route from New York to New Orleans. Most points lie along this route, with a few outliers, such as Columbia, Ashwood, Purple Heart Bridge, and Massachusetts. The route remains visible in the spaCy/geopy layer, but it is less clear because more points are scattered across the US, down to South America, Africa, and Asia. A dense cluster has been formed in Europe as well. 
In the LLM layer, similar things can be said. The route is still visible, but there are more scattered points and an even denser cluster in Europe. Overall, the LLM layer has the most plotted points, followed by spaCy/geopy, while the manual layer has the fewest. 

### Case Studies:
Looking more closely at individual locations, we noticed that the two automated methods often produced exactly the same coordinates, even when they differed significantly from our manual annotations. One of the clearest examples was Florence. In the novel, the text mentions “The Great Pee Dee River Bridge: a bridge in Florence,” which points to Florence, South Carolina. We placed Florence at 34.1954, -79.7626, while both spaCy + geopy and the LLM placed it in Florence, Italy, at 43.7698, 11.2556. Both spaCy + geopy also recognized the Great Pee Dee River Bridge as a location but were unable to assign it coordinates. The reference to the bridge provided enough geographic context to distinguish Florence, South Carolina, from Florence, Italy, yet this connection was not reflected in either automated result. 

Similarly, this disagreement also appeared with Lorton, which we manually placed in Virginia at coordinates 38.7043, -77.2278, while both spaCy + geopy and the LLM placed it in England at coordinates 54.6216, -3.3127. SpaCy + geopy also located Lorton Road in Cape Town, South Africa, even though our annotation placed it in Virginia as well. In our manual annotation, we also encountered Lorton Station Town Center, which had two possible coordinate pairs. We selected the one closer to the previously mentioned location to maintain consistency. However, neither automated method assigned coordinates to Lorton Station Town Center. This meant that several references associated with the same place in Virginia were either placed in completely different countries or left unmapped by the automated methods. Unlike Florence, where the bridge provided a direct clue about the intended location, the Lorton example shows how considering nearby references together helped us narrow down places with ambiguous names. The automated results show that these names were treated as separate geographic locations rather than connected references, which helps explain why the same area became scattered across different parts of the world. 

  
## Critical discussion - Limitations
While the previous examples show how the automated methods did not accurately identify ambiguous locations, they also made us question how reliable our own manual annotations were. Although our manual annotations were mostly concentrated around the journey from New York to New Orleans, this does not necessarily mean that our results were more accurate. We also made assumptions when choosing coordinates, especially when the text did not provide enough information. For example, when we had two possible coordinates for Violetville and Lorton Station Town Center, we chose the ones closest to the previously mentioned locations. While annotating, this seemed like the most reasonable choice because we were trying to follow the direction of the journey. However, the writing in 1 the Road does not always follow a clear geographic sequence, so the closest location does not necessarily mean it’s the correct one. This becomes even more important when considering that different students annotated different sections of the novel. Each annotator may have had a different approach when it came to ambiguous locations. As a result, the manual dataset may not have been completely consistent. Some students may have chosen a location that fits the expectation of what the road trip should look like; others may have followed a different approach or assumption. 
Moreover, automated geocoding may assign coordinates to references without verifying if they are actual locations. As an example, the LLM plotted a point for “I - 20” in Germany; we do not have enough information to know whether I-20 even refers to a place; I-20 could be an object or a code. This adds to the issues we identified earlier with the automated methods, where the coordinates appeared precise despite the uncertainty surrounding the references. 
However, interpreting geographic context was not straightforward for us either. Take the point Belgium, for example, the only point outside the United States in our manual annotations. The text describes, “Urban South Brewery: a brewery in New Orleans that the seventh surveyor had given him to Belgium and had been taken out of the place where he had been sitting in the city.” Since 1 the road was generated by AI, the sentence does not provide a clear connection between New Orleans and Belgium. We could not determine whether the text was referring to Belgium in Europe or one of the 4 “Belgium’s” in the US, or possibly to something completely different. 
Finally, another limitation is that the maps only represent the text we were given, rather than everything Wordcar originally produced. The published novel is 146 pages long, but we know that the car generated many thousands of words during the journey and that some passages were removed during publication. However, studying the maps cannot tell us which passages were removed or whether they contained geographic references. For example, the smaller number of points in the northeastern United States does not necessarily mean that Wordcar generated less writing about that region. It could reflect the number of recognizable locations in the text or potentially the decisions made during publication. Therefore, our maps are useful for comparing how different methods interpret geographic information, but they cannot fully reconstruct the original journey or explain why certain places appear more frequently than others.

## Conclusion
Ultimately, this comparison shows that computational approaches to spatial data have limitations, especially when working with a text like 1 the Road. Although automated methods can identify and map hundreds of locations much faster than manual annotation, they cannot always understand the context in which a place is mentioned. This means that the resulting maps can contain misleading geographic patterns, even when the coordinates appear precise. 

## AI 
AI has been used to embed the images and the map into the pages (it has provided the code to do that); AI has also been used to check for grammatical errors; AI has not been used to analyse or do the work for this assignment.

## Refrences
Goodwin, Ross. 1 the Road. Introduction by Kenric McDowell, Jean Boîte Éditions, 2018.
Allado-McDowell, Kenric. “AI Poetry Hits the Road.” Artists + Machine Intelligence, Medium, 14 Apr. 2017, https://medium.com/artists-and-machine-intelligence/ai-poetry-hits-the-road-eb685dfc1544.

Ready for grading!






