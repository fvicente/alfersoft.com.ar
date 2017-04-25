---
uid: 771
title: Oldies - Radio Times
date: 2017-04-24T00:00:00+00:00
author: fvicente
layout: post
guid: ?p=771
categories:
  - Ham Radio
  - Amateur Radio
  - Software Development
tags:
  - ham radio
  - oldies
  - clipper
  - vb
comments: true
excerpt: Old radiocommunication-related programs
---
A collection of old programs I made in several languages like Basic, Clipper and Visual Basic 6.0.

<!--more-->

## How to build and run these programs

In order to build and run Basic and Clipper programs, I&#8217;ve downloaded [DOSBox](https://www.dosbox.com/) which is a D.O.S. emulator that runs on Mac OS X and other platforms.

Also, I&#8217;ve found installers for CA-Clipper on a site called [WinWorld](https://winworldpc.com/download/9175DEEA-5A91-11E5-8FA3-C86000DD9ED6), and Quick Basic 4.5 from [http://www.phatcode.net/](http://www.phatcode.net/downloads.php?id=172&action=get&file=qb45.zip)

DOSBox allows you to mount directories of your local machine as drives. So, I&#8217;ve created two directories in my home folder and mounted them as A: and C:.

On DOSBox run:
{% highlight bat %}
Z:\>mount a ~/temp
Z:\>mount c ~/drive
{% endhighlight %}

When you extract the downloaded CA-Clipper installer, you will see two disk images disk01.img and disk02.img.
Then mount locally disk01.img and copy the files inside the image to your ~/temp. Mount disk02.img and copy the content to ~/temp, but without overwriting any existing file (you can press "Keep both" on Finder).
Press Ctrl + F4 (or Ctrl + fn + F4) in order to force DOSBox to rescan the directory.

To run the installer, go back to DOSBox and type:
{% highlight bat %}
Z:\>a:
A:\>install
{% endhighlight %}
Follow the instructions of the installer. I&#8217;ve installed it on C: drive, using default path \CLIPPER5

Add Clipper to the PATH environment variable, so you can build the sources from any directory
{% highlight bat %}
A:\>c:\AUTOEXEC.BAT
{% endhighlight %}


## Maxon SM-3010 Frequency / Diode Matrix calculator

Maxon SM-3010 is a VHF radio transceiver whose channel&#8217;s frequencies are programmed through an internal diode matrix, you either open or soldier diodes in order to program a frequency.

There was a technical manual with lots of printed pages with all the possible frequencies, which was very annoying at the time. Using some deduction, we figured out the algorithm to calculate the matrix.

This Clipper program (named just 3010) was used to calculate the frequency given the current state of the diode matrix (open / closed), and also the other way around, given a frequency, display diode state.

Also have a nice 3 page screen showing some of the equipment features. All in Spanish :D

Code is from September 1996, so don&#8217;t expect too much. 

To build the code, copy the source (see GitHub link below), to your ~/drive and then run:
{% highlight bat %}
A:\>c:
C:\>cl 3010
... wait for build to finish
C:\>3010.EXE
{% endhighlight %}

## Maxon SM-3010 Pictures found on the Internet

[<img src="{{ site.baseurl }}/images/3010.jpg" alt="Maxon SM-3010 Equipment (picture found on the Internet)"/>]({{ site.baseurl }}/images/3010.jpg)

[<img src="{{ site.baseurl }}/images/3010_diode_board.jpg" alt="Maxon SM-3010 Diode Board (picture found on the Internet)"/>]({{ site.baseurl }}/images/3010_diode_board.jpg)


## 3010 Software Screenshots

<figure class="half">
	<a href="{{ site.baseurl }}/images/3010_01.png" target="_blank"><img src="{{ site.baseurl }}/images/3010_01.png" alt="3010 Screenshot 1 on DOSBox" title="3010 Screenshot 1"/></a>
	<a href="{{ site.baseurl }}/images/3010_02.png" target="_blank"><img src="{{ site.baseurl }}/images/3010_02.png" alt="3010 Screenshot 2 on DOSBox" title="3010 Screenshot 2"/></a>
</figure>

<figure class="half">
	<a href="{{ site.baseurl }}/images/3010_03.png" target="_blank"><img src="{{ site.baseurl }}/images/3010_03.png" alt="3010 Screenshot 3 on DOSBox" title="3010 Screenshot 3"/></a>
	<a href="{{ site.baseurl }}/images/3010_04.png" target="_blank"><img src="{{ site.baseurl }}/images/3010_04.png" alt="3010 Screenshot 4 on DOSBox" title="3010 Screenshot 4"/></a>
</figure>

<figure class="half">
	<a href="{{ site.baseurl }}/images/3010_05.png" target="_blank"><img src="{{ site.baseurl }}/images/3010_05.png" alt="3010 Screenshot 5 on DOSBox" title="3010 Screenshot 5"/></a>
	<a href="{{ site.baseurl }}/images/3010_06.png" target="_blank"><img src="{{ site.baseurl }}/images/3010_06.png" alt="3010 Screenshot 6 on DOSBox" title="3010 Screenshot 6"/></a>
</figure>


[3010 Source Code on GitHub](https://github.com/fvicente/oldies/tree/master/3010)
