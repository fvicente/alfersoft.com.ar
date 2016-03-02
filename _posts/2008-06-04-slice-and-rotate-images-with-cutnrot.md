---
uid: 8
title: Slice and rotate images with CutNRot
date: 2008-06-04T20:24:19+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=8
permalink: /2008/06/04/slice-and-rotate-images-with-cutnrot/
categories:
  - Software Development
tags:
  - cut
  - images
  - pictures
  - python
  - rotate
  - scanned
  - wxpython
comments: true
---
<figure>
	<img src="{{ site.baseurl }}/images/cutnrot.jpg">
</figure>
Today I&#8217;m introducing another utility script written in python that uses wxPython to edit your scanned images (JPEG, BMP, TIFF, etc.) obtain sub-images, rotate and save them.

<!--more-->

This first version is very basic but effective, there is no necessity of learning complex commands or image handling programs, just:

* click in the upper-left corner
* click in the lower-right corner
* rotate if you wish
* close and save

The unfortunate name of this script is **cutnrot** 😀 and it is released under BSD license, download, modify and use it at your own risk, as usual.

<a title="Download Cut And Rotate" markdown="0" href="https://github.com/fvicente/cutnrot/archive/master.zip" class="btn">Download Source Code</a>

[cutnrot on GitHub](https://github.com/fvicente/cutnrot/ "cutnrot on GitHub"){:target="_blank"}

<a title="cutnrot Windows Binaries" markdown="0" href="{{ site.baseurl }}/files/cutnrot_win32_bin.zip" class="btn">Download Windows Binaries</a>

Windows binaries generated with <a title="py2exe" href="http://www.py2exe.org/" target="_blank">py2exe</a> for those that does not have a Python interpreter and/or wxPython installed.
