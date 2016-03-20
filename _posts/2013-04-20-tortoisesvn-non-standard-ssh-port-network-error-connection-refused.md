---
uid: 591
title: 'TortoiseSVN, non-standard SSH port, Network Error: Connection Refused'
date: 2013-04-20T02:08:19+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=591
permalink: /2013/04/20/tortoisesvn-non-standard-ssh-port-network-error-connection-refused/
categories:
  - Programming FAQ
  - Software Development
tags:
  - error
  - network
  - port
  - ssh
  - tortoisesvn
  - windows
comments: true
excerpt: How to configure TortoiseSVN to use a non-standard SSH port
---
[<img src="{{ site.baseurl }}/images/spongebob.jpg" alt="spongebob"/>]({{ site.baseurl }}/images/spongebob.jpg)

<!--more-->

Another reminder. TortoiseSVN, non-standard SSH port, Network Error: Connection Refused.

* Edit C:\Users\user\AppData\Roaming\Subversion\config
* Under [tunnels] section add:

{% highlight INI %}
ssh = TortoisePlink.exe -v -P NNNN
{% endhighlight %}
(where _**NNNN**_ is the non-standard port)

* Proceed to checkout your project normally using SVN+SSH, like `svn+ssh://user@myserver:NNNN/svn/project`
