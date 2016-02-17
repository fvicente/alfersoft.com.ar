---
id: 103
title: Simplified DES (S-DES) file encryption implementation in C
date: 2009-05-08T17:34:11+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=103
permalink: /2009/05/08/simplified-des-s-des-file-encryption-implementation-in-c/
categories:
  - Software Development
tags:
  - C encryption S-DES simplified DES cipher
comments: true
---
<figure style="width: 128px" class="wp-caption alignnone"><img title="Simplified DES" src="http://www.alfersoft.com.ar/files/lock.png" alt="Simplified DES" width="128" height="128" /><figcaption class="wp-caption-text">Simplified DES</figcaption></figure> 

This encryption algorithm is not secure at all, in fact it was made for educational purposes. It uses a 10-bit key that can be quickly broken by a brute force attack. But, who knows, it may be good for someone interested in some quick encryption where security is not that important, or for embedded devices were resources are limited, or just for lazy students. 😀

<!--more-->

So, here is my version of the S-DES algorithm in plain C. Differently from the <a title="S-DES cpp implementation" href="http://www.programmersheaven.com/download/47588/download.aspx" target="_blank">other version in the web</a> my implementation is more byte-oriented while the other (probably more didactic) is converting the input to a string with &#8216;0&#8217;s and &#8216;1&#8217;s and working over it. Also the other implementation uses some C++ functions while mine is written in plain C.

<a title="Simplified DES C implementation" href="http://www.alfersoft.com.ar/files/sdes/sdes.c" target="_blank">Download it</a> and take a look to the code (it is less than 300 lines) &#8212; released under BSD license.

To build it with gcc type:

gcc sdes.c -lm -o sdes

Note that I&#8217;m using math library only because of the pow() function that is used only in debug mode.

And here is a copy of a <a title="S-DES algorithm PDF" href="http://www.alfersoft.com.ar/files/sdes/sdes.pdf" target="_blank">document explaining the algorithm</a>.

###### <a title="Lock" href="http://upload.wikimedia.org/wikipedia/commons/f/ff/Crystal_Clear_action_lock.png" target="_blank">Image source</a>
