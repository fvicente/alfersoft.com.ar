---
id: 128
title: How do I compare two binary files on Linux?
date: 2010-02-19T14:51:00+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=128
permalink: /2010/02/19/how-do-i-compare-two-binary-files-on-linux/
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
---
<img class="alignnone" title="FAQ" src="http://www.alfersoft.com.ar/files/question.png" alt="" width="48" height="48" />

The easiest way I found is dumping the binaries into text files using hexdump and then comparing them with your favourite program (diff, Meld, etc.). E.g.:
  
`<img class="alignnone" title="FAQ" src="http://www.alfersoft.com.ar/files/question.png" alt="" width="48" height="48" />

The easiest way I found is dumping the binaries into text files using hexdump and then comparing them with your favourite program (diff, Meld, etc.). E.g.:
  
` 

<!--more-->

###### <a title="Lock" href="http://commons.wikimedia.org/wiki/File:Gnome-dialog-question.svg" target="_blank">Image source</a>
