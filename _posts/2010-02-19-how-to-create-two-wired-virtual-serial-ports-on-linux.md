---
uid: 134
title: How to create two wired virtual serial ports on Linux?
date: 2010-02-19T15:18:32+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=134
permalink: /2010/02/19/how-to-create-two-wired-virtual-serial-ports-on-linux/
categories:
  - Linux FAQ
  - Programming FAQ
tags:
  - bridged
  - linux
  - null-modem
  - port
  - rs-232
  - serial
  - ubuntu
  - virtual
  - wired
comments: true
excerpt: How to create two wired virtual serial ports on Linux using socat
---
<figure>
	<img src="{{ site.baseurl }}/images/question.png">
	<figcaption>Photo Credit: <a href="http://commons.wikimedia.org/wiki/File:Gnome-dialog-question.svg" title="Wikimedia Commons"> Wikimedia Commons</a></figcaption>
</figure>

To create two bridged virtual serial ports use the following command:

{% highlight bash %}
socat -d -d pty,raw,echo=0 pty,raw,echo=0
{% endhighlight %}

The output will show you which are the virtual ports (or pseudo terminals) created, e.g.:

{% highlight bash %}
2010/02/19 16:16:33 socat[9662] N PTY is /dev/pts/3
2010/02/19 16:16:33 socat[9662] N PTY is /dev/pts/4
2010/02/19 16:16:33 socat[9662] N starting data transfer loop with FDs [3,3] and [5,5]
{% endhighlight %}

Note: if you are using Ubuntu and you do not have this command, try:

{% highlight bash %}
sudo apt-get install socat
{% endhighlight %}
