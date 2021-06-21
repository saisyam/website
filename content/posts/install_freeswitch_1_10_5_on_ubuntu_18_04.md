---
title: "Install FreeSWITCH 1.10.5 on Ubuntu 18.04"
categories:
  - FreeSWITCH
tags:
  - Ubuntu 18.04
  - FreeSWITCH 1.10.5
date: 2021-01-20
images:
  - ../freeswitch_ubuntu.jpg
---

[FreeSWITCH](https://freeswitch.com/) is a Software Defined Telecom Stack enabling the digital transformation from proprietary telecom switches to a versatile software implementation that runs on any commodity hardware. From a Raspberry PI to a multi-core server, FreeSWITCH can unlock the telecommunications potential of any device.

{{< table_of_contents >}}

{{< adsense >}}

## Build FreeSWITCH from source
FreeSWITCH recommends Debian as their installation platform as most of the development happens on that platform. In this article we see how to install [FreeSWITCH 1.10.5](https://github.com/signalwire/freeswitch/releases/tag/v1.10.5) on [Ubuntu 18.04](https://releases.ubuntu.com/18.04/). Below are the list of steps we follow to install FreeSWITCH on Ubuntu:
1. Bring up a VM (I use Virtualbox) and install Ubuntu 18.04 server.
2. Install dependent packages to compile FreeSWITCH
3. Compile and build SpanDSP and sofia-sip
4. Download FreeSWITCH source, build and install
5. Make FreeSWITCH to run as a service

**Note:** *The scope of this article is to install and run FreeSWITCH successfully on Ubuntu 18.04. We don't configure FreeSWITCH to handle incoming/outgoing calls.*

## Setup Ubuntu 18.04 server
I use Virtualbox to create an Ubuntu 18.04 server with 30GB of hard disk space and 4GB of RAM. You can build the same or use a desktop or laptop running Ubuntu 18.04. If you want more information on how to install Ubuntu on Virtualbox refer to this [article](https://hibbard.eu/install-ubuntu-virtual-box/).

{{< adsense >}}

## Install dependent packages to compile FreeSWITCH
We need some dependent packages to compile FreeSWITCH code. FreeSWITCH is built on C/C++. Let's install the packages on Ubuntu server using its package manager by `ssh` into the server:

{{< gist saisyam 4645728303643074df1b0dc8a9a06075 >}}

FreeSWITCH comes with lots of modules. For example, it has support for MySQL, MongoDB, PostgreSQL etc. Most of the deployments use only one database and want to disable the other. So, they don't install dependent packages of the modules which they don't need. But in our case we install FreeSWITCH with default modules enabled. Below is the list of additional packages we need:

{{< gist saisyam b6c3ccc07f962dd78adaa16617d7e791 >}}

We have compile and build `libks`
{{< gist saisyam 1d11794d04ec8758ad76bca45731f55a >}}

## Compile and build SpanDSP and sofia-sip
According to [FreeSWITCH 1.10.x release notes](https://freeswitch.org/confluence/display/freeswitch/freeswitch+1.10.x+Release+notes) SpanDSP and sofia-sip packages are removed from build. We need to compile and build them separately. 

Execute the below commands to build sofia-sip:

{{< gist saisyam 66b85e46f6bc4d74626653d5ce45e517 >}}

{{< adsense >}}

Execute the below commands to build SpanDSP:

{{< gist saisyam eb027f2eb2c22964e63f5997b0970715 >}}

## Download FreeSWITCH source, build and install
We can download the FreeSWITCH release source code from [Github releases](https://github.com/signalwire/freeswitch/releases). Execute the following commands to download, build and install FreeSWITCH:

{{< gist saisyam 1cbbad55531083df0e9298e07991b0e3 >}}

Once `configure` is successfully completed, `modules.conf` file is created in `freeswitch-1.10.5` folder. You can comment out unnecessary modules in `modules.conf` file so that FreeSWITCH will not build those modules. As of now we will comment out only one module, `mod_signalwire`.

{{< gist saisyam 9b87e7397d759e8bd7905725759cf5a4 >}}

Let's start the build:

{{< gist saisyam 6fe24666da6657f541631836d07a9fb6 >}}

After successful installation we need to install default sounds and voice files.

{{< gist saisyam 39fbce99820e20dde5f88d59acc93ce6 >}}

FreeSWITCH is installed at `/usr/local/freeswitch` and the binaries are available at `/usr/local/freeswitch/bin` folder.

Now you can run FreeSWITCH using:

{{< gist saisyam 1a9461603d4db2124b695631ee0ef744 >}}

[`fs_cli`](https://freeswitch.org/confluence/pages/viewpage.action?pageId=1048948) is a FreeSWITCH command-line interface that allows a user to connect to running FreeSWITCH instance. Open the other terminal and run:

{{< gist saisyam 3732302e5ea5e46acf01dc506c4cb3fd >}}

{{< adsense >}}

## Make FreeSWITCH to run as a service
By default FreeSWITCH will not be installed as a service in Ubuntu. Let's make it a service

Add new group and user with less privileges to run FreeSWITCH service.

{{< gist saisyam 1a2d59c43a2a05dff6741e702beab12f >}}

We need to add FreeSwitch as a `systemd` unit file. Open new file `/etc/systemd/system/freeswitch.service` using `vim` editor paste the below content:

{{< gist saisyam e6af8d064dd004c6e66f6d9f2451a59f >}}

Start FreeSWITCH service and enable it on system startup

{{< gist saisyam 89926f2c6e6d352633175aedd79761ed >}}

Now check status of FreeSWITCH service

{{< gist saisyam adc4bb8000ff5befcd99d5bca0783671 >}}

![Freeswitch Service](../freeswitch_service.jpg)

{{< adsense >}}

## Conclusion
In this article we learnt how to install FreeSWITCH on Ubuntu 18.04. This is a default installation with all the modules included. Production ready installations remove lot of unnecessary modules which reduces build and load time. You can configure what modules need to be loaded during startup by changing `modules.conf.xml` file under `/usr/local/freeswitch/conf/autoload_configs` folder. In the coming articles we will see how to configure FreeSWITCH to handle simple inbound and outbound calls.