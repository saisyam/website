+++
title = "What are Cloud native apps?"
description = "Cloud native apps are built using technologies that leverage the full potential of the cloud, deployed and managed in the cloud. Cloud native apps are loosely coupled services that are small and independent. They are auto deployed and scale automatically based on the load. The four important tenets in building cloud native apps are: Microservices, Containers, DevOps and Continuous Integration/Delivery (CI/CD)"
author = "Saisyam"
publishdate = "2020-10-15"
lastmodified = "2020-10-15"
categories = ["cloud"]
tags = [
    "cloud native",
    "microservices",
    "container",
    "devops",
    "ci/cd"
]
draft = "true"
+++
Cloud native is an approach to building and running applications that exploits the advantages of the cloud computing delivery model. In simple terms, Cloud native applications are built using cloud-based technologies, fully hosted and managed in the cloud.

{{< pimage src="cloud_native.jpg" alt="Cloud native flow">}}

## Cloud Native approach
Here we will discuss about the four major components of Cloud native development:

1. Microservices
2. Containers
3. DevOps
4. CI/CD

{{< adsense type="article" >}}

## Microservices
Microservices are small modular and loosely coupled services. Each microservice will perform a single task, [single responsibility principle](https://en.wikipedia.org/wiki/Single-responsibility_principle). Loose coupling and service based design will help organizations increase their application creation speed without increasing complexity. Microservices run in their own process and communicate via HTTP APIs or messaging. Each microservice can be deployed, upgraded, scaled and restarted independent of other services in the application.

## Containers
Containers (running instances of dockers) provide both efficiency and speed when compared to virtual machines. They use Operating System (OS) level virtualization, where one instance of OS services is divided across various containers. To know more about dockers and containers read my article [here](https://saisyam.com/understanding-dockers-and-containers/). The low overhead of creating/destroying and starting/stopping containers make them ideal to deploy microservices. We can run multiple containers in a single VM.

## DevOps
DevOps is a collaboration between developers and operations with a goal of constantly delivering high quality software. Early adoption of DevOps culture into the organization will help accelerate the path to cloud0-native applications and deploy apps faster and more efficiently. DevOps adoption not only relies on tools and technologies, but also the mindset, willingness and trust of the people to collaborative to develop and deliver applications.

## CI/CD
CI/CD is all about shipping small batches of software to production frequently through automation. Automation of continuous delivery will help organizations to quicky and efficiently deliver updated software to production without bringing down the whole system.   

{{< adsense type="article" >}}

## Advantages of Cloud native development approach
Our traditional development approach is mostly monolith. We will create an application with different modules doing differen tasks. For example, in a shopping cart application, we have modules for product management, user management, billing, etc. But all these are tightly coupled with each other which makes it difficult to make any changes to an individual module without disturbing the others.

This is a successful model till date. Then **why we need to shift to cloud native approach?** To answer this, let's see the differences between traditional and cloud native approaches from different perspectives. Below table will illustrate that:

{{< table "table table-striped table-bordered" >}}

|                          | Traditional Approach                    | Cloud native Approach |
|------------------------|-----------------------------------------|-----------------------|
| Development Methodology  | Waterfall                               | Agile, DevOps, CI/CD  |
| Design/Architecture | Tightly coupled, monolith | loosely coupled, API/Message based communication, single responsibility principle |
| Workforce                | Isolated development, operations, QA and delivery teams| DevOps teams |
| Delivery | Long cycles | Short and Continuous |
| Infrastructure | OS Dependent, Vertically scaled, Pre-provisioned for peak load | OS independent, horizontal scaling, on-demand capacity |
| Maintenance/Recovery | In case of failure, whole system will go down, slow recovery | Fast recovery, whole system will never go down |

{{< /table >}}


## Cloud native approach - Things to consider
Though cloud native approach looks promising with more advantages over traditional approach and improves the efficiency and productivity of the team, it comes with a price. Below are some of the things that need to be considered while designing cloud native apps.

1. How to split the application into independent logic units or services?
2. How to connect individual microservices so that the aggregate serves as a complete application?
3. Monitoring the health of Microservices (dead/alive) is important to make sure the request is handled properly
4. Debugging of microservices will be daunting if the number of microservices are more

There are tools which are production ready to solve the above issues. Discussion of these tools will take another post.

{{< adsense type="article" >}}

## Conclusion
Most of the organizations are moving their applications from private datacenters to public cloud platforms using cloud native architectural precepts. Learning and investing time on Cloud native applications will definitely uplift your resume/CV. In the next article we will discuss how to build a cloud native application from scratch and deploy it. Thanks for reading. 




