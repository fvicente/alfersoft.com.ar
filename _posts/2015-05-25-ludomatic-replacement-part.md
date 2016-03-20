---
uid: 744
title: Ludomatic Replacement Part
date: 2015-05-25T19:13:16+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=744
permalink: /2015/05/25/ludomatic-replacement-part/
categories:
  - 3D Printer
tags:
  - 3d printer
  - ludomatic
  - part
  - replacemente
comments: true
excerpt: This is a very simple model for printing Ludomatic part replacement. Ludomatic is a popular variation of Ludo
---
This is a very simple model for printing Ludomatic part replacement. Ludomatic is a popular variation of [Ludo](http://es.wikipedia.org/wiki/Ludo).

{% highlight python %}
rotate(180, [0, 1, 0]) {
	difference() {
		union() {
			cylinder(r = 5.9, h = 22.7, center = false);
		}
		cylinder(r = 4.4, h = 21.2, center = false);
	}
}
{% endhighlight %}

[<img src="{{ site.baseurl }}/images/ludomatic_openscad.png" alt="Ludomatic Part OpenSCAD"/>]({{ site.baseurl }}/images/ludomatic_openscad.png)

[<img src="{{ site.baseurl }}/images/ludomatic_01.jpg" alt="Ludomatic Part Replacement"/>]({{ site.baseurl }}/images/ludomatic_01.jpg)

[<img src="{{ site.baseurl }}/images/ludomatic_02.jpg" alt="Ludomatic Part Replacement"/>]({{ site.baseurl }}/images/ludomatic_02.jpg)

## Download

<a title="Download ludomatic.scad" markdown="0" href="{{ site.baseurl }}/files/ludomatic.scad" class="btn">Download ludomatic.scad</a>

[Thingiverse link](http://www.thingiverse.com/thing:847407){:target="_blank"}
