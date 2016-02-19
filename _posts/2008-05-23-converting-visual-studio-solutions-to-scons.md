---
id: 5
title: Converting Visual Studio solutions to SCons
date: 2008-05-23T20:19:46+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=5
permalink: /2008/05/23/converting-visual-studio-solutions-to-scons/
categories:
  - Software Development
tags:
  - code
  - development
  - python
  - scons
comments: true
---
A python script to convert Microsoft Visual Studio 2005 solution files (\*.sln) and the associated project files (\*.vcproj) into a set of SCons files (SConstruct and SConscript).

<!--more-->

This script is similar to my previous post Sln2Make but instead of generating Makefiles it creates SCons scripts. The class Sln2SCons does all the work, parses the sln and vcproj files and generates a main SConstruct and one SConscript for every project in the solution. To use it, just instantiate the class with the following parameters:

`Sln2SCons(slnpath, exlist, dirrepl, librepl, outdir)`

where:

  * **slnpath** is the path to the solution file
  * **exlist** an optional list of files to exclude, optionally you can pass an alternative script name to be included from the main SConstruct: [[script\_to\_exclude, alt_script], &#8230;]
  * **dirrepl** a list of directory replacement rules
  * **librepl** library name replacement rules (when win library name doesn&#8217;t match OS&#8217;s library name)
  * **outdir** is the output directory for the SConstruct

example:

{% highlight python %}
# custom scripts
exlist = [('../extsrc/apr/SConscript', 'extsrc/apr/SConscript.py')]
# special directory replacement rules
dirrepl = []
# special library name replacement rules (when win library name doesn't match OS's library name)
librepl = []
Sln2SCons("../winnt/test.sln", exlist, dirrepl, librepl, "../")
{% endhighlight %}

To correct the path case, I&#8217;ve used a script published by Moshe Zadka [here](http://mail.python.org/pipermail/python-list/2000-June/038502.html "Case Correction in Python").

Use this code at your own risk, it is released under BSD license.

<a title="sln2scons on GitHub" markdown="0" href="https://github.com/fvicente/sln2scons/archive/master.zip" class="btn">Download Source Code</a>
