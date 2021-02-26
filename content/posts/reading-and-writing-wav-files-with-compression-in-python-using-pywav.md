---
title: "Reading and Writing WAV files with compression in Python using Pywav"
categories:
  - Python
tags:
  - WAV compression
  - PCMA
  - PCMU
date: 2020-12-20
---

Testing voice applications involve handling RTP streams. Most of the IVR applications use PCMA/PCMU codecs to stream voice over RTP. [PyWav](https://pypi.org/project/pywav/) is a Python library which helps you to convert those RAW streams with PCMU/PCMA encodings into a WAV file. The library is similar to [PyWave](https://pypi.org/project/PyWave/), but the limitation with PyWave is, it will not support compression. 

{{< table_of_contents >}}

{{< adsense >}}

## What is PyWav?
When building a SIP test tool, I have to convert RTP audio streams with PCMA/PCMU encodings to WAV files for validation. PyWav is the result of that activity. Using PyWav you can write RAW streams into a playable WAV file supporting PCM/PCMA/PCMU compressions. Similarly, you can even read a WAV and get the RAW data from that WAV file to send it over RTP stream.

## Reading a WAV file
Below sample code shows how to read a WAV file:

```python
wave_read = pywav.WavRead("<filename to read>")
# print parameters like number of channels, sample rate, bits per sample, audio format etc
# Audio format 1 = PCM (without compression)
# Audio format 6 = PCMA (with A-law compression)
# Audio format 7 = PCMU (with mu-law compression)
print(wave_read.getparams())
```
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

```python
# first parameter is the file name to write the wave data
# second parameter is the number of channels, the value can be 1 (mono) or 2 (stereo)
# third parameter is the sample rate, 8000 samples per second
# fourth paramaer is the bits per sample, 8 bits per sample
# fifth parameter is the audio format, 1 means PCM with no compression.
wave_write = pywav.WavWrite("<filename to write>", 1, 8000, 8, 1)
# raw_data is the byte array. Write can be done only once for now.
# Incremental write will be implemented later
wave_write.write(<raw data>)
# close the file stream and save the file
wave_write.close()
```

## Conclusion
You can install this as a `pip` package using `pip3 install pywav`. The source code of this project is available on [Github](https://github.com/saisyam/pywav). 
