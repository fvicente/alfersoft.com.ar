---
id: 586
title: Enabling GoogleAppEngineLauncher Source Code Change Detection in Mac OS X Lion
date: 2013-04-05T02:17:20+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=586
permalink: /2013/04/05/enabling-googleappenginelauncher-source-code-change-detection-in-mac-os-x-lion/
categories:
  - Software Development
tags:
  - appengine
  - appserver
  - code change
  - devappserver
  - google
  - googleappenginelauncher
  - macos
comments: true
---
Quick recipe to enable source code change detection on the GoogleAppEngineLauncher (Mac OS X Lion).
  
If you take a look to the app engine dev server logs, you&#8217;ll probably see the following message:
  
`Quick recipe to enable source code change detection on the GoogleAppEngineLauncher (Mac OS X Lion).
  
If you take a look to the app engine dev server logs, you&#8217;ll probably see the following message:
  
` 
  
That basically means that you need to install PyObjC in your python environment. Turns out it is not that straight forward since neither `pip install pyobjc` nor `easy_install pyobjc` seems to work, at least in my machine, so I googled and found the following step-by-step.
  
<!--more-->


  
This link should be the answer to your problems: <a href="https://github.com/pypa/pip/issues/11" title="pip doesn't install PyObjC" target="_blank">https://github.com/pypa/pip/issues/11</a>
  
HOWEVER -obviously there is a but, otherwise I wouldn&#8217;t be writing this down- I needed to modify the step-by-step for me, because I wanted to install PyObjC in my local Python 2.7 environment. Here it goes:

1) `sudo pip-2.7 install distribute`
  
2) `sudo pip-2.7 install -U pyobjc` &#8212; You will get an error
  
3) `sudo sed -i.bak 's/use_2to3 = True/use_2to3 = False/g' ./build/pyobjc-core/setup.py`
  
4) `cd build/pyobjc-core/`
  
5) `sudo python setup.py install`

That&#8217;s it!
