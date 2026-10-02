---

layout: single
title: "Assignment 1"
date: 2026-10-01
excerpt: ""
header:
  teaser: /assets/images/greece-banner.jpg

---


# Introduction

Hello everyone, and welcome to my first assignment!

For this assignment, I had the opportunity to explore a country that I am already quite familiar with, but this time from a completely different perspective: data and mapping. I lived in Greece during my middle school years, so I already knew quite a bit about the country. I was curious to see whether what I already knew about Greece would match what appeared in the GeoNames dataset.

<iframe src="{{ site.baseurl }}/assets/maps/GR_featuremap_extras.html"
        width="100%"
        height="600px"
        style="border:none;">
</iframe>

*Figure 1. Greece and the geographic features explored in this assignment.*

# GeoNames Data

To begin, I downloaded the country dataset from the GeoNames server and loaded it into Posit Cloud. At first, working with R Markdown was a little difficult. I found it hard to understand some of the information on the screen, and the code looked quite intimidating. However, as I continued experimenting with the code, testing different commands, and reading through the instructions, it gradually started to make more sense.

The Greece dataset contained **37,040 rows**.

To check the major periods for the data's updates, I asked Gemini for a way to extract this information rather than checking the dates manually. It suggested using the following code to create a histogram:

```r
hist(
  as.Date(country_data$modification_date),
  breaks = "years",
  main = "GeoNames Record Update Timeline",
  xlab = "Year of Last Modification",
  col = "#2b5c8f"
)
```

After running the code, it created the following graph:

![GeoNames Record Update Timeline]({{ site.baseurl }}/assets/images/greece-timeline.png)

*Figure 2. GeoNames record update timeline for Greece.*

As shown, record updates were almost nonexistent before 2010, but there was a significant increase in modifications that peaked between 2014 and 2016. There was also a smaller spike around 2018 and 2019. Since 2022, the number of modifications has significantly decreased to a much lower rate.

# Map Features

When it came to choosing my feature codes, I didn't really have a specific theme or idea of what I wanted the map to look like. Instead, I chose three features that stood out to me: seas, islands, and schools.

After running the code, I found that there were only eight schools in the entirety of Greece, which I knew was clearly incorrect. I couldn't even find the schools that I had attended there, so I came to the conclusion that the school data was not up to date. I therefore replaced schools with mountains.

The three original features I selected were:

* **SEA** — Sea
* **ISLS** — Islands
* **MTS** — Mountains

I was unsure about using the sea feature because the number was quite low compared to the results of the other codes. I was worried that this feature would not occur often enough to create an interesting map, but I decided to keep it anyway.

It made more sense later why the number of seas was so low. Unlike islands or mountains, seas cover much larger areas, so there are naturally fewer individual features to identify.

The results were:

* **SEA = 7**
* **ISLS = 112**
* **MTS = 48**

<iframe src="{{ site.baseurl }}/assets/maps/GR_featuremap.html"
        width="100%"
        height="600px"
        style="border:none;">
</iframe>

*Figure 3. Initial map showing seas, islands, and mountains.*

The **ISLS** feature had the highest number, which I honestly did not expect. I knew that Greece had many islands, but I thought the total would be around 50 at most. I was surprised to see that there were 112 islands. This made me realize that I didn't know Greece as much as I thought I did.

# Issues with R

While assigning colors to each code, I was having trouble because the colors were not being assigned to the correct codes.

My original intention was for the map to use:

* Tiffany for islands, which I later had to change because the color was too light
* Blue for seas
* Olive for mountains

At first, I thought the problem was simply the order in which I had selected the feature codes. However, changing the order did not solve the problem.

I then tried assigning colors directly to the individual map layers. I also checked the values of `code_1`, `code_2`, `code_3`, and `selected_codes` to make sure that the codes themselves were correct, and they were.

I then used ChatGPT to help me figure out the issue. It told me to run the following code to see what color the palette was actually assigning to each feature:

```r
pal("ISLS")
pal("SEA")
pal("MTS")
```

The results were:

```text
ISLS → "#B0E0E6"
SEA  → "#808000"
MTS  → "#007FFF"
```

This showed me that the colors were being assigned differently from what I intended. The island color was correct, but the colors for sea and mountains were switched.

After troubleshooting with AI, I discovered that the issue was related to how the domain interpreted the color palette. I had assumed the code order determined the colors, but `colorFactor()` sorts them alphabetically: **ISLS, MTS, SEA**.

I fixed the problem by rearranging the codes in the domain so the correct colors matched each code.

Fixing the colors created a new problem with the map legend. I wanted the legend to display as:

**ISLS → SEA → MTS**

but it was showing:

**ISLS → MTS → SEA**

To get around this, I renamed my layer groups with numbers:

```r
group = "1_ISLS"
group = "2_SEA"
group = "3_MTS"
```

I then updated my `addLayersControl()` function:

```r
addLayersControl(
  overlayGroups = c("1_ISLS", "2_SEA", "3_MTS"),
  options = layersControlOptions(collapsed = FALSE)
)
```

*Note: I later added more features, so the legend changed entirely.*

I found this part of the assignment frustrating at first, but eventually I began to enjoy the process. I realized that there are often multiple ways of approaching a coding problem. There is not always one single correct solution. I could test different approaches, see what happened, identify what was actually causing the problem, and then adjust the code.

# Additional Map Features

After getting my original three features working, I experimented with adding additional features to the map.

I added forests using the feature code **FRST**, and I also experimented with:

* **HSTS** — Historical Site
* **HTL** — Hotel

