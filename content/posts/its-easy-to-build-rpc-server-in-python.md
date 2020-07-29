+++
title = "It's Easy to Build RPC Server in Python"
description = "RPC stands for Remote Procedure Call. With RPC we can build lot of test automation tools. In this post we will learn how to build a RPC server using Python."
author = "Saisyam"
publishdate = "2020-05-18"
lastmodified = "2020-06-09"
categories = ["networking"]
tags = [
    "rpc server",
    "python twisted",
]
+++

RPC stands for Remote Procedure Call. RPC server is similar to any HTTP server running on a specific port. RPC also communicates over HTTP but uses XML for data transfer. So, Let's get started to build it using Python [Twisted](https://twistedmatrix.com/trac/) framework. If you want to know more about RPC, check its [Wikipedia](https://en.wikipedia.org/wiki/XML-RPC) page.

## Install Python Twsited
I always keep my Python projects inside virtual environments. So, I create a virtual environment using [virtualenv](https://virtualenv.pypa.io/en/stable/) Python package and install [Python Twisted](https://pypi.org/project/Twisted/) framework into the virtual environment.

## Create an RPC Server
Python `Twisted.Web` package provides `XMLRPC` class. We will extend our call from base `XMLRPC` class.

{{< gistify url="https://gistify.saisyam.com/https://github.com/saisyam/python-tools/blob/master/rpc/rpcserver.py" >}}

We are running our RPC server on port 7080. The RPC server exposes three methods, `info`, `add` and `multiply`. The RPC methods have to start with `xmlrpc_` key. The other parts of the code, like, endpoint, server, site and reactor are Twisted library components. Start with [reactor](https://twistedmatrix.com/documents/13.1.0/core/howto/reactor-basics.html) documentation. 

{{< adsense type="article" >}}

## Create RPC client
Let's build a client to talk to the server and call info, add and multiply methods. Below is the code snippet for the client.

{{< gistify url="https://gistify.saisyam.com/https://github.com/saisyam/python-tools/blob/master/rpc/rpcclient.py" >}}

When calling RPC methods, you don't need to use `xmlrpc_` key. You can call them directly as mentioned in the above code.

## Conclusion
You may be wondering why I need to create RPC server and Client for simple methods like add and multiply. This is a sample code to demonstrate how to build RPC server/client with Python Twisted. The main use of this RPC server comes when you want to communicate to other machines via Twisted. for example, I want to connect to a remote server via ssh and send commands to that server. Twisted is completely asynchronous and runs its own event loop when we call `reactor.run`. Read more about this [here](https://twistedmatrix.com/documents/8.2.0/core/howto/async.html). It's really awesome.