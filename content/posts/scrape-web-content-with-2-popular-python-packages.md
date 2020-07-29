+++
title = "Scrape web content with 2 Popular Python packages"
description = "Scrape web content with Python's extensive library of tools. Extract data from web to power your analytics algorithms. In this post we will discuss about Beautifulsoup and Splinter"
author = "Saisyam"
publishdate = "2020-05-11"
lastmodified = "2020-06-10"
categories = ["web scraping"]
tags = [
    "beautifulsoup",
    "python",
    "splinter",
    "web scraper",
]
+++

Information is wealth. Today there is tremendous amount of data online. There are thousands of crawlers scrape web daily to collect information and process them. In this article we see 2 popular tools to easily extract content from the webpages.

## Why scrape web
We have lot of data online in the form of websites. Websites include HTML content and JavaScript. We need specific parsers to parse the HTML content and extract information into meaningful buckets. For example, reviews of a product. A review contain information about review date, user, product, sentiment (whether the review is negative or positive), intent of the user and rating. The review is presented in website with design style (CSS). We need to remove all those styles, HTML tags and get the content, in this case, plain text.

## How to scrape web?
Let's look at a simple example. We will take simple review from [tripadvisor](https://www.tripadvisor.in/Restaurant_Review-g1078423-d948529-Reviews-Martin_Berasategui-Lasarte_Province_of_Guipuzcoa_Basque_Country.html). Below is the image of the review.

![Review Image](/blogimages/review.jpg)

In the above review we have the following information:

1. Date of the review - 10 March 2020
2. rating - 5 stars (as all are green)
3. User - username bornaz2018
4. User avatar - Image above the user name
5. review heading - Great!
6. review text - Best service ever. Food was ...

Now we will see the HTML content for just the username and user avatar. I am using Chrome browser to inspect element. Right click on the review and click "Inspect" in the menu. You will see the below screen. Move the mouse over the HTML Div content and you will see the content highlighted on the left as shown below:

![HTML content](/blogimages/html_content.jpg)

You see the "div" element highlighted on the right and the respective review on the left. Now we will navigate into the 'div' and get specific elements.

![Member section content](/blogimages/member_section_html.jpg)

The above screenshot shows the 'div' associated with the member section. If we copy the complete element, you get the following HTML code:

{{< gist saisyam 69c4a36559f98c93ee7ccecc581161eb >}}

{{< adsense type="article" >}}

We are interested in getting user avatar and user name. So, we need the following div sections:

{{< gist saisyam e59f69c9ae93f037988a52d6690fb6ea >}}

Now we got the HTML, it's time to extract the information from them.

## Using Beautifulsoup

We use [Beautifulsoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) Python package to scrape web page. This is the most easiest and simple way to navigate through HTML. To install packages use the following PIP command (create a virtual environment for clean code):

`$ pip3 install beautifulsoup4 lxml`

[lxml](https://lxml.de/) package is required for parsing HTML. Below is the code snippet to get the avatar image and name of the user. Using the below example we got the image as base64 encoded data and name as string.

{{< gistify url="https://gistify.saisyam.com/https://github.com/saisyam/Web-Scraping/blob/master/blog/bs4sample.py" >}}

To know more about Beautifulsoup refer to its [documentation](https://programminghistorian.org/en/lessons/intro-to-beautiful-soup) page.

## Using Splinter
We will use Splinter Python package to scrape web content. Splinter is used mostly for web automation testing. Read [my article](https://saisyam.com/web-automation-testing-with-python-splinter/) to know more about Splinter. In the below example we use Splinter to scrape the data from the web page. Using the below example we got the image as base64 encoded data and name as string.

{{< gistify url="https://gistify.saisyam.com/https://github.com/saisyam/Web-Scraping/blob/master/blog/splintersample.py" >}}

{{< adsense type="article" >}}

## Conclusion
We use Beautifulsoup and Splinter in our projects. With Splinter you can perform actions, like, clicks, scrolling etc on the webpage as you have a live browser instance. Beautifulsoup is fast when you have the raw HTML downloaded. Beautifulsoup is use with [requests](https://requests.readthedocs.io/en/master/) package to connect to the webpage.


