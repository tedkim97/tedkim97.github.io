---
layout: post
title: Dealing with Interactivity for Scraping
subtitle: Using Selenium to deal with a common issue when web scraping
published: true
enable_latex: false
permalink: webscraping_tips
frontpage: false
technical: true
funstuff: false
tags:
  - scrape
  - data-sci
  - db
  - programming
curated:
  - data_anonymization
  - webscraping
concepts:
  - selenium
  - javascript
  - sms
---

* TOC
{:toc}

# Introduction
The naive implementation of python web scrapers (the ones that use the [`requests` library)](https://requests.readthedocs.io/en/master/) can't always handle the "modern" web - mainly the interactions that web services expect *humans* (mainly javascript and authorization) to use[^1]. Fortunately, there are solutions! If you have issues with login credentials and page interactivity, I recommend skimming through this post.
 
[^1]: It doesn't mean that the library is useless for web scraping - it's very useful for 90% of the cases out there.

**NOTE:** This is post is a slightly modified excerpt from a bigger blog post I wrote on web scraping. I cut out this segment because a shorter post would be much more digestible for someone just looking to solve a problem. 

# Web Scraping and Javascript

A problem in scraping is that a website may involve some sort of login[^2] or interaction. The reason why this is a problem is that our naive crawler can't just make a POST request with login credentials or press a button. and expect to get back login access (it's a whole process with tokens, authorizations, etc). Moreover, modern websites don't load information on a page at once. If you're looking at a product page, you have to press a button ("more details", "load more", "expand") to display the whole information on the page.

[^2]: Although if they have a login, scraping is probably against TOS and their `robots.txt`

A hack/fix to this problem is to use [selenium](https://www.selenium.dev/)[^3] - a tool mainly used for automating browser testing - to control a browser session and scrape through that interface. Again, people primarily use Selenium for automating web testing, but we can use it to run a crawler. 

[^3]: Even though working with the Selenium Python API can be a bit clunky (long class names, importing a lot of different subpackages), the minor, unergonomic aspects are worth the flexibility.

### Selenium: Locating Elements
Below is a simple example where you locate a "load more" button by identifying the button's class name and "clicking" it through selenium. 

```python
from selenium import webdriver

button_class = 'masonry-load-more-button'
driver = webdriver.Chrome(options=browser_options)
driver.get('somewebsite.com/test')

try:
    load_button = driver.find_element_by_class_name(button_class)
    load_button.click()
    time.sleep(2) # give time for the content to load
    web_content = driver.page_source
    '''
    save web_content
    '''
except:
	'''
	NOTE: the webdriver might fail to find the button for several reasons.
	Common reasons include: 
	1) typo in the class name
	2) the Internet did not load the button in time
	3) the button does not exist on the page (ever) 
	'''
	print("failed to find button")
```
Selenium offers a way of retrieving the first example that fits the criteria (`find_element_by_class_name`), or every element that fits the criteria (`find_elements_by_class_name`). It's up to you to decide how to locate the right elements to interact with, and how to ignore the wrong buttons. 

Keep in mind there is more than one way of identifying elements in Selenium - you can also locate them by `id`, `name`, `xpath`, `link_text`, `partial_link_text`, `tag_name`, and `css_selector`. I recommend you read the [documentation](https://selenium-python.readthedocs.io/locating-elements.html) for proper usage and details. 

**REMEMBER:** Make sure you save the page source **after** you've interacted with the page and all information has been loaded.

##### Selenium Tip: Handling Exceptions
Selenium throws exceptions for situations where it can't execute an instruction. If you don't want to reset your crawler every time you run into the exceptions, you should run some try/catch clauses in your crawler. If you want to handle specific exceptions, you need to import them from selenium like: 

```python
from selenium.common.exceptions import NoSuchElementException

try:
	button_press(x)
except NoSuchElementException:
	print("could not clickbutton")
	'''
	However you handle it
	'''
except:
	print("Different exception")
	'''
	Do whatever you want
	'''
```
##### Selenium Tip: Manage Browser Version 
If you try to open a webdriver, and get an error like `...: Message: session not created: This version of ChromeDriver only supports Chrome version XYZ`. The solution is to install a compatible webdriver from the internet. There is another solution that requires no additional configuration detailed in this [stack exchange post](https://stackoverflow.com/questions/29858752/error-message-chromedriver-executable-needs-to-be-available-in-the-path/52878725#52878725) if you don't want to manage something like that.

##### Selenium Tip: Headless Mode
You'll notice that when you run the the crawler through selenium, a browser session will pop up on the screen. If you don't want this, you can hide the screen (but keep the activity) by adding a `"--headless"` option to your webdriver settings. Below is an example of how to use this feature in python. 

```python
from selenium import webdriver

browser_options = webdriver.ChromeOptions()
browser_options.add_argument('--incognito')
browser_options.add_argument("--headless");

driver = webdriver.Chrome(options=browser_options)
driver.get(url)
```

##### Selenium Tip: Detaching
I'm not sure if this works anymore, but you can detach your browser so that when it completes, the browser session stays intact. This is helpful for visually debugging.

```python
browser_options = webdriver.ChromeOptions()
browser_options.add_experimental_option("detach", True)
```

##### Selenium Tip: Implicit Waits/Waiting
Selenium offers Implicit Waits - which is a way of pausing the web driver until a certain element exists (like when a button is done loading, or if a certain tag appears). This can create a more robust scraper that's less likely to miss information. **However, I personally have never gotten Implicit Waits to work. I've looked at documentation and examples on the internet, but I have *never* gotten Implicit Waits to work for me**. I could be misunderstanding how ImplicitWaits worked, coding it incorrectly, or it's an issue with the Selenium Python binding (not likely), but I have never gotten it to work. 

I could have spent 1-2 more hours trying to solve the problem, but I just judged that it was faster to `time.sleep(5)` the driver, and move on. This approach has drawbacks. I could be wasting a lot of time waiting even though the contents of the page has loaded. In addition, the driver might not be waiting long enough (which destroys the point of the implicit waits), but in my experiences `time.sleep` has been an adequate solution. 

# Conclusion
In conclusion, we've gone over some use cases and examples of using `Selenium` to deal with interactivity in web pages. This post only goes over *some* (not all) of the potential `Selenium` has to offer when it comes to web scraping. 

I encourage readers to search for better ways of approaching this problem. I ***personally dislike*** using Selenium as a scraping method[^4], but the reality is that Selenium is **easy to learn** and **flexible**. 

If you have a particular requirements for a crawling product - Selenium is probably sub-ideal. However, spending weeks engineering better solutions is not an efficient allocation of time for a crawler that runs for an hour. 

[^4]: wait what??

# Footnotes