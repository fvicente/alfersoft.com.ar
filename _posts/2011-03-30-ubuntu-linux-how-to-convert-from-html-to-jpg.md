---
id: 202
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
[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/03/100px-Orange_Icon_Picture.svg_.png" alt="" title="Orange_Icon_Picture" width="100" height="100" class="alignleft size-full wp-image-210" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/03/100px-Orange_Icon_Picture.svg_.png) Here is an easy way to convert from HTML to JPG. Actually, I&#8217;m converting the HTML to PDF and then from PDF to JPG.
  
<!--more-->


  
First, install the following packages:
  
`sudo apt-get install python-pisa imagemagick python-imaging`

Then simply:
  
`xhtml2pdf report.html<br />
convert report.pdf report.jpg`

**Note:** if you receive an error message in python &#8220;AttributeError: &#8216;NoneType&#8217; object has no attribute &#8216;bands'&#8221; while trying to use the xhtml2pdf command, then you will need to apply this patch: http://hg.effbot.org/pil-2009-raclette/changeset/fb7ce579f5f9
  
In other words, edit Image.py and move line 1501 &#8220;self.load()&#8221; to 1497, just before the &#8220;if self.im.bands == 1:&#8221;
  
[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/03/Image_py_fix.png" alt="Image.py fix" title="Image_py_fix" width="450" height="104" class="alignleft size-medium wp-image-203" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/03/Image_py_fix-300x69.png 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/03/Image_py_fix.png 932w" sizes="(max-width: 450px) 100vw, 450px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/03/Image_py_fix.png)
