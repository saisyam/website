---
title: "Understanding Dockers and Containers"
categories:
  - Cloud
tags:
  - Dockers
  - Containers
date: 2020-10-13
---

Dockers and containers are the building blocks for Cloud native development. Docker enables developers to build, run and ship applications that can be virtually run on any Operating System (OS). A running instance of a docker is called Container. In this article we will discuss about hardware and OS Virtualization, how docker works, docker workflow and commands to create and run docker.

{{< table_of_contents >}}

{{< adsense >}}

Docker bought a revolutionary change in Cloud Computing. Docker is inspired by a technology in Linux kernel called [LXC (Linux Containers)](https://linuxcontainers.org/). LXC refers to the capabilites of Linux kernel, specifically namespaces and control groups, which allow sandboxing of processes from one another, and controlling their resource allocations. Docker utilizes the capabilities of LXC and hence it does not require any OS, instead it relies on the OS own functionality. LXC is a OS level virutalization method for running multiple isolated Linux systems (we call them containers) on a control host using a single Linux kernel.

## Hardware Virutalization vs OS Level Virtualization
If you use [VMWare](https://www.vmware.com/in.html) or [Virtualbox](https://www.virtualbox.org/), then you are already using hardware virtualization. Below diagram shows the difference between the two.

![Virtualization](/assets/images/blog/virtualization.jpg)

The primary advantage of OS virtualiation is that multiple workloads can run on a single OS instance. With hardware virtualization (VMs), the hardware is being virtualized to run multiple OS instances.


## What are Containers?
OS virtualization had grown tremendously over the last decade. Containers sit on top of a physical server and its host OS (Linux/Windows). Each container shares the host OS kernel and libraries in a read-only mode. Containers are thus exceptionally light (few mega bytes) and will start in few seconds when compared to a VM. Containers’ speed, agility, and portability make them the primary tool to help building cloud native apps.

## What is a Docker?
Docker is a layered binary file with all the necessary ingredients to run an application. For example, I want to use PostgreSQL database in my project and I don't want to install it on my machine. I can download PostgreSQL docker from [docker hub](https://hub.docker.com/_/postgres) and run it with `docker` command and use it for my development. You can also create your own docker for your application and upload that to docker hub for others to use. You can install docker community edition from [here](https://docs.docker.com/get-docker/).

{{< adsense >}}

## Let's run a Nginx docker
In this section we will download and run nginx docker. [Nginx](https://www.nginx.com/) is a popular web server that is used in most of the production ready environments. In the process we will learn some basic docker commands.

{{< gist saisyam 73e26c570f472ea4ff4d1576d6208a3f >}}

The above command `docker run -p 80:80 nginx` will pull `nginx` from [docker hub](https://hub.docker.com/_/nginx) as I don't have it locally and runs it on port 80:80 (local:container). If I go to `http://localhost/` I will see nginx default html page with `Welcome to nginx!` message. To kill it, simply press CTRL+C. To run the docker in the background or as daemon we will add `-d` option to the above command. List of some basic commands below:

<div class="my-table">

| Command                                         | Action                                         |
|-------------------------------------------------|----------------------|
| docker images                                   | Prints all local images |
| docker run <image>                              | Run a docker image |
| docker run -p &lt;host-port:container-port&gt; &lt;image&gt; | Run docker image with port forwarding from host to container |
| docker run -d &lt;image&gt;                           | Run docker image as daemon or in the background |
| docker run -e &lt;environment variable&gt; &lt;image&gt;    | Run docker image with defined environment variable |
| docker ps                                       | Displays all running containers |
| docker ps --all                                 | Displays all containers including idle ones |
| docker kill &lt;container-id&gt;                      | Kill (force kill) the running docker image with the container id|
| docker rm &lt;container-id&gt;                        | Remove the container |
</div>

## Docker workflow
Below diagram shows the docker workflow. I have taken this image from a LinkedIn training.

![Docker Workflow](/assets/images/blog/docker_workflow.jpg)

At a high level the image explains:
1. Building docker images from docker file
2. Tagging docker images
3. Pushing docker images to docker hub
4. Pulling images from docker hub
5. Running docker images
6. Starting/stopping/restarting docker containers
7. Loading and saving docker images externally

{{< adsense >}}

## Conclusion
In this article we learn what is docker and container? We have learnt how to download and run a docker and some basic commands on managing docker images and containers. We have looked into docker workflow. In the coming articles we will see how to create our own docker and publish to docker hub. Thanks for reading.

