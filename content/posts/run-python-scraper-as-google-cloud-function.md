---
title: "Run Python Scraper as Google Cloud Function"
categories:
  - Python
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

```json
{
	"title": "Travis Scott Dunks Low",
	"ebay_id": "203449771112",
	"images": ["https://i.ebayimg.com/images/g/0zcAAOSwssFgnD-K/s-l1600.png", "https://i.ebayimg.com/images/g/kZ4AAOSwUKBgnD-K/s-l1600.png"],
	"metadata": {
		"price": "US $220.00",
		"shipping": "FREE",
		"Condition": "A brand-new, unused, and unworn item (including handmade items) in the original packaging (such as",
		"UK Shoe Size": "UK 11",
		"Customized": "No",
		"Features": "Limited Edition, Comfort, Cushioned",
		"Model": "Travis Scott Dunks",
		"Department": "Men",
		"Product Line": "Nike Dunk Low, Nike SB, Nike Dunk",
		"Pattern": "Solid",
		"AU Shoe Size": "AU 11.5",
		"EU Shoe Size": "EUR 42.5",
		"Performance/Activity": "Skateboarding",
		"US Shoe Size": "10",
		"Shoe Width": "Standard",
		"Insole Material": "Fabric",
		"Brand": "Nike",
		"Shoe Shaft Style": "Low Top",
		"Style": "Sneaker",
		"Vintage": "No",
		"Color": "Beige",
		"Upper Material": "Leather",
		"Theme": "Retro",
		"Country/Region of Manufacture": "Singapore",
		"Character Family": "Friends",
		"Year Manufactured": "2020",
		"Type": "Athletic"
	},
	"url": "https://www.ebay.com/itm/203449771112?hash=item2f5e8d2468:g:0zcAAOSwssFgnD-K&var=503791306328"
}
```
Refer to my [ebay.py](https://github.com/saisyam/scraper-on-cloud/blob/main/ebay-scraper/ebay.py) file for the scraper.

{{< adsense >}}

## Deploy scraper as Google Cloud Function
To deploy our scraper we need to create a file called `main.py` in the same folder where we have our scraper. Add the following code to `main.py`:

```python
def scrape_ebay(request):
    """HTTP Cloud Function.
    Args:
        request (flask.Request): The request object.
        <https://flask.palletsprojects.com/en/1.1.x/api/#incoming-request-data>
    Returns:
        The response text, or any set of values that can be turned into a
        Response object using `make_response`
        <https://flask.palletsprojects.com/en/1.1.x/api/#flask.make_response>.
    Note:
        For more information on how Flask integrates with Cloud
        Functions, see the `Writing HTTP functions` page.
        <https://cloud.google.com/functions/docs/writing/http#http_frameworks>
    """
    url = request.args.get('url')
    return scrape_ebay_product(url)
```
`scrape_ebay` is the Cloud function that we call using a HTTP request. We pass URL, in our case Ebay product URL as a query parameter. We will see the response as json. To deploy the cloud function you need `gcloud` application on your system. You have to create a project on Google cloud and initialize `gcloud` to use that project to deploy this cloud function. Use the below command to deploy:

```shell
gcloud functions deploy scrape_ebay --runtime python38 --trigger-http --allow-unauthenticated
``` 
If the above command is successful, you will get the URL to run your cloud function. Something like this:
```
https://<region-project>.cloudfunctions.net/scrape_ebay
```
You can add the Ebay URL as query parameter:

```
https://<region-project>.cloudfunctions.net/scrape_ebay?url=https://www.ebay.com/itm/203449771112?hash=item2f5e8d2468:g:0zcAAOSwssFgnD-K&var=503791306328
```

If you see any errors fix them. To run cloud functions two basic things are important:
1. Enable billing on your project
2. Enable Cloud Build API

{{< adsense >}}

## Conclusion
You will have [more options](https://cloud.google.com/sdk/gcloud/reference/functions/deploy) during the deployment. For example, you can set the memory required for your function to run. The complete source code of the project is available [here](https://github.com/saisyam/scraper-on-cloud/). The major advantage of Google cloud functions when compared to AWS Lambda is, Google cloud functions respect `requirements.txt` file to install dependencies, where as in AWS Lambda you need to bundle all your dependencies. Thanks for reading.
