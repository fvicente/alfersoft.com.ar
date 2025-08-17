---
uid: 145
title: 'Firefox add-on: UploadProgress'
date: 2010-08-26T19:37:48+00:00
author: fvicente
layout: single
guid: ?p=145
categories:
  - Software Applications
  - Software Development
tags:
  - add-on
  - component
  - firefox
  - meter
  - plugin
  - progress
  - upload
comments: true
excerpt: Firefox add-on to display file upload progress
---
<figure>
	<img alt="UploadProgress add-on" src="{{ site.baseurl }}/images/upload.png" title="UploadProgress add-on">
</figure>

This Firefox add-on adds a new option to the Tools menu called &#8220;Uploads&#8221; that displays a small window, similar to the downloads, but displaying only current uploads in progress. The uploads are automatically removed from the window after they finish. The idea is to have a way to know the progress of your file uploads and an estimated remaining time to finish.

Useful for sites that does not shows the upload progress like youtube.

This is my first add-on, so if you find something wrong please let me know.

I&#8217;ve submitted it to [AMO](https://addons.mozilla.org/) <del datetime="2010-10-03T02:48:06+00:00">but since it takes time to get released to the general public, I&#8217;ve decided to put it here in our blog if you want to give it a try.<br /> </del>

[Download it here](https://addons.mozilla.org/en-US/firefox/addon/221510/), enjoy!

<!--more-->


Tested in Linux, MacOS X and Windows, this small add-on is 100% JavaScript, no dlls no native XPCOMs. I&#8217;ve read in some forums that once upon a time (2004) Firefox already included an upload progress indicator but since then till today is broken. Today other modern browsers like Chrome already provides an upload percent indicator.

Edit: version 0.3 now displaying a small progress bar in Firefox&#8217;s status-bar. Check it out!

[uploadprogress on GitHub](https://github.com/fvicente/uploadprogress "uploadprogress on GitHub"){:target="_blank"}

Now accepting contributions 😀
