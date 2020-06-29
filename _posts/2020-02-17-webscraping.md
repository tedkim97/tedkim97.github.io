---
layout: post
title: Web Scraping
published: true
permalink: /webscraping
frontpage: true
technical: true
funstuff: false
---

* TOC
{:toc}

# Introduction

*Disclaimer:* This article is a written article of a presentation I gave on web scraping. 

My problem with the tutorials is that they teach you how to build a web scraper/crawler for an example site, and it works for that use case! These tutorials don't teach enough about  fundamentals or troubleshooting basic issues to let beginners solve their own problems.

As a result, I wanted to take a "medium-level" approach in this tutorial.

Such that this article doesn't exhaustively cover the topics related to web scraping, but allows someone fairly new to acquire the terminology and look around for themselves. 

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

The website/web service will have internal API's that build the website using Javascript. Obviously these API's are not made public, and reverse engineering these points can be a pain. 

When allocating time into a data-project, it's important to remember these details and not allocate too much time or energy into the crawler. Creating beautiful, reusable code is going to be hard depending on what information you're collecting.

# Building the crawler

### Setup
I use python 3.7 with `beautifulsoup4` and `requests`. You don't need these libraries to scrape! You could replace `beautifulsoup4` with regular expressions, and `requests` with Python's native HTTP library (or make your own). However using the high-level libraries make web scraping *significantly* easier.

### Debugging/Testing
It's important to realize that building a crawler is an iterative process. You should be using testing your requests and extraction methods using an interactive shell (if you can). For instance, I like testing if my code can extract the correct elements using a jupyter notebook.

### Web Scraping (Abstractly)
You can think of web scraping as a series of these steps. Implementation of these steps is important because it determines how information is collected. In addition, it's completely language-agnostic (but I highly recommend python).

1. Figure out which URLs (web addresses) to visit
- You could decide by following a pattern of urls (i.e abc.com/page=1; abc.com/page=2, etc)
- You could request a page and determine additional pages to visit from there (ex: visiting a profile and adding the user's friends to a queue) 
2. Make a request to the URL (and receive the webpage's content)
- Make a https request in your language of choice
3. Save the webpage's source (underlying HTML)
4. Parse the web page's HTML to extract desired information
5. Save the information

### Psuedocode template 
With some minor tweaks this could should work for simple sites
```python
import requests
from bs4 import BeautifulSoup

def download_urls(list_of_urls):
	for index, link in enumerate(list_of_urls):
		r = requests.get(link)
		if(r.status_code == 200):
			with open('saved{}.html'.format(index), 'wb') as f:
				f.write(r.content)
	return 

def store_data(data):
	# Store Data

def scrape_saved_html(data_dir):
	for fname in os.listdir(data_dir):
		with open(fname, 'rb') as f:
			soup = BeautifulSoup(f.read(), 'html.parser')
			# Extract Information
			store_data(extracted_info)
	return 

if __name__ == "__main__":
	pages = ['www.xyz.com/page={}'.format(i) for i in range(10)]
	download_urls(pages)
	scrape_saved_html('/')
	print("done scraping")
```
### Advisable Practices
I hesitate to say "Best" because I am not a leading practitioner on webs scraping.

##### Salted Cryptographic Hashes
If you're trying to collect some user data in a model where users interact with something, you might be tempted to save usernames 

Cryptographic hash functions are very complicated, but in summary they fulfill the following properties: 

Psuedo random (unable to derive input from output)
Low Collision 
One to One 

Cryptographic hash functions and salting are common in storing passwrods (this is why when you forget your password they can't just tell you, the company probably doesn't know it themselves).

Cryptographic functions are one-way
Adding "salt" to your anonymyzing scheme 

Although, you should note *this isn't perfect*. If someone malciious had your anonymizing scheme, they could brute force usernames from different websites and such.

##### Saving HTML
You might observe that saving HTML pages to your disk is unnecessary! A crawler could just request pages and extract data from them in memory. However, in the process of testing and debugging a crawler, an individual is going to have to repeat different variations of the code if you make mistakes. The crawler will be making a lot of repeat requests to the server as a result - which is an unnecessary cost to the developer (in addition to a potential IP ban). 

As a result, I advise saving page sources. 



### Common Issues: Overcoming Interactivity and JS
The most common issue people will face is user interactivity/extracting information from pages with javascript.

The reason why this is a problem is because uses can't just make a POST request with login credentials and expect to get back login access.

In addition, sites will use javascript to load in

### Common Issues: Rate Limits

### Common Issues: 2FA
Cellphone modules (I'm pretty sure this is what those phone spam services use)

# Web Scraping Example

# Advanced Concepts and Design

### Overcoming Javascript
Modern websites won't load all of the information on the page at once. Instead they'll load 

### Parallelization
Web scraping can benefit significantly from parallelizing the task. Having multiple scraping processes run at once is a straightforward extension with Pythons `thread` library. 

### IO Blocks
Reading from File
Bandwidth

# Understanding HTML & URLs
Now that we know the abstract steps to web scraping, the next step is to figure out *which* pages to scrape and *what* to scrape. In order to do that let's take a step 


# Rules & Regulations

### Robots.txt
Websites normally have a `robots.txt` file that indicates *who* can interact with the website and *how*. Almost every major website will have one. I will note that plenty of people ignore these files though. 

### Privacy & Ethics
I have met plenty of people that try to scrape information from social media - which is incredibly unethical. All social media sites have rules and clauses against this and you'll be violating their TOS.


### Legal Complications

The rules and regulations in web scraping are complicated - there exists lawsuits and rulings about web scraping. As of 2020, the rulings are being appealed, so I recommend that you google the current status of web scraping. 

I will say that it is generally hard to find someone running a scraper unless the crawler(s) are being deployed at a massive scale.   


# Conclusion