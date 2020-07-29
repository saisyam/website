+++
title = "Github Files as Gists in webpage"
description = "It's common to embed code snippets in blogs or personal websites. Adding code snippets from project source code as gists is a painful and double effort. Why not embed the Github project source file itself into a webpage? Yes, you can do that using Gistify."
author = "Saisyam"
publishdate = "2020-04-08"
lastmodified = "2020-06-09"
categories = ["tools"]
tags = [
    "flask",
    "python",
]
+++

Including Github Gists into a webpage is a common practice. But sometimes I would like to add a source file from one of my repos (public repos) into the webpage instead of splitting the file into multiple gists. 

I write technical blogs which involve lots of code to be shown and explained as you are reading now. I want to embed any source file from my public Github repo into my webpage or my blog. It will be nice if I can specify which lines of the source file I want to display.

So, I wrote a simple web application using Python Flask and deployed at `https://gistify.saisyam.com` which will perform the desired task. The complete source code of the application is available in my [Github](https://github.com/saisyam/gistify). The application performs the following tasks:

## Embed the complete source file

Just embed the URL of the source file to `https://gistify.saisyam.com`. For example, to embed, `https://github.com/saisyam/gistify/blob/master/sample.py` you have to append the Github file URL to, 

`https://gistify.saisyam.com/https://github.com/saisyam/gistify/blob/master/sample.py`


{{< gistify url="https://gistify.saisyam.com/https://github.com/saisyam/gistify/blob/master/sample.py" >}}

{{< adsense type="article" >}}

## Embed few lines of the source code

Add line numbers, or range of line numbers to embed along with the Github file url. Only thing to remember here is to add a `/l/` between the url and the line numbers. For example, to embed, 1 and 2 lines, you have to create URL like this, `https://gistify.saisyam.com/https://github.com/saisyam/gistify/blob/master/sample.py/l/1,2` 

{{< gistify url="https://gistify.saisyam.com/https://github.com/saisyam/gistify/blob/master/sample.py/l/1,2" >}}

You can provide range as well and both. For example, to embed, 1,2 and 6-8 you have to create the URL like this, 

`https://gistify.saisyam.com/https://github.com/saisyam/gistify/blob/master/sample.py/l/1,2,5-8`

{{< gistify url="https://gistify.saisyam.com/https://github.com/saisyam/gistify/blob/master/sample.py/l/1,2,5-8" >}}

## How to include `gistify` in your webpage

{{< adsense type="article" >}}

You have to create a `div` and some `JavaScript` code to load the content.

{{< gist saisyam 109806c5970aecab00b7f73bcb8d91a7 >}}

If you have issues with the styling workout your CSS or disable some classes.