<iframe src="{{ site.baseurl }}/assets/maps/GR_featuremap_extras.html"
        width="100%"
        height="600px"
        style="border:none;">
</iframe>

*Figure 4. Updated Greece map with additional GeoNames features.*

Adding the forest feature created another problem with the colors because the color assignments became mixed up again. This time, I used Claude to help identify the problem.

The issue was again related to how the color palette's domain was being interpreted. The codes were not being matched to colors according to the order I had originally written them. After lots of trial and error with Claude, I was eventually able to resolve the issue.

A minor issue I encountered was related to the map tiles. The CartoDB basemap was not working correctly with my map, so I initially switched to an Esri basemap.

Later, I found a Reddit post that led me to Mapbox. After creating a profile on Mapbox, I was provided with a public access token, which I was able to use for the Mapbox tile layer. ChatGPT helped me integrate the layer by providing me with the necessary code.

# Findings

## Tourism and Hotels

Other than noticing that there were only eight schools in Greece and being surprised by the number of islands, I found a lot of interesting information by observing the map.

One of the main patterns I noticed was how much more visible tourism was compared to other aspects of Greece.

Firstly, there were a lot more hotels along the shoreline than in the middle of the country. When I toggled on both the islands and hotels, this pattern became even more noticeable.

Hotels are spread across Greece, but there appears to be a much higher concentration on the islands and in popular tourist destinations such as **Corfu, Mykonos, Santorini, Zakynthos, and central Athens**.

<img src="{{ '/assets/images/Hotels_and_Islands.png' | relative_url }}"
     alt="Hotels and islands across Greece"
     width="100%">
     
*Figure 5. Hotels and islands across Greece, showing where tourism is more concentrated.*

## Missing Forests

However, when I looked at the other layers, I began to notice what seemed to be missing.

There were only four forests listed:

* Strofylia Forest
* Dásos Schiniá
* Petrified Forest of Lesbos
* Tsamliki Forest

I couldn't find Tsamliki Forest online, and after a quick search, I found that there are many other forests that are missing from the data as well.

I would attribute this to the difficulty of putting an exact number on how many forests there are. It can be difficult to establish what qualifies as a forest, or perhaps the difference is related to the priorities behind which types of data are chosen to be documented.

## Missing Mountains

This brings me to the next point: the mountain data also seemed incomplete.

Although the map's topography allows me to roughly see where the mountains are, the marked mountain points only show some of them.

It is also possible that one point represents an entire mountain range rather than a single mountain.

## Historical Sites

The same issue appears once again with the historical sites.

In the whole of Athens, only one site is listed: the Ancient Agora of Athens.

I was surprised to see the Acropolis missing, so I checked the data and saw that it was listed as *HLL*. After checking GeoNames, I found that it was classified as a hill rather than a historical site.

This was another example of how the classification system can affect what appears on a map.

# Do Maps Lie?

The video *Do Maps Lie?* states that maps can mislead due to honest mistakes by the author or because they can be designed to mislead us on purpose.

I believe that if someone who had never been to Greece looked at this dataset, they might think the country is almost entirely made of beach resort hotels, with practically no ancient history or forests.

Although this dataset is misleading, I doubt that it was intentional.

The map is showing information that exists in the dataset, but that information does not necessarily represent everything that exists in the real world.

# Data Is Not Neutral

As Kitchin and Lauriault explain, data is not simply a collection of neutral, raw facts. Data is shaped by the technologies, organizations, institutions, economic interests, and practices involved in producing and maintaining it.

GeoNames, for example, aggregates information from more than 100 different sources and combines government and geographic databases with other sources, including hotel websites. It also allows users to add and correct information.

This helped me understand why some features appeared much more frequently than others.

Hotel chains and booking sites constantly publish and update their locations because there is an economic incentive to be visible online. On the other hand, public forests and ancient ruins don't have marketing teams uploading their coordinates.

The map doesn't necessarily mislead us because someone wanted to trick us. Instead, it can be misleading because some types of data are constantly updated while other types of information can be left behind.

# Conclusion

Creating my map made me realize that maps are not a complete mirror of reality. A map is a representation shaped by the data that is available.

In the case of Greece, tourism becomes particularly visible, while other aspects of Greece, such as its forests, mountains, and historical sites, are less visible.

The map therefore does not necessarily tell us what Greece is and what it has. It tells us what this particular dataset allows us to see.

If the data were more completely documented, we would actually see that there are hundreds of historical sites in Greece, thousands of schools, and many more hotels and forests.

This assignment also changed how I think about maps. Before working with the GeoNames dataset, I mostly thought of a map as something that showed us where things were. Now I understand that maps also depend on decisions about what gets recorded, how things are classified, and how often the information is updated.

# References
Anthropic. Claude. Anthropic, https://claude.ai/.

GeoNames. “About GeoNames.” GeoNames, https://www.geonames.org/about.html.

Google. Gemini. Google, https://gemini.google.com/.

Kitchin, Rob, and Tracey P. Lauriault. “Towards Critical Data Studies: Charting and Unpacking Data Assemblages and Their Work.” The Programmable City Working Paper 2, 2014, https://mural.maynoothuniversity.ie/id/eprint/5683/.

Mapbox. Mapbox Documentation. Mapbox, https://docs.mapbox.com/.

OpenAI. ChatGPT. OpenAI, https://chatgpt.com/.

Sheffield Methods Institute. “Do Maps Lie? – Why Numbers Matter, Episode 2.” YouTube, 3 Feb. 2017, https://www.youtube.com/watch?v=G0_MBrJnRq0.

Ready for Grading!
