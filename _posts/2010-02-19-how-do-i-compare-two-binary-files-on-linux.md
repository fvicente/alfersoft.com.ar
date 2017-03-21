---
uid: 128
title: How do I compare two binary files on Linux?
date: 2010-02-19T14:51:00+00:00
author: fvicente
layout: post
guid: ?p=128
categories:
  - Linux FAQ
  - Programming FAQ
tags:
  - binaries
  - binary
  - compare
  - diff
  - hexdump
  - linux
  - meld
comments: true
excerpt: How to compare two binary files on Linux terminal using hexdump and diff
---
<figure>
	<img src="{{ site.baseurl }}/images/question.png">
	<figcaption>Photo Credit: <a href="http://commons.wikimedia.org/wiki/File:Gnome-dialog-question.svg" title="Wikimedia Commons"> Wikimedia Commons</a></figcaption>
</figure>

The easiest way I found is dumping the binaries into text files using hexdump and then comparing them with your favourite program (diff, Meld, etc.). E.g.:

{% highlight bash %}
hexdump -C a.bin >a.txt
hexdump -C b.bin >b.txt
diff a.txt b.txt
{% endhighlight %}
