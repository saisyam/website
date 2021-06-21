---
title: "Run Python Scraper as Google Cloud Function"
categories:
  - Python
  - Cloud
tags:
  - Web Scraping
  - BeautifulSoup
  - Cloud function
  - Google Cloud
date: 2021-03-05
images:
  - ../python_google_cloud_function.jpg
---

Web scrapers run on a regular basis to extract information. Today every application irrespective of size is deployed on Cloud providers like AWS, Google Cloud and Azure. In this article we will see how to deploy our Ebay product scraper as Google cloud function.

{{< table_of_contents >}}

{{< adsense >}}

## Why Cloud?
Today cloud providers offers more services than simply providing machines or infrastructure. The main reason for going to cloud deployments are auto scaling, monitoring, ease of deployment, faster delivery process, wide range of infrastructure options suitable for different types of applications and ofcourse cost. Cloud deployments show considerable cost saving when compared to managing own infrastructure. More details about cloud is out of scope of this article.

## What are Google Cloud Functions?

> Cloud Functions allows you to trigger your code from Google Cloud, Firebase, and Google Assistant, or call it directly from any web, mobile, or backend application via HTTP. You are only billed for your function's execution time, metered to the nearest 100 milliseconds.

You pay for what you use. In this article we will deploy a simple ebay product scraper as a Google Cloud function.
Our Ebay scraper is built using `BeautifulSoup` library. This scraper will extract the necessary information of a product given its URL. For example, given this URL [https://www.ebay.com/itm/203449771112?hash=item2f5e8d2468:g:0zcAAOSwssFgnD-K&var=503791306328](https://www.ebay.com/itm/203449771112?hash=item2f5e8d2468:g:0zcAAOSwssFgnD-K&var=503791306328) to the scraper we will get the following result:


{{< gist saisyam a3a963c81275ff964215c629baa1c17d >}}


Refer to my [ebay.py](https://github.com/saisyam/scraper-on-cloud/blob/main/ebay-scraper/ebay.py) file for the scraper.

{{< adsense >}}

## Deploy scraper as Google Cloud Function
To deploy our scraper we need to create a file called `main.py` in the same folder where we have our scraper. Add the following code to `main.py`:

{{< gist saisyam a8815d133966bb8840639f5987778c5c >}}

`scrape_ebay` is the Cloud function that we call using a HTTP request. We pass URL, in our case Ebay product URL as a query parameter. We will see the response as json. To deploy the cloud function you need `gcloud` application on your system. You have to create a project on Google cloud and initialize `gcloud` to use that project to deploy this cloud function. Use the below command to deploy:

{{< gist saisyam e045609ad1ce7460455edeb222bc2a6c >}}

If the above command is successful, you will get the URL to run your cloud function. Something like this:

{{< gist saisyam 91f89c51a8213487b899235d528edcca >}}

You can add the Ebay URL as query parameter:

{{< gist saisyam e9d13b4dbc441df53fb61b7e9ac061c2 >}}


If you see any errors fix them. To run cloud functions two basic things are important:
1. Enable billing on your project
2. Enable Cloud Build API

{{< adsense >}}

## Conclusion
You will have [more options](https://cloud.google.com/sdk/gcloud/reference/functions/deploy) during the deployment. For example, you can set the memory required for your function to run. The complete source code of the project is available [here](https://github.com/saisyam/scraper-on-cloud/). The major advantage of Google cloud functions when compared to AWS Lambda is, Google cloud functions respect `requirements.txt` file to install dependencies, where as in AWS Lambda you need to bundle all your dependencies. Thanks for reading.
