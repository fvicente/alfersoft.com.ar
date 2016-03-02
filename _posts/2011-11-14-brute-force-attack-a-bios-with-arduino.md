---
uid: 320
title: Brute force attack a BIOS with Arduino
date: 2011-11-14T21:55:27+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=320
permalink: /2011/11/14/brute-force-attack-a-bios-with-arduino/
dsq_thread_id:
  - 4582867533
categories:
  - Electronic Projects
tags:
  - Arduino
  - attack
  - bios
  - brute
  - Duemilanove
  - hack
  - password
comments: true
---
[<img src="{{ site.url }}/images/bios/bios_hack_01.jpg" alt="BIOS hack" title="BIOS hack"/>]({{ site.url }}/images/bios/bios_hack_01.jpg)

The goal of this experiment is to convert the Arduino board into an USB keyboard plus a VGA sniffer to crack the password of a standard BIOS using the [brute force attack](http://en.wikipedia.org/wiki/Brute-force_attack "Brute force attack") method. There are no advantages in using this method, in fact this can be very slow and you may never find the password at all, but as always we do it for fun. It&#8217;s just a proof of concept, there are many ways of resetting a BIOS specially if you have access to the hardware, and you need it anyway because we&#8217;re talking about BIOS and there is no &#8220;remote access&#8221; as far as I know.

<!--more-->

In theory, you can use it with other programs also not only a BIOS setup, but there must be some special conditions, for example the software must be one of those that doesn&#8217;t block after a few failed password entry attempts.

Also one of the main limitations is that we cannot read the whole VGA frame and process it, instead we read one single pixel from (more or less) the middle of the screen, and according to its color we go through the different steps, for example: a red pixel in the middle of the screen may indicate that the password is wrong in a regular BIOS setup, while a blue pixel can indicate that it is ready to receive the next password.


### USB Keyboard Emulator

For the USB keyboard part, I&#8217;ve used the <a href="http://code.google.com/p/vusb-for-arduino/" title="V-USB for Arduino" target="_blank">V-USB for Arduino</a> code, which in turns uses <a href="http://www.obdev.at/products/vusb/download.html" title="V-USB" target="_blank">V-USB library</a>. You will need to install the V-USB for Arduino to make the &#8220;pde&#8221; work.


### Circuit

The Arduino shield for this project is pretty simple, I&#8217;ve attached a regular LCD module to have an output to avoid a second computer just to see the progress or result.

A couple of Zener diodes to make the USB keyboard interface (it&#8217;s one of the four options suggested in the V-USB Readme, here is a <a href="http://www.practicalarduino.com/projects/virtual-usb-keyboard" title="Virtual USB Keyboard Arduino" target="_blank">link to another project</a> that uses this method also).

There is a button which is used to pause/continue the attack. If you keep the button pressed for more than 2 seconds, the attack will be reset.


[<img src="{{ site.url }}/images/bios/schematic.png" alt="BIOS hack schematic" title="BIOS hack schematic"/>]({{ site.url }}/images/bios/schematic.png)

[<img src="{{ site.url }}/images/bios/board.png" alt="BIOS hack board" title="BIOS hack board"/>]({{ site.url }}/images/bios/board.png)


### Sniff the VGA

To know the color of the pixel in the middle of the screen, we need to read the analog Red signal, and also the vertical and horizontal synch pulses to know when to read the Red. The first attempt was using Arduino&#8217;s `attachInterrupt` to capture the HSYNC and VSYNC but the overhead made the USB keyboard to stop working.

The ISR() and SIGNAL() macros seems to work better in this case, so the VSYNC pulse will reset a global variable called `h_line` while the HSYNC will increment it to know in which line is the VGA frame being drawn.

Our `waitWrongPassword` function does the analysis of the pixel. It waits for a few seconds to appear the red pixel, and keeps looking at the line counter so when it is in the #238 (almost the vertical middle in an 640&#215;480 resolution) it will delay a little bit to get the horizontal middle timing, and read the analog input.

Then, after reading the red analog input the result is compared to see if the &#8216;wrong password&#8217; dialog is popped, I&#8217;m talking about the `if (valueR > 140)`. You will probably **need** to change this value, according to your VGA card levels.

### Code

You need to define the character set that you want to use for the attack. To do this, modify the `charset` array adding the USB key codes you want to use. We only have `KEY_A, KEY_B, KEY_C` by default in the example code. Also, you need to modify a second array called `charset_log` which must have the same size, but instead of the key code it reflects the printable byte, for logging purposes.

The other thing you need to change is the maximum length of the password, by default set to 4. Look for the `MAX_LEN` define.

The state is periodically saved in the EEPROM so in case of power failure, you can continue from the last (or near the last) tested password.

Here some pictures of the shield

<figure class="half">
	<a title="BIOS Hack case" href="{{ site.url }}/images/bios/bios_hack_02.jpg" target="_blank"><img src="{{ site.url }}/images/bios/bios_hack_02.jpg" alt="An iPod box for the case" /></a>
	<a title="BIOS Hack circuit board" href="{{ site.url }}/images/bios/bios_hack_03.jpg" target="_blank"><img src="{{ site.url }}/images/bios/bios_hack_03.jpg" alt="Tone transfer method to make the board. A little bit burnt due to the excessive ironing." /></a>
</figure>

<figure class="half">
	<a title="BIOS Hack front view" href="{{ site.url }}/images/bios/bios_hack_04.jpg" target="_blank"><img src="{{ site.url }}/images/bios/bios_hack_04.jpg" alt="BIOS Hack front view" /></a>
	<a title="BIOS Hack back view" href="{{ site.url }}/images/bios/bios_hack_05.jpg" target="_blank"><img src="{{ site.url }}/images/bios/bios_hack_05.jpg" alt="BIOS Hack back view" /></a>
</figure>

---

<a title="Download BIOS Hack" markdown="0" href="https://github.com/fvicente/bios-hack/archive/master.zip" class="btn">Download Source Code</a>

[BIOS Hack on GitHub](https://github.com/fvicente/bios-hack "BIOS Hack on GitHub"){:target="_blank"}

Feel free to modify it if you need.

Bye!
