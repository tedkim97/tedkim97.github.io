---
layout: post
title: Dealing with Interactivity for Scraping
published: true
permalink: /webscraping_tips
frontpage: false
technical: true
funstuff: false
---

* TOC
{:toc}

# Introduction
The naive implementation of web scrapers can't handle the "modern" web - mainly the interactions that web services expect *humans* to use. Luckily, there are solutions to deal with this. If you have issues with login credentials and page interactivity, I recommend skimming through this.

**NOTE:** This is post is a slightly modified excerpt from a bigger blog post I wrote on web scraping. I cut out this segment because a shorter post would be much more digestible for someone just looking to solve a problem. 

# Web Scraping and Javascript

A problem in scraping is that a website may involve some sort of login[^1]. The reason why this is a problem is that our naive crawler can't just make a POST request with login credentials or press a button. and expect to get back login access (it's a whole process with tokens, authorizations, etc).

[^1]: Although if they have a login, scraping is probably against TOS and their `robots.txt`

Moreover, modern websites don't load information on a page at once. If you're looking at a product page, you have to press a button ("more details", "load more", "expand") to display the whole information on the page.

A fix to this problem is to use [selenium](https://www.selenium.dev/)[^2] - a tool for automating browser tasks - to control a browser section and scrape through the human computer interface. People primarily use Selenium for automating web testing, but we can use it to run a crawler. 

[^2]: Even though working with the Selenium Python API can be a bit clunky (long class names, importing a lot of different subpackages), the minor, unergonomic aspects are worth the flexibility.

### Selenium
Selenium offers several ways to located page elements, and you should read the [documentation](https://selenium-python.readthedocs.io/locating-elements.html) for more details. Below is an example where you locate a "load more" button by identifying the button's class name. 

```python
from selenium import webdriver

button_class = 'masonry-load-more-button'
driver = webdriver.Chrome(options=browser_options)
driver.get('somewebsite.com/test')

try:
    load_button = driver.find_element_by_class_name(button_class)
    load_button.click()
    time.sleep(2) # give the content time to load
    '''
    scrape content
    '''
except:
	print("failed to find button")
```
Selenium offers a way of retrieving the first example that fits the criteria (`find_element_by_class_name`), or every element that fits the criteria (`find_elements_by_class_name`). It's up to you to decide how to locate the right elements to interact with, and how to ignore the wrong buttons. 

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

# Footnotes