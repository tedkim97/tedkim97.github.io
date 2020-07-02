---
layout: post
title: A (more) in-depth guide to Web Scraping
published: true
permalink: /webscraping
frontpage: true
technical: true
funstuff: false
---

* TOC
{:toc}

# Introduction

My problem with popular tutorials on web scraping is that they teach you how to scrape for an example site! While it does its job and might work for simple cases, these tutorials don't teach enough about the fundamentals of the web or troubleshooting basic issues to let beginners extend their crawlers.

As a result, I wanted to take a "medium-level" approach in this tutorial. This article won't cover every topic tangential to web scraping, but allows a beginner to acquire terminology and concepts to be able to research and learn for themselves. This means this article won't be so "high-level" where every complication is abstracted, making the activity not enriching, but also not so "low-level" where the activity is excessively time-consuming and slow. 

**How to use**: You don't have to read the whole thing - this blog post was a lot longer than I intended it to be! Just read the parts that are applicable to you. In addition, I always recommend doing additional research!

# Understanding Web Scraping

Web scraping is the process of automatically extracting information from the web. Bots and crawlers leverage the structure of websites (html & js) and their addresses (url) to extract data automatically and consistently.

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
Building a crawler can't solve all of your data-collection needs.

Scrapers/crawlers can be highly specialized and finicky as a result.
1. Websites can change their front end that make it difficult to reuse homemade scraping functions 
2. Websites/services actively implement measures to prevent scraping. Social media sites impose a limit on the number of different profiles you can view in an hour.

When allocating time into a data-project, it's important to remember these details and not allocate too much time or energy into the crawler. Creating beautiful, reusable code might not be economic depending on your use case. Don't spend hours programming something that will only run once (and for a short time).

# Building the crawler

### Setup
I use python 3.7 with `beautifulsoup4` and `requests`. You don't need these libraries to scrape! You could replace `beautifulsoup4` with regular expressions, and `requests` with Python's native HTTP library (or make your own). However using high-level libraries make web scraping *significantly* easier (remember where you want to be spending the majority of your time).

Building a crawler is an iterative process - testing your requests and extraction methods using an interactive shell allows you to prototype a crawler much more quickly. For instance, I like testing my crawler's accuracy using a jupyter notebook.

### Web Scraping (Abstractly)
You can think of web scraping as a series of these steps. Implementation of these steps is important because it determines how information is collected. In addition, it's completely language-agnostic (but I highly recommend python).

