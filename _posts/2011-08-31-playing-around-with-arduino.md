---
uid: 277
title: Playing around with Arduino
date: 2011-08-31T01:10:03+00:00
author: fvicente
layout: single
guid: ?p=277
categories:
  - Electronic Projects
tags:
  - Arduino
  - Duemilanove
  - opinion
  - review
comments: true
excerpt: Some quick thoughts about the Arduino Duemilanove board
---
Some quick thoughts about the <a href="http://www.arduino.cc/en/Main/ArduinoBoardDuemilanove" title="Arduino Duemilanove Board" target="_blank">Arduino Duemilanove</a> board.

[<img src="{{ site.baseurl }}/images/duemilanove.jpg" alt="Duemilanove" title="Duemilanove"/>]({{ site.baseurl }}/images/duemilanove.jpg)

<!--more-->

## First Impressions

I&#8217;ve got one of these boards like a month ago, and I can tell you so far that I like many things about it. First of all the simplicity of plugging the USB cable and get things working almost instantly. I don&#8217;t think is necessary to explain anything in this blog since the Internet is plenty of Arduino tutorials, examples, projects, documents, forums, etc. If you just got one of this boards, your starting point should be here: <a href="http://arduino.cc/en/Guide/HomePage" title="Arduino Home Page" target="_blank">http://arduino.cc/en/Guide/HomePage</a>

So, first thing you will do is download the Arduino software, install it and try to upload one of the sample programs already included like the blinking led, using the provided IDE.

## Easy

If you are impatient like me, you will probably read the first page and then jump to the board and try to connect components, like a led or maybe a 16&#215;2 LCD display, etc. and you will quickly discover that it IS really easy to use: the core Arduino API already have functions to handle LCD displays, interruptions are trivial with &#8220;attachInterrupt&#8221;, then with the &#8220;shiftOut&#8221; function you can extend your ports by attaching one or more 74HC595, and many other features.

## First Depressions

On the other hand, I don&#8217;t like the IDE, maybe because I&#8217;m adult?. If you are used to any other IDE, like eclipse, or notepad.exe (he he), you may not like the IDE too. I mean, if you select a text and press Ctrl+F, what do you expect to appear in the search box? well, according to this IDE, nothing or the text from the last search. Not to talk about the annoying (Java?) bug that doesn&#8217;t allows you to use dead keys with some specific combinations of JDK and OS.

Then you have the Sketches concept. Sketches, are files with .pde extension which happens to be actually C++ files. Before building this file, the IDE instruments it to add a core include &#8220;WProgram.h&#8221; and prototypes all your functions. Then the IDE will link with the core files which already includes a very simple main() that calls the init(), setup() and loop() functions, as you can see in the core Arduino source code main.cpp:

{% highlight cpp %}
#include <WProgram.h>

int main(void)
{
	init();

	setup();

	for (;;)
		loop();

	return 0;
}
{% endhighlight %}

## A more advanced test

I&#8217;ve been able to convert the Arduino into an USB keyboard with a few external components following the example from <a href="http://www.practicalarduino.com/projects/virtual-usb-keyboard" title="Arduino Virtual USB Keyboard" target="_blank">www.practicalarduino.com</a> which uses the <a href="http://code.google.com/p/vusb-for-arduino/" title="V-USB for Arduino" target="_blank">Arduino V-USB library</a> which in turns uses the <a href="http://www.obdev.at/products/vusb/index.html" title="V-USB AVR" target="_blank">Virtual USB Port for AVR microcontrollers</a>. This will be the base for one of my upcoming project involving brute-force attack BIOS hacking :).

## Shields

Finally, I would like to recommend this shield if you need an ISP programmer: <a href="http://drug123.org.ua/mega-isp-shield/" title="Mega-ISP Shield" target="_blank">The Mega-ISP Shield</a>.

I made it, it works and is great! really. With this shield you can convert your Arduino into an ISP programmer by loading the <a href="http://code.google.com/p/mega-isp/" title="mega-isp" target="_blank">mega-isp</a> &#8220;sketch&#8221;.

[<img src="{{ site.baseurl }}/images/mega_isp_shield.jpg" alt="Mega-ISP Shield" title="Mega-ISP Shield"/>]({{ site.baseurl }}/images/mega_isp_shield.jpg)

As a side note, you will notice that you won&#8217;t be able to create a shield using the regular perforated boards, because of the pin alignment. Unless you only need pins 1 to 7 Xor 8 to 13 from the D port.
