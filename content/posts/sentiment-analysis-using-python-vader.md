---
title: "Sentiment Analysis using Python Vader"
categories:
  - Python
tags:
  - Sentiment analysis
  - Text Analytics
  - Vader
date: 2020-09-26
---

Sentiment analysis is a process of determining whether the given emotion (text) is postivie, negative or neutral. Sentiment Analysis is useful in identifying customers emotions for a service or product. In this article we will perform sentiment analysis on restaurant reviews. 

{{< table_of_contents >}}

{{< adsense >}}

VADER(Valence Aware Dictionary and sEntiment Reasoner) is a lexicon and rule-based sentiment analysis tool that is specifically attuned to sentiments expressed in social media. VADER not only tells about the positivity and negativity score but also tells us about how positive or negative it is. VADER sentimental analysis relies on a dictionary that maps lexical features to emotion intensities known as sentiment scores. The sentiment score of a text can be obtained by summing up the intensity of each word in the text.

For example, words like, *'happy', 'awesome', 'good'* all convey positive emotion. VADER is intelligent enough to understand the context of these words. For example, *"Food is not good"* is considered negative. If also understands the emphasis of capitalization and punctuation. For example, *"AWESOME"* (capital letters) will represent the high intensity of positivity.


## Installing VADER Sentiment Analysis Tool
VADER is available as part of NLTK Python package. I use `pip3` to install Python packages. Below command will install `nltk`.

{{< gist saisyam e5a7db17bc5504a03b575181b502684b >}}

Once `nltk` is installed, we need to download the `vader lexicon`.

{{< gist saisyam fc7851f8f35f5af28aaa07069a905231 >}}

This will install required data for using VADER sentiment analysis.

{{< adsense >}}

## Performing Sentiment Analysis
We have everything installed to perform the sentiment analysis. Let's use VADER to find the sentiment of the following review:

*"Pretty pricey but the lamb burger ($25) is beyond amazing. Definitely worth it. So, so good."*

The following code will perform the sentiment analysis.

{{< gist saisyam a06b57411041d92a1d66689b6304df7f >}}

When we run the above code we get the following output:

{{< gist saisyam b6c5347e1f4931f9a8b618f5888927e6 >}}

VADER's `SentimentIntensityAnalyzer()` takes in a string and returns a dictionary of scores in each of four categories:
* negative
* neutral
* positive
* compound (computed by normalizing the scores above)

The above result says that the emotion in the given review is *positive*. Let's look at the other review:
*"The food is so good. The service is so bad."*
When we run the above code for the given text, the output is:

{{< gist saisyam 97cbb5f5e2cd1ff752e6d9c85dbc54b3 >}}

The review has two polarities. The customer is appreciating the food but not satisfied with the service. To judge whether the review is positive or negative we use the below logic.

{{< gist saisyam 17276051626ab5b80f7912b6ae46d783 >}}

Which says the above review is *negative*.

{{< adsense >}}

## What is missing with VADER Sentiment Analysis?
VADER only tries to get the emotion (postivie/negative/neutral) out of text. It won't care about the aspect. For example, the review,
*"The food is so good. The service is so bad."* is *negative* from the `service` aspect but *postive* from the `food` aspect. If we identify sentiment based on aspects then it will be much more helpful. This is called *Aspect based Sentiment Analysis*.

## Conclusion
In this post we have learnt how to find whether the text is positive, negative or neutral using Python based VADER Sentiment Analysis. We also discussed the next level of sentiment analysis based on aspects. In the next article we will see how we can identify aspects for a given industry or domain and implement the sentiment analysis based on aspects.
