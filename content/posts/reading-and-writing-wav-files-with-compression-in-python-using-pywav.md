---
title: "Reading and Writing WAV files with compression in Python using Pywav"
categories:
  - Python
tags:
  - WAV compression
  - PCMA
  - PCMU
date: 2020-12-20
images:
  - ../pywav.jpg
---

Testing voice applications involve handling RTP streams. Most of the IVR applications use PCMA/PCMU codecs to stream voice over RTP. [PyWav](https://pypi.org/project/pywav/) is a Python library which helps you to convert those RAW streams with PCMU/PCMA encodings into a WAV file. The library is similar to [PyWave](https://pypi.org/project/PyWave/), but the limitation with PyWave is, it will not support compression. 

{{< table_of_contents >}}

{{< adsense >}}

## What is PyWav?
When building a SIP test tool, I have to convert RTP audio streams with PCMA/PCMU encodings to WAV files for validation. PyWav is the result of that activity. Using PyWav you can write RAW streams into a playable WAV file supporting PCM/PCMA/PCMU compressions. Similarly, you can even read a WAV and get the RAW data from that WAV file to send it over RTP stream.

## Reading a WAV file
Below sample code shows how to read a WAV file:

{{< gist saisyam 1193c9281005ad18e1a0c7ff3f5a65b1 >}}

Following are the list of parameters that are extracted from the WAV file:
* Number of channels
* Sample rate
* Byte rate
* Bytes per sample
* Bits per sample
* Sample length
* Data (raw data)
* Audio format
* Extra params

To know more about WAV files and formats refer [this article](http://www-mmsp.ece.mcgill.ca/Documents/AudioFormats/WAVE/WAVE.html).

{{< adsense >}}

## Writing a WAV file
You can write the RAW data captured from the RTP stream into a WAV file using the following sample:

{{< gist saisyam 7d8a2ebd2c50b105c3410d72ae74a4b0 >}}

## Conclusion
You can install this as a `pip` package using `pip3 install pywav`. The source code of this project is available on [Github](https://github.com/saisyam/pywav). 

[Background photo created by kjpargeter - www.freepik.com](https://www.freepik.com/photos/background)