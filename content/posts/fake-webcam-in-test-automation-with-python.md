+++
title = "Proven way to Fake Webcam in Test Automation with Python"
description = "To test web application with webcam we need to fake webcam with a video. We can use Chrome webdriver settings with Python Selenium or Splinter to do that."
author = "Saisyam"
publishdate = "2020-05-08"
lastmodified = "2020-06-07"
categories = ["testing"]
tags = [
    "web testing",
    "splinter",
    "ffmpeg",
    "fake-webcam",
]
+++

We automate web application testing using Python Selenium or Splinter. How will you automate testing applications with webcam? You can fake a webcam device with Chrome Web driver. In this article we will see how to do that.

## Challenge
The challenge here is to fake the webcam with proper video format. In this post I am using Chrome browser along with [Splinter](https://splinter.readthedocs.io/en/latest/).
If you are new to Splinter, check out my article and I promise you love it. We use Splinter not only for testing but also for web scraping. Chrome need Y4M format to replace the camera feed. To generate that format you need [ffmpeg](https://www.ffmpeg.org/) tool. Using that tool you can convert any MP4 video into Y4M format.

> Y4M is an un-compressed format. The converted Y4M file size will be almost three times the MP4 file. So, take a small MP4 file or repeat the image to create the video.

{{< adsense type="article" >}}

Once you have the Y4M file you can follow the below mentioned steps to fake the webcam. [Here](https://testrtc.com/y4m-video-chrome/) is an article on how to create Y4M files using [ffmpeg](https://www.ffmpeg.org/).

## Chrome Settings to fake webcam

We will set `webdriver.ChromeOptions` to enable media stream using a fake device.

{{< gist saisyam 98aa0345e2a470b39d754b35b7910596 >}}

## Conclusion
You are all set to test the web applications uing a fake video. The complete source code along with the sample web application (using Python Flask) is available as a [gitlab project](https://gitlab.com/saisyam/web-automation-testing/-/tree/master/fake_webcam).

