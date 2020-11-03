+++
title = "My Work"
subtitle = "Some of the projects I worked"
type = "work"  
layout = "page"
publishdate = "2020-10-30"
lastmodified = "2020-10-30"
+++

{{< table "table table-striped table-bordered" >}}

| Project | Reputation Management and Online presence ranking  |
|----------------------------------|-----------------------------------------|
| **Description** | A platform for checking the online presence of a business and managing the online reputation of the business |
| **Solution** | The platform uses web scrapers to collect data for a specific business from various sources on the Internet and generates a score based on postivie/negative feedback from the consumers. Based on score, suggestions are provided to improve the reputation of the business online. This platform helps businesses to correct or improve their online presence to get more consumers and increase revenue.|
| **Technologies** | Python Django, ReactJS, MongoDB, PostgreSQL, Splinter, BeautifulSoup, Vader for Sentiment analysis, RabbitMQ, Authorize.net payment integration, Highcharts |

{{< /table >}}

{{< table "table table-striped table-bordered" >}}

| Project | Visualize metrics generated from a hardware Device  |
|----------------------------------|-----------------------------------------|
| **Description** | A hardware device is subjected to load testing. During the test the device generates lots of metrics like, temperature, voltage, power, bytes received/transferred etc. These metrics need to be captured and presented in a web application using different kinds of charts. Each module on the hardware device is owned by a team which will look at this metrics when the test case fails. The metrics are stored in MongoDB for furture reference. |
| **Solution** | The device generates data at a very high speed which is difficult for a human eye to validate. The speed of the data also made the web application freeze. We introduced a delay of one second in presenting the data, but at the same time we don't want to loose the data that is generated in that second. We used sockets to avoid HTTP overhead. The data from sockets is dumped in a RabbitMQ queue and the consumers push the data to web application and MongoDB. The backend is built on Flask as client required a light weight framework. Frontend was built on PolymerJS with D3 Charts.|
| **Technologies** | Python Flask, PolymerJS, MongoDB, Sockets, RabbitMQ |

{{< /table >}}
