---
id: 591
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
---
[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2013/04/spongebob-300x227.jpg" alt="spongebob" width="300" height="227" class="aligncenter size-medium wp-image-592" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2013/04/spongebob-300x227.jpg 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2013/04/spongebob.jpg 400w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2013/04/spongebob.jpg)

<!--more-->


  
Another reminder. TortoiseSVN, non-standard SSH port, Network Error: Connection Refused.

  * Edit C:\Users\user\AppData\Roaming\Subversion\config
  * Under [tunnels] section add:
  
    ssh = TortoisePlink.exe -v -P _**NNNN**_
  
    (where _**NNNN**_ is the non-standard port)
  * Proceed to checkout your project normally using SVN+SSH, like `svn+ssh://user@myserver:NNNN/svn/project`
