---
id: 8
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
<div id="wikicontent" style="padding: 0pt 3em 1.2em 0pt;">
  <p>
    <img src="http://www.alfersoft.com.ar/files/cutnrot.jpg" alt="Cut and Rotate" width="400" height="150" />
  </p>
  
  <p>
    Today I&#8217;m introducing another utility script written in python that uses wxPython to edit your scanned images (JPEG, BMP, TIFF, etc.) obtain sub-images, rotate and save them.
  </p>
  
  <p>
    <!--more-->This first version is very basic but effective, there is no necessity of learning complex commands or image handling programs, just:
  </p>
  
  <ul>
    <li>
      click in the upper-left corner
    </li>
    <li>
      click in the lower-right corner
    </li>
    <li>
      rotate if you wish
    </li>
    <li>
      close and save
    </li>
  </ul>
</div>

<div id="wikicontent" style="padding: 0pt 3em 1.2em 0pt;">
  The unfortunate name of this script is cutnrot 😀 and it is released under BSD license, download, modify and use it at your own risk, as usual. <a title="Cut And Rotate" href="http://code.google.com/p/cutnrot/" target="_blank">Here is the Google Code page for this tiny project.</a>
</div>

<div style="padding: 0pt 3em 1.2em 0pt;">
  <a title="cutnrot Windows Binaries" href="http://cutnrot.googlecode.com/files/cutnrot_win32_bin.zip">Download Windows Binaries</a> generated with <a title="py2exe" href="http://www.py2exe.org/" target="_blank">py2exe</a> for those that does not have a Python interpreter and/or wxPython installed.
</div>
