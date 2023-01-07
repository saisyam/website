---
title: "Will Webassembly Takeover Containers in future?"
categories:
  - WebAssembly
tags:
  - WebAssembly
  - Containers
date: 2023-01-07
image: /webscraping.jpg
---
[WebAssembly(WASM)](https://webassembly.org/) has the potential to provide a more lightweight and secure execution environment than traditional containers, which could be beneficial for applications that require high performance and security. However, it is still relatively early in its development cycle and it is unclear how much traction it will gain in the future. But, based on the recent developments happening in this space gives us a feel that WASM will slowly take over containers.

In this article we will see what developments are happening in this area and which companies are interested in building Microservices using WASM.

## What is WebAssembly(WASM)?
WASM by itself need a complete post. I don't go into very technical details of WASM but will discuss at a very high level.

> WebAssembly (abbreviated Wasm) is a binary instruction format for a stack-based virtual machine. Wasm is designed as a portable compilation target for programming languages, enabling deployment on the web for client and server applications.

WASM was originally developed to embed applications or libraries built with other programming languages like (C/C++, Python, Rust, Go etc) into web applications. For example, if we compile a simple Python program into WASM binary format, it can be executed inside a browser.

Recent developments made to run WASM outside of browser which opened a lot of opportunities in Microservices space. One way to run WebAssembly outside of a web browser is to use a standalone WebAssembly runtime, such as [Wasmer](https://wasmer.io/) or [Wasmtime](https://wasmtime.dev/). These runtimes can execute WebAssembly modules directly, without the need for a web browser.

## What is WebAssembly System Interface (WASI)?
WASM has limitations in terms of I/O which is solved by WASI. WASI is a system interface for WebAssembly that allows WebAssembly programs to access system resources such as the file system, environment variables, and system calls. It is designed to be a platform-agnostic interface that can be used to run WebAssembly programs on a variety of different platforms, including servers, desktops, and embedded devices.

WASI is implemented as a set of imports that can be added to a WebAssembly module, similar to how WebAssembly can import functions from JavaScript. This allows WebAssembly programs to call WASI functions in order to access system resources.

WASI is still in development and is not yet widely supported. However, it has the potential to greatly expand the capabilities of WebAssembly and make it even more useful for a variety of different applications. [WASI is being standardized](https://hacks.mozilla.org/2019/03/standardizing-wasi-a-webassembly-system-interface/). 

## Recent Developments
[Fermyon](https://www.fermyon.com/about) is working extensively of using WASM for building Microservices. They developed tools to use WASM to build Microservices in Rust and Go. All their websites run WASM microservices in their backend. Fermyon developed [Spin](https://developer.fermyon.com/spin/index).

> Spin is a framework for building and running event-driven microservice applications with WebAssembly (Wasm) components. With Spin, we’re trying to make it easier to get started with using WebAssembly on the server so that we can all take advantage of the security, portability, and speed WebAssembly provides when it comes to running microservices.

Fermyon also have a [cloud application platform](https://developer.fermyon.com/cloud/index) for WASM microservices (which is in open beta at the time of this post). It enabled you to run Spin applications, at scale, in the cloud, without any infrastructure setup. 

They have a good documentation on how to build and run WASM based Microservices with Rust and Go. These are the guys who introduced Kubernets to WASM with Krustlet.

[Krustlet](https://krustlet.dev/) Krustlet is a Kubelet written in Rust — which listens on the event stream for new pods that the scheduler assigns to it, based on specific Kubernetes tolerations.

We can deploy WASM binaries on [Azure](https://www.infoworld.com/article/3681853/azure-kubernetes-doubles-down-on-webassembly.html).

## Conclusion
In this article we learnt what is WASM is (ofcourse, at a very high level) and what are the recent developments happening in bringing it to the mainstream development. I am very excited to try to build a simple Microservice in Golang with Fermyon Spin and deploy it to Azure Kubernetes. Will definitely write more articles on WASM. Thanks for reading.

