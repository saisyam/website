---
title: "How to scrape Google reviews using Python"
categories:
  - Python
tags:
  - Scraping
  - Splinter
  - BeautifulSoup
  - Web driver
date: 2021-08-15
image: /webcam_testing.jpg
---
This post explains how to scrape reviews from Google using Python. We use two popular Python packages used extensively for scraping, [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) and [Splinter](https://splinter.readthedocs.io/en/latest/). Though the process or the code snippets can be used to scrape any business listed in Google, but I am more interested in scraping reviews for restaurants in US. So, Let's get started.

## Identify businesses to scrape
First thing we need to do is to identity the businesses we want to scrape. As I mentioned earlier, I am interested in restaurants in US. Let's stick to restaurants in New York state. Below is the URL and the image of the business we want to scrape.