1. Figure out which URLs (web addresses) to visit
- You could decide by following a pattern of urls (i.e xyz.com/page=1; xyz.com/page=2, etc)
- You could request a page and determine additional pages to visit from there (ex: visiting a profile and adding the user's friends to a queue) 
2. Make a request to the URL (and receive the webpage's content)
- Make a https request in your language of choice
3. Save the webpage's source (underlying HTML)
4. Parse the web page's HTML to extract desired information
5. Save the structured data

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
I hesitate to say "best" (which is why I don't) because I am not a leading practitioner on scraping/crawling. However, I think that some of these practices below should be followed.

##### Save HTML
When you're scraping, store a copy of the webpages (the HTML) onto whatever device you store data.

You might observe that saving HTML pages to your disk is unnecessary! A crawler could just request pages, keep them in memory, and extract data. However, in the process of testing and debugging a crawler, someone might execute different variations of the code to fix any problems they have. If you do this without a local copy of the webpages, the crawler will make a lot of repeat requests to the host as a result - which is an unnecessary cost to the host, waste of your bandwidth, and may result in an IP ban. In addition, storing the source pages will allow you to look for any mistakes and **validate the correctness** of your data. Optimizing for write endurance on your drives it just not worth it. 

##### Anonymize/Pseudonymize Data 
Anonymize your data to protect people's privacy. In addition, make sure you anonymize your data in a **meaningful way**. If you Anonymize data with a lookup table, you can recover the person's ID by a reverse lookup - you aren't protecting anyone's identity. 

However, if you need to be able to identify behaviors over time, submissions by an individual, or a group of individuals deleting the ID may not be helpful. There are two approaches: anonymization and pseudonymization. Anonymization prevents re-identification of a data point that can't be used to identify anyone while Pseudonymization allows the data point to be **recovered given additional information**.

**Anonymization: Deleting the ID**:
Just deleting the ID of the data point is the best way of maintaining anonymity. 

**Pseudonymization: Salted Cryptographic Hashes**:
Disclaimer: this isn't a perfect solution! There are plenty of articles that point out the problems with this approach - mainly that it can't *truly* anonymize data

Cryptographic hashing and salting are common practice in storing passwords. This is why a company can't tell you what your password is when you forget - the company doesn't know it themselves, just the salted hash of your password.

An added benefit is that a cryptographic hash can take any length input and outputs a fixed size hash. 

We will abstract the engineering of cryptographic hash functions and just look at some of their properties and why they might be helpful for our use case. A good cryptographic hash function should be: 
- **Deterministic** : Hashing some value with the hash function will always be the same w.r.t the value. (`SHA256('abc') == SHA256('abc')`)
- **(Practically) Collision Free** : Practically speaking, (good) cryptographic functions are injective, meaning you won't find two inputs that result in the same outputted hash (or a "collision"). Collisions are *possible* (making them not *truly* injective), but very unlikely. Refer to [this stack exchange post](https://stackoverflow.com/questions/4014090/is-it-safe-to-ignore-the-possibility-of-sha-collisions-in-practice#:~:text=For%20instance%2C%20with%20SHA%2D256,second%20to%20about%2010%2D15.) if you want an idea of how unlikely. 
- **Easy to compute**: 
- **One-way** : This means that it should be difficult to find the inverse of `hash(x)`. Assuming you're using a popular hashing method, this should be given. If you could quickly invert SHA-256 you'd rule the world.

However, naked hash functions by themselves are not good at anonymizing data.

It's not perfect - there are plenty of research detailing the flaws of this approach. 

For example this paper "Introduction To The Hash Function as a Personal Data Pseudonymisation Technique" lists some strengths and weaknesses of using hasing as a pseudonymization schemes[^1].

[^1]: https://edps.europa.eu/sites/edp/files/publication/19-10-30_aepd-edps_paper_hash_final_en.pdf


When implementing anonymization, I recommend using a library (like `pycryptodome`).  



However, regular cryptographic hashes are not enough. Anyone with the same cryptopgraphic hash function could guess very commonly used usernames from other popular websites (if people re-use passwords, they definitely re-use usernames). 

Although, you should note *this isn't perfect*. If someone malicious had your anonymizing scheme, they could brute force usernames from different websites and such.

##### Don't Be Bad
If you're trying to use web scraping to detect botting or [astroturfing](https://en.wikipedia.org/wiki/Astroturfing#:~:text=Astroturfing%20is%20the%20practice%20of,is%20supported%20by%20grassroots%20participants.), I encourage you to do that! However, don't do it to bad things like sell people's information or undermine democracy.

### Common Issues: Interactivity and JS

One common issue people face is dealing with use interactivity/extracting information from pages with javascript. Modern websites won't load all of the information on the page at once. Instead they'll display information with a button press. Another situation is that a website may involve some sort of login (although if they have a login, scraping is probably against TOS). The reason why this is a problem is because our naive crawler can't just make a POST request to press a button, or with login credentials and expect to get back login access.

This is a very complicated problem because login and javascript requires user interaction within the browser. 

An fix to this solution is to use [selenium](https://www.selenium.dev/) which is a tool for automating browser tasks. People primarily use Selenium for automate web testing, but for our purposes we can use it to run a crawler. 


### Selenium: one way to deal with interaction
Selenium offers several ways to located page elements, and you should read the [documentation](https://selenium-python.readthedocs.io/locating-elements.html) for more details. Below is an example where you locate a "load more" button by identifying the button's class name. 

Selenium offers a way of retrieving the first example that fits the criteria, or every element that fits the criteria.

It's up to you to decide how to find the right element to interac twith.


```python
from selenium import webdriver

button_class = 'masonry-load-more-button'
driver = webdriver.Chrome(options=browser_options)
driver.get(SOMEURL)

try:
    load_button = driver.find_element_by_class_name(button_class)
    load_button.click()
    time.sleep(2)
    # Scrape contents after loading more
except:
	print("failed to find button")
```

For example, you can find them by 

Even though working with the Selenium Python API can be a bit clunky (importing a lot of different subpackages), the unergonomic aspects are worth the power you can do

##### Selenium Tip: Handling Exceptions
Selenium throws exceptiosn - if you don't want to reset your crawler everytime you run into the exceptions, you should run some try/catch clauses n your crawler.

Keep in mind, that if you want to work with specific exceptions, you need to import them from selenium
```python
from selenium.common.exceptions import NoSuchElementException
```

##### Selenium Tip: Manage Browser Version 

Updating your local version of your chrome/firefox browser can interfere with Selenium, giving you a common excpetion
like `blah blah: Message: session not created: This version of ChromeDriver only supports Chrome version XYZ`. The solution is to install a compatible webdriver from the internet.

There is another solution (although I am not a fan of this approach) detailed in this [stack exchange post](https://stackoverflow.com/questions/29858752/error-message-chromedriver-executable-needs-to-be-available-in-the-path/52878725#52878725).

##### Selenium Tip: Headless Mode

##### Selenium Tip: Detaching
I'm not sure if this works anymore, but you can detach your browser so that when it completes, the browser session stays intact. This is helpful for visually debugging.

`# browser_options.add_experimental_option("detach", True)`

```python
browser_options = webdriver.ChromeOptions()
browser_options.add_argument('--incognito')
# browser_options.add_experimental_option("detach", True)
browser_options.add_argument("--headless");
```

##### Selenium Tip: Implicit Waits
you might notice that Selenium has Implicit Waits - which will pause the webdriver until a certain element loads - allowing you to load the information, and then scrape.

This brilliant and you should use this feature! However, I personally have never gotten Implicit Waits to work. I've looked at documentation and examples on the internet, but I have *never* gotten Implicit Waits to work properly.

I could be misunderstanding how ImplicitWaits worked, coding it incorrectly, or maybe it's an issue with the Selenium Python binding, but I have never gotten it to work. 

Maybe I could have spent 1-2 more hours trying to solve the problem, but I just judged that it was faster to `time.sleep(5)` the driver, and move on.

This approach as obvious drawbacks. I could be wasting a lot of time waiting even though the contents of the page has loaded. In addition, the driver might not be waiting enough (which destroys the point of the implicit waits), but in my experiences `time.sleep` has been an adequate solution. 



### Common Issues: Rate Limits

### Common Issues: 2FA
Cellphone modules (I'm pretty sure this is what those phone spam services use)

Online phone number services, some of these services exist - I'm not sure well they work as they're usually behind a paywall.

Free services (google voice, skype) does not work incredibly well. These companies won't allow you to register a phone number with these services.

# Web Scraping Example

# Advanced Concepts and Design

### Parallelization
Web scraping can benefit significantly from parallelizing the task. Having multiple scraping processes run at once is a straightforward extension with Pythons `thread` library. 

### IO Blocks
Reading from File

Just remember adding more threads can increase everything. Remember that when you're reading from a 
Hard Disk Drive, you could (potentially) be limited by how fast the disk spins on your HDD!

Bandwidth
In addition, network bandwidth can be a limitation. 

# Understanding HTML & URLs
Now that we know the abstract steps to web scraping, the next step is to figure out *how* to scrape and *which* pages to scrape. 

In order to do that, we need a basic understanding of how the web works. 

On a high level, HTML represents the content of the page while a URL indicates it's location.

A website will have well structured pages (HTML) and well structured locations (pages). 

Using our understanding of HTML, we can exploit the structure of websites to make parsing our desired data simple and predictable (the how), and our understanding of URLs to know which pages to go to (the which). 

However, our understanding of HTML can help us puzzle together how to process pages 
The process of extracting data will ultimately be an iterative process


### HTML?

HTML (Hypertext Markup Language) is the standard markup language for information on the web. HTML (along with CSS, JS, and more) is responsible for building all the pretty websites on the web. Websites will have an underlying HTML source that instructs your browser to display content properly.

![fig1](/figures/webscraping_fig3.jpg){: .center-image }
<center> <font size="2"> <i>
A webpages content in browser (top) and its underlying source (bottom). Not pictured: CSS.
</i> </font> </center>  
<br> 


Having a basic understanding of HTML allows us to exploit the structure of a webpage and make parsing data simple and 
predictable. Different websites will have different structures, and the more fluent we are the faster we build the crawler. 

## URL?

Even though they might not always be intelligible to us (i.e amazon.com/soap/dp/B07M63H81H?pf_rd_r=7NDCD524VXDHCMBVQGS9&pf_rd_p=8f819d9f-3f10-459c-877d-64ffa525d3b4&pd_rd_r=b5353e1b-cc55-40e2-8050-4105565358f1&pd_rd_w=LaiQz&pd_rd_wg=NCNvQ&ref_=pd_gw_bia_d0).

### What i


The website/web service will have internal API's that build the website using Javascript. These API's are not made public, and reverse engineering these points can be a pain. 

# Rules & Regulations

### Robots.txt
Websites normally have a `robots.txt` file that indicates *who* can interact with the website and *how*. Almost every major website will have one. I will note that plenty of people ignore these files though. 

### Privacy & Ethics
I have met plenty of people that try to scrape information from social media - which is incredibly unethical. All social media sites have rules and clauses against this and you'll be violating their TOS.

### Legal Complications
The rules and regulations in web scraping are complicated - there exists lawsuits and rulings about web scraping. As of 2020, the rulings are being appealed, so I recommend googling whether web scraping is punishable by death. On another note, I will say that it can be hard to find someone running a crawler unless it is being deployed on a massive scale.

# Alternatives from Scratch?

There are plenty of services/platforms that can abstract a lot of the work in web scraping. I don't have any personal experiences with them, but I feel like I should let readers know they exist. [Octoparse](https://www.octoparse.com/) and [Scrapy](https://scrapy.org/) are some that I've heard about.

# Conclusion

In conclusion, I hope this article gave you a more complete understanding of how to build a scraper, and retool it for your specific needs.

In addition, you have a nice set of advanced topics to increase the flexibility/speed of your 


# Footnotes