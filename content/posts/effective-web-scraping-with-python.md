---
title: "Effective Web Scraping with Python"
categories:
  - Python
tags:
  - Web Scraping
  - Splinter
  - BeautifulSoup
  - Requests
  - Proxies
  - Webdriver
date: 2021-01-10
images:
  - ../webscraping.jpg
---

Web scraping is a process of automatically extracting information from websites. Web scrapers are small programs written to crawl and extract specific information from a webpage. For example, getting latest news items from news websites like BBC, CNN etc.

{{< table_of_contents >}}

{{< adsense >}}

## Why Scraping?
There are millions of websites on the Internet which have information on different topics. For example, news, travel, food, finance, business, tech, entertainment, to name a few. All these websites don't have a proper way (APIs or feeds) of sharing data. So, we need scrape such websites to extract the information.

Scrapers parse the HTML elements of the page using tags, ids, class names etc to get the required content from the page. Consider the following simple HTML page:

{{< gist saisyam 9d13a4e0c16de8326b491bd89a77c963 >}}

The above HTML snippet contains tags like, `title`, `div`, `h1`, and `p`. We can extract the `div` element with `id=main` and then extract text between `h1` and `p` tags under the `div` element. We can identify specific tags using `id` or `class` or any other attributes.

## Scraping with Python
It's easy to write scrapers in Python. Python has a rich set of tools or packages to perform scraping effectively. The below code snippet parses the above HTML and extracts the information required:

{{< gist saisyam 7d19d9c6247194e9130e578e08be6336 >}}

The above code uses `Beautifulsoup` package. The response of the above function is:
```python
{
	'title': 'Sample HTML page',
	'heading': 'Scraping with Python',
	'summary': 'This article explains how web scraping is done in Python and the packages required.',
	'author': 'By Saisyam'
}
```
## Challenges while scraping
Not all web pages are so simple as shown in the above example. Below is the list of challenges that we encounter while scraping:
1. **Dynamic content** - Today most of the websites use latest JavaScript frameworks to build their sites. The content or the HTML that is rendered is generated dynamically with JavaScript. You need a real browser to load the webpage inorder to get the rendered HTML
2. **Getting blocked** - Websites will block your IP address if they see a continuous traffic from you. While developing the scraper you will continuously hit their website which makes the website think of a suspicious activity and block your IP. If possible download the static HTML page and build your scraper. Most scrapers use proxy IP addresses to solve this problem.
3. **Captcha** - Most websites will check whether you are a human or not when they detect a suspicious activity as mentioned in point **#2**. Always try to avoid this situation. Though there are captcha solving tools, they are not 100% reliable or accurate.

## Python packages for scraping
In order to parse HTML, first we need to get the HTML of the page. There are two ways in Python to get the HTML content of the page:
1. **Using `requests` package** - [Requests](https://requests.readthedocs.io/en/master/) is a Python package, HTTP library built for humans. We use `requests.get()` method to get the HTML content of a page. The advantage of this is, it is fast and simple but will not get the dynamically loaded content as it is not a browser to execute JavaScript.
2. **Using `splinter` package** - [Splinter](https://splinter.readthedocs.io/en/latest/) is a Python package used to test web applications. Splinter uses webdrivers (Chrome, Firefox etc) to load a web page and get HTML. You need to have browser application and associated webdriver to make it work. With Splinter we can get dynamically loaded content as well.

## Scraping approach
The approach that we follow for creating scrapers are:
1. Get HTML content of the website using `requests` or `splinter` based on the requirement.
2. Use `Beautifulsoup` to parse the HTML and extract the content. BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) is a Python package used to parse HTML content. `Beautifulsoup` supports multiple HTML parsers. `html5lib` is a parser built completely in Python and considered as the fastest among the existing parsers.


Getting the complete HTML content is a complex activity, because of the challenges explained in the above section. We avoid using single IP address and use proxies and rotate our IPs. There are some free proxy providers like [SSLProxy](https://sslproxies.org/), [SpysOne](https://spys.one/en/https-ssl-proxy/), [ProxyList](https://www.proxy-list.download/HTTPS) to name a few. 

If you don't want to deal with all the mess and do just parsing the content, use [ScraperAPI](https://www.scraperapi.com/). It is a proxy API for web scraping. Scraper API handles proxies, browsers, and CAPTCHAs, so you can get the HTML from any web page with a simple API call!. Check out [ScrapingBee](https://www.scrapingbee.com/) as well.

## Using proxies for IP rotation
You can tell `requests` API to use proxies to fetch html.

{{< gist saisyam 12410ff3c8c027cf0b58c903bff0d5a8 >}}

Your request will go through the proxy address provided. Use HTTP proxies to skip SSL verification step which happens in case of HTTPS proxies.

In a similar way, we can use proxies with Splinter as well. We use Chrome browser here.

{{< gist saisyam 7b8b7f4954086ae581a59b31731ec38f >}}

If one IP gets blocked, you can get an another one and continue scraping.

## Conclusion
In this article we learnt what is scraping and why it is required. We have explored few packages in Python to do scraping and identified challenges in scraping along with few solutions to overcome those challenges. For more information on scraping visit my [Scrapers](https://github.com/saisyam/scrapers) repository on Github. Happy scraping!