+++
title = "Python Splinter to Kickstart your Web Automation Testing"
description = "Python Splinter is new and have an easy to use API compared to Selenium. Python Splinter can also be used to scrape web content."
author = "Saisyam"
publishdate = "2020-05-09"
lastmodified = "2020-06-07"
categories = ["testing"]
tags = [
    "python splinter",
    "web automation",
]
+++

Today automation is the key for any project. Python Splinter is the new tool in Web Automation Testing space. Splinter has an easy to use API when compared to Selenium. Splinter uses Selenium web driver and built on same principles as Selenium.

## Automation Testing
Today web applications are becoming more complex with lot of features. Mobile first approach of web applications requires more testing. Automation will help in testing complex business logic and provide a quality product to the customers.

Below steps are followed while testing a webpage:

1. Start the Browser
2. Load the page into the browser
3. Navigate to appropriate element or section
4. Do Actions - Click, enter data, select etc.
5. Verify the result
6. Close the Browser

Python Splinter will help you in handling all the above activities.

## Install Python Splinter
We can install Splinter using PIP, the default package installer for Python. 

`$ pip install splinter`

I am using Python3 and created my virtual environment.

{{< adsense type="article" >}}

## Simple Program

A very simple program to get started with Python Splinter.

{{< gistify url="https://gistify.saisyam.com/https://github.com/saisyam/web-automation-testing/blob/master/simple.py" >}}


**Note:** You need to download the respective Webdrivers for the browsers and keep it in your PATH. Below are the links to Firefox and Chrome drivers.

1. Firefox - [https://github.com/mozilla/geckodriver/releases](https://github.com/mozilla/geckodriver/releases)
2. Chrome  - [https://chromedriver.chromium.org/downloads](https://chromedriver.chromium.org/downloads)

Firefox is the default browser. To select a browser your have pass it as an argument to Browser class. `browser = Browser('chrome')` will launch Chrome browser if chrome driver is available in the PATH.

## Headless Mode

When you execute the above script. Firefox or Chrome browser will open and load the page. Sometimes we need to hide the browser window. This is called `headless` mode. Though the browser is opened it will not be visible on the screen. This is very much useful in non-GUI environments like servers. To run the test cases in headless mode we need to make `headless=True` in `Browser` class. 

`browser = Browser('chrome', headless=True)`

You code snippet will look like:

{{< gistify url="https://gistify.saisyam.com/https://github.com/saisyam/web-automation-testing/blob/master/headless.py" >}}

## Testing with Proxies

Sometimes we need to test with proxies. One of our client wants to test region specific features in his web page. So we identified proxies from various countries and tested the application. Below example show how to setup a proxy for `Chrome` browser with `Webdriver Chrome options`. For full source code visit my [github](https://github.com/saisyam/web-automation-testing/).

{{< adsense type="article" >}}

{{< gistify url="https://gistify.saisyam.com/https://github.com/saisyam/web-automation-testing/blob/master/proxy_chrome.py" >}}

## Support for Flask and Django Applications

Splinter supports Django and Flask driver as well. It means you can test your Flask and Django applications. Below is a simple example with Flask.

{{< gistify url="https://gistify.saisyam.com/https://github.com/saisyam/web-automation-testing/blob/master/simple_flask_test.py" >}}

## Simple to use API to find elements

Splinter provides 7 methods to find elements in a page:

1. id - find_by_id('header')
2. css - find_by_css('p[class="ip-address"]')
3. xpath - find_by_xpath('//h1')
4. tag - find_by_tag('span')
5. name - find_by_name(name)
6. text - find_by_text("content")
7. value - find_by_value(value)

For more information refer to finding elements [documentation](https://splinter.readthedocs.io/en/latest/finding.html)

## Conclusion
We use Python Splinter extensively for web automation testing especially testing webcam applications for a client. Refer to this article for faking web cam applications. We built a Behavior Driven Development (BDD) framework around it. Very soon you will expect an article on this. Apart from test automation we use Python Splinter for scraping web to extract content.
