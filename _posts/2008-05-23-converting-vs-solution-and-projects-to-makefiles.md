---
id: 4
title: Converting Visual Studio solutions to Makefiles
date: 2008-05-23T00:27:05+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=4
permalink: /2008/05/23/converting-vs-solution-and-projects-to-makefiles/
categories:
  - Software Development
tags:
  - convert
  - development
  - make
  - makefile
  - visual studio
comments: true
---
A python script to convert Microsoft Visual Studio 2005 solution files (\*.sln) and the associated project files (\*.vcproj) into a set of Makefile&#8217;s.

The class Sln2Make does all the work, parses the sln and vcproj files and generates a main Makefile and one Makefile.ag for every project in the solution. The Makefiles.ag generated will have two targets, &#8216;all&#8217; and &#8216;clean&#8217;, the script also includes the dependent libraries if they are properly defined in the solution. To use it, just instantiate the class with the following parameters:

<!--more-->

`Sln2Make(slnpath, exlist, dirrepl, librepl)`

where:

  * **slnpath** is the path to the solution file
  * **exlist** an optional list of files to exclude, optionally you can pass the content of an alternative make file using this syntax: [[dest, make\_all\_replacement, make\_clean\_replacement], &#8230;]
  * **dirrepl** a list of directory replacement rules
  * **librepl** library name replacement rules (when win library name doesn&#8217;t match OS&#8217;s library name)

example:

{% highlight python %}
exlist = [
    (
        '../extsrc/zlib/projects/visualc6/Makefile.ag',
        '$(MAKE) -C $(dir_root)extsrc/zlib -f Makefile libz.a\n',
        '$(MAKE) -C $(dir_root)extsrc/zlib -f Makefile clean\n'
    )
]
dirrepl = [['winnt', 'linux']]
librepl = [['zlib', 'z'], ['nspr', 'nspr4 -lplc4 -lplds4']]
#
Sln2Make("../winnt/test.sln", exlist, dirrepl, librepl)
{% endhighlight %}

To correct the path case, I&#8217;ve used a script published by Moshe Zadka [here](http://mail.python.org/pipermail/python-list/2000-June/038502.html "Case Correction in Python").

Use this code at your own risk, it is released under BSD license.

<a title="sln2scons on GitHub" markdown="0" href="https://github.com/fvicente/sln2make/archive/master.zip" class="btn">Download Source Code</a>
