---
uid: 202
title: 'Ubuntu Linux: How to Convert from HTML to JPG'
date: 2011-03-30T12:03:41+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=202
permalink: /2011/03/30/ubuntu-linux-how-to-convert-from-html-to-jpg/
dsq_thread_id:
  - 4582867518
categories:
  - Linux FAQ
  - Software Applications
tags:
  - convert
  - html
  - jpeg
  - jpg
  - linux
  - ubuntu
  - xhtml
comments: true
---
<figure>
	<img src="{{ site.baseurl }}/images/question.png">
	<figcaption>Photo Credit: <a href="http://commons.wikimedia.org/wiki/File:Gnome-dialog-question.svg" title="Wikimedia Commons"> Wikimedia Commons</a></figcaption>
</figure>

Here is an easy way to convert from HTML to JPG. Actually, I&#8217;m converting the HTML to PDF and then from PDF to JPG.

<!--more-->

First, install the following packages:

{% highlight bash %}
sudo apt-get install python-pisa imagemagick python-imaging
{% endhighlight %}

Then simply:

{% highlight bash %}
xhtml2pdf report.html
convert report.pdf report.jpg
{% endhighlight %}

**Note:** if you receive an error message in python &#8220;AttributeError: &#8216;NoneType&#8217; object has no attribute &#8216;bands'&#8221; while trying to use the xhtml2pdf command, then you will need to apply this patch: [http://hg.effbot.org/pil-2009-raclette/changeset/fb7ce579f5f9](http://hg.effbot.org/pil-2009-raclette/changeset/fb7ce579f5f9)

In other words, edit Image.py and move line 1501 `self.load()` to 1497, just before the `if self.im.bands == 1:`

[<img src="{{ site.baseurl }}/images/image_py_fix.png" alt="Image.py fix" title="Image py fix"/>]({{ site.baseurl }}/images/image_py_fix.png)
