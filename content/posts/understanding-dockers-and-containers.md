+++
title = "Understanding Dockers and Containers"
description = "Dockers and containers are the building blocks for Cloud native development. Docker enables developers to build, run and ship applications that can be virtually run on any Operating System (OS). A running instance of a docker is called Container. In this article we will discuss about hardware and OS Virtualization, how docker works, docker workflow and commands to create and run docker."
author = "Saisyam"
publishdate = "2020-10-13"
lastmodified = "2020-10-13"
categories = ["cloud"]
tags = [
    "cloud native",
    "docker",
    "container",
    "virtualization"
]
draft = "true"
+++

Docker bought a revolutionary change in Cloud Computing. Docker is inspired by a technology in Linux kernel called [LXC (Linux Containers)](https://linuxcontainers.org/). LXC refers to the capabilites of Linux kernel, specifically namespaces and control groups, which allow sandboxing of processes from one another, and controlling their resource allocations. Docker utilizes the capabilities of LXC and hence it does not require any OS, instead it relies on the OS own functionality. LXC is a OS level virutalization method for running multiple isolated Linux systems (we call them containers) on a control host using a single Linux kernel.

## Hardware Virutalization vs OS Level Virtualization
If you use [VMWare](https://www.vmware.com/in.html) or [Virtualbox](https://www.virtualbox.org/), then you are already using hardware virtualization. Below diagram shows the difference between the two.
