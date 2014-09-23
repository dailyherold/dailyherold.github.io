---
layout: post
title: Docker + Linux Mint 17
excerpt: Why the trouble?
date: 2014-09-23 00:00:00 
author: John Paul Herold
categories: docker
published: true
---
### Some tips on getting Docker to play nice with LM 17.
***
The past four days can be broken down as follows:
{% highlight js %}
|Activity  | 10 20 30 40 50 60 70 80 90 100 | %
|Football  |=======                         | 
|Amazon EC2|====                            |
|Docker    |================================|======
|Food/Sleep|===========                     |
{% endhighlight %}

Not matematically accurate, but Docker has been on my mind [and terminal] a lot. It started with an assignment to implement [Jenkins](http://jenkins-ci.org/) at work, which naturally led me to dusting off Vagrant...so that I could run a build of CoreOS, and play with Docker, in the context of Mr. Jenkins. 4 birds one session.

Why start with Vagrant you ask? Answer, I had no experience with LXC containers and wanted an isolated environment to really get my hands dirty. Auxillary truth, CoreOS has always perked my interest, and I figured its' sweet [Vagrant repo]() would give me a superb Docker-friendly sandbox to test.

72 hours later, I absorbed a lot (more to come), and wanted to get Docker running on my native OS. This should be easy I told myself, I've been running Docker on CoreOS (Vagrant), RHEL (Amazon), and the containers I've ran originated from a wide spectrum of base OS images...

#### *Problem*
`Cannot connect to the Docker daemon. Is 'docker -d' running on this host?`

#### *Research*
I followed the standard install process (link) for Ubuntu based distros. Purged, then tried the shortcut curl method (link). Tried all the docker service calls (start/stop/restart). Logged in/out, restarted machine. `$ apt-cache show docker.io` to get a list of the recommended packages and installed those. Even Docker's specific instruction for Mint users: `$ sudo apt-get update && sudo apt-get install cgroup-lite apparmor` [doc](https://docs.docker.com/installation/ubuntulinux/#ubuntu-trusty-1404-lts-64-bit).

Found some good resources:
- Mint sets Recommended packages to false by default (https://bugs.launchpad.net/linuxmint/+bug/1266234)
- closed issue with suggested lxd (https://github.com/docker/docker/issues/4578)
- others with my issue (https://github.com/docker/docker/issues/1899)
- Mint recommends set to false (https://bugs.launchpad.net/linuxmint/+bug/1266234)

#### *Environment*
{% highlight sh %}
$ uname -a
3.13.0-24-generic #47-Ubuntu SMP x86_64 x86_64 x86_64 GNU/Linux
{% endhighlight %}

{% highlight sh %}
$ cat /etc/*-release
DISTRIB_ID=LinuxMint
DISTRIB_RELEASE=17
DISTRIB_CODENAME=qiana
DISTRIB_DESCRIPTION="Linux Mint 17 Qiana"
NAME="Ubuntu"
VERSION="14.04.1 LTS, Trusty Tahr"
{% endhighlight %}

#### *The Problem*
