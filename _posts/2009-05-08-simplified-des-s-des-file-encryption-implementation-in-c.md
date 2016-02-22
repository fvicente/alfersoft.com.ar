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
<figure>
	<img title="Simplified DES" src="{{ site.url }}/images/lock.png" alt="Simplified DES"/>
	<figcaption>Photo Credit: <a href="http://upload.wikimedia.org/wikipedia/commons/f/ff/Crystal_Clear_action_lock.png" title="Wikimedia Commons"> Wikimedia Commons</a></figcaption>
</figure>

This encryption algorithm is not secure at all, in fact it was made for educational purposes. It uses a 10-bit key that can be quickly broken by a brute force attack. But, who knows, it may be good for someone interested in some quick encryption where security is not that important, or for embedded devices were resources are limited, or just for lazy students. 😀

<!--more-->

So, here is my version of the S-DES algorithm in plain C. Differently from <a title="S-DES cpp implementation" href="http://www.programmersheaven.com/download/47588/download.aspx" target="_blank">this other version in the web</a> my implementation is more byte-oriented while the other (probably more didactic) is converting the input to a string with &#8216;0&#8217;s and &#8216;1&#8217;s and working over it. Also, there is another implementation that uses some C++ functions while mine is written in plain C.

You can <a title="Simplified DES C implementation" href="https://github.com/fvicente/sdes/blob/master/sdes.c" target="_blank">take a look at the code online</a> (it is less than 300 lines) &#8212; released under BSD license.

To build it with gcc type:

{% highlight bash %}
gcc sdes.c -lm -o sdes
{% endhighlight %}

Note that I&#8217;m using math library only because of the pow() function that is used only in debug mode.

<a title="Download S-DES" markdown="0" href="https://github.com/fvicente/sdes/archive/master.zip" class="btn">Download Source Code</a>

Includes a paper by William Stallings explaining the algorithm.

[S-DES on GitHub](https://github.com/fvicente/sdes "S-DES on GitHub"){:target="_blank"}
