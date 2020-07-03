---
layout: post
title: Common Troubleshooting Tips for Scraping
published: false
permalink: /webscraping_tips
frontpage: false
technical: true
funstuff: false
---

* TOC
{:toc}

# Introduction

Shorter excerpt from a bigger webscraping article on this browser. Hopefully, this will be more helpful than the whole article. 

### Common Issues: Interactivity and JS

One common issue people face is dealing with use interactivity/extracting information from pages with javascript. Modern websites won't load all of the information on the page at once. Instead they'll display information with a button press. Another situation is that a website may involve some sort of login (although if they have a login, scraping is probably against TOS). The reason why this is a problem is because our naive crawler can't just make a POST request to press a button, or with login credentials and expect to get back login access.

This is a very complicated problem because login and javascript requires user interaction within the browser. 

An fix to this solution is to use [selenium](https://www.selenium.dev/) which is a tool for automating browser tasks. People primarily use Selenium for automate web testing, but for our purposes we can use it to run a crawler. 




### Selenium: one way to deal with interaction
Selenium offers several ways to located page elements, and you should read the [documentation](https://selenium-python.readthedocs.io/locating-elements.html) for more details. Below is an example where you locate a "load more" button by identifying the button's class name. 

Selenium offers a way of retrieving the first example that fits the criteria, or every element that fits the criteria.

It's up to you to decide how to find the right element to interact with.


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


https://github.com/pablo/huawei-modem-python-api-client


https://www.adafruit.com/category/281

https://www.adafruit.com/product/1946

https://mcsarge.blogspot.com/

# Conclusion

In conclusion, I hope this article gave you a more complete understanding of how to build a scraper, and retool it for your specific needs.

In addition, you have a nice set of advanced topics to increase the flexibility/speed of your 


# Footnotes