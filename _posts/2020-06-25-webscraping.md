---
layout: post
title: Web Scraping
published: true
permalink: /webscraping
frontpage: true
---

* TOC
{:toc}

# Preamble

This article is a written article of a presentation I gave on web scraping. 

This article takes a "medium-level" approach to web scraping. Meaning that it's not so "high-level" where complications are abstracted, and the activity is not enriching, but also not so "low-level" where the activity is excessively time-consuming or frustrating. The target audience should be familiar with programming, but an expert may not learn anything new. 

# Understanding Web Scraping

Web scraping is the process of automatically extracting information from the web. Bots and crawlers leverage the structure of websites (html & js) and their addresses (url) to extract data quickly and consistently.

### Why should I scrape from the web?
Situations where you would like to scrape from the web are rather niche; however, if you need: 
- A new idea involving predictions on some user behavior
- Novel data for an original research project
- Something new from Kaggle datasets for your ***sick*** machine learning skills


### Use Cases of Web Scraping
The data you need may exist - whether it's from tech companies (Google, Facebook, etc) or data vendors (Bloomberg, Equifax, etc). The problem is that the capital or security clearance needed isn't worth it for a hobby or experiment.

No access to official data or APIs (ex: scrape products and their information from various retailers) 

![fig1](/figures/webscraping_fig1.png){: .center-image }


Need to find niche data that might not exist yet (ex: dataset on all the houses for sale in Hyde Park)![fig2](figures/webscraping_fig2.png){: .center-image }

### Limitations of Web scraping
Building a crawler won't be able to solve all of your data-collection needs.

Scrapers/crawlers can be highly specialized and are finicky as a result.
1. Websites can change their front end that make it difficult to reuse specific scraping modules
2. Websites/services actively implement measures to prevent scraping. Social media sites impose a limit on the number of different profiles you can view in an hour (try it out yourself!).

The website/webservice will have internal API's that build the website using Javascript. Obviously these API's are not made public, and reverse engineering these points can be a pain. 

When allocating time into a data-project, it's important to remember these details and not allocate too much time or energy into the crawler.

### Web Scraping (Abstractly)
You can think of web scraping as a series of these steps. Implementation of these steps is important because it determines how information is collected. 

1. Figure out which URLs (web addresses) to visit
- You could decide by 
- You could also land on a page 
2. Make a request to the URL (and receive the webpage's content)
3. Save the webpage's source (underlying HTML)
4. Parse the webpage's HTML to extract desired information
5. Save the information


# Understanding HTML & URLs

# Rules & Regulations

### Legal Complications

The rules and regulations in web scraping are complicated


### Robots.txt

Websites normally have a `robots.txt` file that indicates who and what can do on a website. 



# Setting Up

# Web Scraping Example

# Advanced Concepts and Design


# Testing

aosdijaoisdjdjoiaj
iqwuehriuh
iowerjoweirwoierj