---
layout: post
title: Hip-Hop's Diss Tracks As Graphs (/Networks)
subtitle: Analyzing beef throughout history
published: false 
enable_latex: false
enable_d3: true
permalink: diss_track_graphs
frontpage: true
technical: true
funstuff: true
tags:
  - music
  - python
  - javascript
  - programming
  - arts
  - graph-theory
  - art
  - vis
  - metrics
concepts:
  - graph theory
  - networkx
  - d3
---

# Introduction
Over the weekend, I decided to construct diss tracks as [graphs](https://en.wikipedia.org/wiki/Graph_(discrete_mathematics)) (or networks), analyze the results, and visualize them. 

Diss tracks are songs meant to verbally attack/disrespect another artist. During the song, the artists will reference past events or faults of their target. The more memorable diss tracks I remember are in hip-hop, but the archetype is not exclusive to hip hop. One of the more recent diss tracks is ["The Story Of Adidon"](https://genius.com/Pusha-t-the-story-of-adidon-lyrics) where Pusha T responds to [Drake's diss track Duppy Freestyle"](https://genius.com/Drake-duppy-freestyle-lyrics) and exposes Drake's hidden child in addition to just shitting on drake. Honestly I can't explain it all, so I recommend looking through the genius explanation. I don't think there is anything incredibly deep about their existence, but I think it's pretty fun to follow the feuds in hip hop. 

I've sourced information from this [Wikipedia article](https://en.wikipedia.org/wiki/List_of_diss_tracks) and this [Complex Top 50 List](https://www.complex.com/music/2018/10/the-50-best-hip-hop-diss-songs/2pac-hit-em-up-1996). I'll upload the json file I've compiled at some later date. Below is a sample graph generated from the dataset.

{% include caption_image.html imgpath="/figures/diss_track_graph_main.jpg" alt="fig1" caption="a noticeable cluster from the interactive d3 version of the graph" %}

Read more if you want to see an interactive version, variations, and some graph statistics.

### Disclaimer: Incomplete Dataset
The dataset I'm using to visualize this information isn't complete nor comprehensive. When I started, I *assumed* that there would be a deeply descriptive and well cited catalog of all diss tracks in history, but there isn't. Unfortunately, I don't think I have the knowledge or energy to compile a comprehensive list of notable diss tracks throughout history. 

As a result, I combined together some results from Wikipedia and complex to form a list. The graph only represents a sample of diss tracks - I'm sure there are plenty of songs out there that I've missed. I'll upload my data on a later date.

# An Interactive Visualization
The main problem I've had with visualizing graphs like these is that they're too large to display as a single image. In order to cover all of the nodes, the camera would need to be zoomed out all the way, and the labels would have to be large enough to see, but also small enough to not cover other nodes.


Even with all of the different possible graph layouts

Normally, this isn't a problem when 

Iteratively form visualizations is viable in something like a jupyter notebook, but not very viable in a blog-style content.

As a result, I decided to try creating an interactive diss track graph in d3.  

- **Hover** over a node to see the respective artists, and the artists 1st degree neighbors.
- **Double click** on a node to toggle on this highlighting. Double clicking on the node (or any other node) will turn it off. 
- **Zoom** with the scroll wheel or by double clicking
- **Pan** by holding down the mouse and dragging

{% include disstrack_vis.html %}

### Notes:
- The zooming is a bit funky, so I recommend zooming out all the way, and then zooming on a particular section you're interested in.
- Some nodes *look* like they're connected (i.e an edge goes through them), but they aren't really. You can clarify the relationship by hovering over these nodes.
- You might notice that it would be more proper for this graph to have arrow heads (indicating artist/target relationship). I agree, but creating the other d3 functionality took up too much time, so I tabled it for a later date.
- The color combo used for highlighting nodes is not super aesthetic, but this color combo attempts to be more colorblind-friendly contrasts. 

# Graph/Network Statistics:
Graph construction and analysis was all done in [`networkx`](https://networkx.org/) - a python library for working with graphs. While I haven't worked with large-scale graphs, it's a pretty great library. Jupyter notebook will be released at a future date

WHy do I include both results? It's because my page rank results for my 

## Measure(s) of Centrality
Centrality is a term used to identify the "importance" (a purposely abstract concept for now) of a network/graph.

Centrality algorithms are used to determine the importance of distinct nodes in a network. The Neo4j GDS library includes the following centrality algorithms, grouped by quality tier:

Find the most important 

https://en.wikipedia.org/wiki/Centrality

## Undirected
- Number of nodes: 143
- Number of edges: 163
- Average degree: 2.2797
- Density

## Directed
- Number of nodes: 143
- Number of edges: 181
- Average in degree: 1.2657
- Average out degree: 1.2657
- Density

### In Degree Centrality & Out Degree Centrality



### Page Rank

age rank was the algorithm that start google searches

Essentially, it's a way of "ranking" webpages thorugh their links.

If hundredes of webpages linked one webpage (say a wikipedia article) it would have a high "rank".

From my understanding, Google has modified/refined page rank even more

There is no decisive "centrality" algorithm, but pagerank is the one that google uses, so that's good enough for us







# Data Schema
When the data was collected, the data was organized in tables using "songs" (track name) as a unique identifier. Each diss track would have fields for "artist", "features", and "targets". In terms of constraints, each song has one artist\*, and can have multiple features and targets.

\* The one artist restriction was a mistake in retrospect.

### Table Schema

|track name          	| artist 			| features          |targets            |
|:--------------------: |:-----------------:|:-----------------:|:-----------------:|
| The Story of Adidon   | Pusha T   		| None              | Drake             |
| Takeover            	| Jay-Z             | None              | Prodigy, Nas      |
| Who Shot Ya?          | Biggie Smalls     | None              | 2Pac              |
| Hit 'Em Up            | 2Pac        		| The Outlawz       | Mobb Deep, Biggie Smalls, ... |
| The Invitation        | Nick Cannon       | Suge Knight, Hitman Holla, ... | Eminem |
| Duppy Freestyle       | Drake             | None              | Pusha T, Kanye West |

### Graph Schema
Translating these **records** (pun) to a graph works like this:

- Each artist, feature, and target exists as node. 
- For each song, the "artist" is dissing each of the "targets". 
- For each song, every person in the "features" is dissing each of the "targets".
-- This assumes that a feature is dissing each one of the targets, which **isn't** a fair assumption, but it makes my life a lot easier.

{% include caption_image.html imgpath="/figures/directed_fl.jpg" alt="graph_demo" caption="a graph representation of the records above (with a force layout)" %}

{% include caption_image.html imgpath="/figures/directed_cl.jpg" alt="graph_demo" caption="a graph representation of the records above (with a circular layout)" %}

### Schema Notes
**Music Producers** are excluded from this schema, but they are important.

**What happened if multiple artists make the same song rather than feature?**
Good question - this was a surprised to me as I didn't notice that ["No Frauds"](https://en.wikipedia.org/wiki/No_Frauds) was the only song in my list that was co-created by Nicki Minaj, Drake, and Lil Wayne. 

It's not correct to say that any of these artists were features because (from my understanding) they had created the song together.

The ***proper*** solution would to adjust the artist field so that it can have multiple values, or add another columns representing artist2, artist3, etc. (Something like below)

|track name          	| artist(s)  		| features          |targets            |
|:--------------------: |:-----------------:|:-----------------:|:-----------------:|
| No Frauds             | Nicki Minaj, Drake, Lil Wayne, | None              | Remy Ma           |

|track name          	| artist1  	  | artist2     | artist3     |features           |targets            |
|:--------------------: |:-----------:|:-----------:|:-----------:|:-----------------:|:-----------------:|
| No Frauds             | Nicki Minaj | Drake       | Lil Wayne   | None              | Remy Ma           |

However, it felt silly retooling my code for conversions and extractions for this one edge case, so I created three different entities for each artist. (Like below)

|track name          	| artist(s)  		| features          |targets            |
|:--------------------: |:-----------------:|:-----------------:|:-----------------:|
| No Frauds             | Nicki Minaj       | None              | Remy Ma           |
| No Frauds             | Drake             | None              | Remy Ma           |
| No Frauds             | Lil Wayne         | None              | Remy Ma           |

This solution is terrible for several reasons - you seriously shouldn't do this, but I've deemed this "okay" because I know it won't matter for the specific use-case I'm thinking about.

# Scraping & Cleaning
I collected the data by scraping these two web pages (a [Wikipedia article](https://en.wikipedia.org/wiki/List_of_diss_tracks) and a [Complex Top 50 List](https://www.complex.com/music/2018/10/the-50-best-hip-hop-diss-songs/2pac-hit-em-up-1996)) and parsing their contents into json. Parsing consisted of using `BeautifulSoup` to find the HTML tags that I needed (easy) and string splits with regular expressions to extract information (annoying). 

Afterwards, I merged the datasets and deleted any duplicates.

### Data Cleaning
Cleaning the data consisted of adjusting any failures in the extraction process, removing small typos or inconsistencies between the two sites, etc. Even though the data was clearly hosted in a table, the Wikipedia page involved a lot of cleaning because it used plain english to convey information about these songs, meaning finding consistent ways of parsing information difficult. On the other hand, cleaning the complex article was much less work. The page was clean and structured, sorting track names, features, artists, targets very clearly.

There were a lot of **little** things that I had to clean after I started building the graphs and noticing some strange quirks.

Furthermore I had to decide on exclusions of diss tracks from the WIkipedia article.

- Aliases 
2Pac as Tupac
Luther Campbell/Luke
Notorious B.I.G /Biggie Smalls

- Casing/Hyphens/Quotes
Casing/Hyphens/Quotes(is Pusha T Pusha-T or Pusha T?)
Is N.W.A N.w.A. or N.W.A
Lil Kim is Lil' Kim or Lil Kim

- Removals
"How To Rob" 50 cent - a bunch of different people
Megadeth - who are these guys?
Jermain Jackson

Zayn Maluyk diss removed
Ed Sheeran Removed

- Abstract Concepts

Abstract Concepts/Organizations (Records, Queens)
For instance "new york new york" disses al lnew york rappers. How am I supposed to work with that? - But i kept it because a follow up diss track "L.A L.A" is a diss towrads the artist

Tim Dogs's "F\*ck Compton" was labelled as dissing the gangster rap genre in addition to the N.W.A - (I removed the diss of gangster rap genre, but I kept the diss towards the N.W.A).


- Weird things

Wikipedia included Nick Cannon's "The Invitation Canceled" featuring Emniem, dissing eminem (but in reality Nick Cannon sampled an old eminem track)


### Scraping Retrospective
I really regret writing a scraper to collect the data. It would have taken an hour (at most) to just copy, paste, and clean these lists myself (Especially considering, I had to clean the end result anyways). I assumed that there would be a lot of data, so that the scraper would pay off, but there wasn't nearly enough data to justify this approach. 

{% include caption_image.html imgpath="https://imgs.xkcd.com/comics/the_general_problem.png" alt="xkcd" caption="why spend 30 minutes manually transcribing when you can spend 2 hours automating it?"%}

Instead it took me 2 minutes scraping the pages, and 2 hours finding a "clean, systematic" method of extracting data from 
the pages. At most it would have taken 30 short, but incredibly boring minutes to record this information manually


# Things that failed to make it to the cut?

- Directed edges
- Edge Hovering that indicates songs
- Edge Hovering with weights
- Better formatting
- Representing number of interactions as edge thickness

Because learning d3 can be a painful process if you don't know what you're doing, and I had other things to do :(. 

I didn't feel like going all the way to implement these things in d3

# TODOs: 
- **Focus on an individual rather than a whole scene** 
For example, there are a handful of rappers with pages dedicated to their feuds.
 Eminem has an entire page on Wikipedia dedicated to feuds -\_-

https://en.wikipedia.org/wiki/Pusha_T#Feuds
https://en.wikipedia.org/wiki/Eminem#Feuds
https://en.wikipedia.org/wiki/50_Cent#Feuds
https://en.wikipedia.org/wiki/Drake_(musician)#Feuds


- **Finish the d3 visualizations**
- **A more thorough analysis of the graphs** 
- **Genius API**
Apparently Genius has an API for music. I was thinking that the Genius API could be used to source information and assume its correct. 

I couldn't do certain things in the program because I couldn't easily determine uniqueness (so i decided not to touch it). 
Genius API might help with that