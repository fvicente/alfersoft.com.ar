---
uid: 131
title: How to calculate the space used by files in Linux?
date: 2010-02-19T15:09:26+00:00
author: fvicente
layout: single
guid: ?p=131
categories:
  - Linux FAQ
tags:
  - awk
  - calculate
  - files
  - find
  - linux
  - occupied
  - pattern
  - space
  - sum
  - used
comments: true
excerpt: How to calculate the space used by files in Linux
---
<figure>
	<img src="{{ site.baseurl }}/images/question.png">
	<figcaption>Photo Credit: <a href="http://commons.wikimedia.org/wiki/File:Gnome-dialog-question.svg" title="Wikimedia Commons"> Wikimedia Commons</a></figcaption>
</figure>

Suppose that you want to calculate the space used by all the png files in current directory and all its subdirectories. From a terminal type:

{% highlight bash %}
find . -name '*.png' -exec du -ab {} \; | awk '{total+=$0}END{print total}'
{% endhighlight %}
