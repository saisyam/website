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
First thing we need to do is to identity the businesses we want to scrape. As I mentioned earlier, I am interested in restaurants in US. Let's stick to restaurants in New York state. Below is the URL and the image of the business, [**Breslin Berger**](https://www.google.com/maps/place/Breslin+Burger/@40.745568,-73.990283,17z/data=!4m5!3m4!1s0x89c259a61b874f57:0xc5fd887f907ea722!8m2!3d40.745568!4d-73.988089) we want to scrape.
![Breslin Burger](/breslin_burger.png "Breslin Burger")
We would like to get the following information from the above page:
* Image of the restaurant
* Restaurant name
* Overall rating and number of reviews
* Type of the restaurant
* Location of the restaurant

Clicking on `reviews` will take you to the reviews page.
![Breslin Burger Reviews](/breslin_burger_reviews.png "Breslin Burger Reviews")
We want the latest reviews, so we sort the reviews by `latest` and then scrape them.

## Scraping business information
Google uses dynamic ids and class names in it's pages which make scraping a bit complex. We use different attributes other than ids and classes to get the elements. We use `Splinter` to load the page as it contains JavaScript. Get the `html` from Splinter `Browser` component and use `BeautifulSoup` to scrape the content. The following code snippet will scrape the business information:

```python
def get_business_info(url):
    business = {}
    business['base_url'] = url
    browser = Browser("chrome", headless=False)
    browser.visit(url)
    time.sleep(5)
    soup = BeautifulSoup(browser.html, "html5lib")
    img_div = soup.select_one('div[class*="section-hero-header-image-hero-container"]')
    img = img_div.find("img").get("src")
    business['img'] = img
    addr_button = soup.select_one('button[data-item-id="address"]')
    addr = addr_button['aria-label'].replace("Address: ", "").strip()
    business['address'] = addr
    business_name = soup.select_one('h1[class*="header-title-title"]').text.strip()
    business['name'] = business_name
    rating = soup.find("ol", {"class":"section-star-array"})['aria-label'].replace("stars", "").strip()
    business['rating'] = float(rating)
    total_reviews = soup.select_one('button:-soup-contains("reviews")').text
    business['review_count'] = int(total_reviews.replace("reviews", "").strip().replace(",","")) 
    res_type = soup.select_one('div[class="gm2-body-2"]').text
    business['type'] = res_type.replace("·","")
    business['source'] = "Google"
    reviews_button = browser.find_by_text(total_reviews).click()
    time.sleep(3)
    business['reviews_url'] = browser.url
    browser.quit()
    return business
```
I have used `select_one` method from BeautifulSoup which finds only the first element that matches the selector. The beauty of this method is we can look for partial selectors as well. This method also uses `Pseudo-Classes`. I have used `:-soup-contains` in the above code to get the button element that has `reviews` in the text. Refer to this [link](https://facelessuser.github.io/soupsieve/selectors/pseudo-classes/) to know more about Pseudo-Classes.

The output of the above method is a python dictionary:
```python
{
    "base_url": "https://www.google.com/maps/place/Breslin+Burger/@40.745568,-73.990283,17z/data=!3m1!4b1!4m5!3m4!1s0x89c259a61b874f57:0xc5fd887f907ea722!8m2!3d40.745568!4d-73.988089",
    "img": "https://lh5.googleusercontent.com/p/AF1QipOQV1HUtfhIRxtict4owToQkQpQHbCJZ_KsW83e=w491-h240-k-no",
    "address": "16 W 29th St, New York, NY 10001, United States",
    "name": "Breslin Burger",
    "rating": 4.3,
    "review_count": 1125,
    "type": "Hamburger restaurant",
    "source": "Google",
    "reviews_url": "https://www.google.com/maps/place/Breslin+Burger/@40.745568,-73.9902777,17z/data=!4m7!3m6!1s0x89c259a61b874f57:0xc5fd887f907ea722!8m2!3d40.745568!4d-73.988089!9m1!1b1"
}
```
## Scraping the reviews page
We have to sort the reviews by latest. We load the page in Splinter Browser component and do the click actions to sort the reviews. Once the page loads with sorted by latest reviews we will scroll the page until we get the desired number of reviews. Google uses dynamic loading of reviews, so we have to scroll down to load the next set of reviews. I just need the 300 latest reviews, ofcourse, count is a parameter to the scraping function.

The following code snippet will scroll the page until we get the desired number of reviews:
```python
def get_html(url, count):
    browser = Browser("chrome", headless=False)
    browser.visit(url)
    time.sleep(2)
    # sort and select newest for the list
    browser.find_by_text("Sort").first.click()
    time.sleep(2)
    new_menu_item = browser.find_by_id("action-menu").find_by_tag("ul").find_by_tag("li")[1]
    new_menu_item.click()
    time.sleep(7)
    rlen = get_review_count(browser.html)
    while rlen < count:
        browser.execute_script('document.querySelector("div.section-layout.section-scrollbox").scrollTop = document.querySelector("div.section-layout.section-scrollbox").scrollHeight')
        time.sleep(2)
        rlen = get_review_count(browser.html)
    html = browser.html
    browser.quit()
    return html

def get_review_count(html):
    soup = BeautifulSoup(html, "html5lib")
    reviews = soup.find_all('div', {'data-review-id': True, 'aria-label': True})
    return len(reviews)
```
The following code will do the scraping of the loaded reviews:
```python
def get_reviews(html):
    soup = BeautifulSoup(html, "html5lib")
    reviews = soup.find_all('div', {'data-review-id': True, 'aria-label': True})

    for r in reviews:   
        user = r['aria-label'].encode('ascii', 'ignore').decode('UTF-8')
        review_id = r['data-review-id']
        content_div = r.find("div", {'data-review-id': review_id})
        stars = content_div.find("span", {'role':'img'})['aria-label'].strip()
        rating = int(stars.split(' ')[0])
        date = content_div.select_one('span[class*="-date"]').text.strip()
        text = content_div.select_one('span[class*="-text"]').text.strip()
        if len(text) == 0:
            continue
        else:
            if "(Translated by Google)" in text: 
                text = text.replace("(Translated by Google) ", "")
                if "(Original)" in text:
                    idx = text.index("(Original)")
                    text = text[0:idx]

        yield {
            "rating": rating,
            "id": review_id,
            "user": user,
            "date": dateparser.parse(date).strftime("%d-%m-%Y"),
            "review": text.replace("\n",'').encode('ascii', 'ignore').decode('UTF-8')
        }
```
## Conclusion
We have learnt how to use `BeautifulSoup` and `Splinter` Python packages to scrape Google reviews. The complete scraper code is available from my [Github repo](https://github.com/saisyam/reviews-scraper). In the future articles we will see how to analyze these reviews to extract some important insights about the business. Thanks for reading. 
