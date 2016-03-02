---
uid: 345
title: Revenge of the Tiny Pong VGA, now controlled with a single button
date: 2012-01-22T01:09:16+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=345
permalink: /2012/01/22/revenge-of-the-tiny-pong-vga-now-controlled-with-a-single-button/
categories:
  - Electronic Projects
tags:
  - assembler
  - attiny
  - attiny45
  - avr
  - pong
  - tiny
  - VGA
  - video
comments: true
---
[<img src="{{ site.url }}/images/tinypong/push_button.png" alt="Tiny Pong VGA push button" title="Tiny Pong VGA push button">]({{ site.url }}/images/tinypong/push_button.png)

As it was suggested by <a href="http://hackaday.com/2011/10/07/8-pin-micro-plays-pong-on-you-widescreen/" title="Hackaday Tiny Pong VGA" target="_blank">Hackaday&#8217;s guys</a>, I&#8217;ve added a simple push button in the only available pin of my ATtiny45 in order to control the Tiny Pong VGA. The switch toggles the paddle direction up and down, every time you release it.

I&#8217;ve made some little changes in the code, so check it out, you might find something interesting or useful. As always, source code, schematics, etc. freely available for download.

<!--more-->

## Hardware

Compared to <a href="{{ site.url }}/2011/09/19/tiny-pong-more-fun-with-attiny45-and-vga/" title="Tiny Pong: More fun with ATtiny45 and VGA" target="_blank">Tiny Pong 1.0</a>, I just added a push button on PB5 with a 10K ohms resistor as pull-down.

Here is a picture of the circuit on the breadboard.

<figure style="text-align: center;">
	<a title="Tiny Pong VGA on the protoboard" href="{{ site.url }}/images/tinypong/tinypong_with_button.jpg" target="_blank"><img src="{{ site.url }}/images/tinypong/tinypong_with_button.jpg" alt="Tiny Pong VGA on the protoboard" title="Tiny Pong VGA on the protoboard"/></a>
	<figcaption>Tiny Pong VGA on the protoboard</figcaption>
</figure>

And a prototype I made with <a href="http://fritzing.org/" title="Fritzing" target="_blank">Fritzing</a>

<figure style="text-align: center;">
	<a title="Tiny Pong VGA - Fritzing" href="{{ site.url }}/images/tinypong/tinypong_fritzing.png" target="_blank"><img src="{{ site.url }}/images/tinypong/tinypong_fritzing.png" alt="Tiny Pong VGA - Fritzing" title="Tiny Pong VGA - Fritzing"/></a>
	<figcaption>Tiny Pong VGA - Fritzing</figcaption>
</figure>

<figure style="text-align: center;">
	<a title="VGA pinout" href="{{ site.url }}/images/tinypong/vga_pinout.jpg" target="_blank"><img src="{{ site.url }}/images/tinypong/vga_pinout.jpg" alt="VGA pinout" title="VGA pinout"/></a>
	<figcaption>VGA pinout</figcaption>
</figure>

## ATtiny firmware

To use PB5 as an I/O port we need to reprogram the ATtiny fuses with some specific flags. Once you do this, you won&#8217;t be able to reprogram it anymore using a regular ISP programmer. If you need to flash another firmware, you will need to reset the fuses using a High Voltage programmer; <a href="http://www.rickety.us/2010/03/arduino-avr-high-voltage-serial-programmer/" title="AVR High Voltage programmer" target="_blank">this one</a> worked fine for me.

AVR GNU toolchain is used for this project, so the code is written in GNU assembler.

In order to build the firmware, I use the following script:

{% highlight bash %}
#!/bin/sh
avr-gcc -mmcu=attiny45 -mmcu=attiny45 -Wall -gdwarf-2 -Os -std=gnu99 -funsigned-char -funsigned-bitfields -fpack-struct -fshort-enums -MD -MP -MT pong.o -MF pong.o.d  -x assembler-with-cpp -Wa,-gdwarf2 -c pong.S tables.S
avr-gcc -mmcu=attiny45 -Wl,-Map=pong.map pong.o tables.o -o pong.elf
avr-objcopy -O ihex -R .eeprom -R .fuse -R .lock -R .signature pong.elf pong.hex
avr-objcopy -j .eeprom --set-section-flags=.eeprom="alloc,load" --change-section-lma .eeprom=0 --no-change-warnings -O ihex pong.elf pong.eep || exit 0
avr-size -C --mcu=attiny45 pong.elf
rm *.o
rm *.o.d
rm pong.eep
rm pong.elf
rm pong.map
{% endhighlight %}

And to upload it to the chip (note that I&#8217;m using the Arduino with mega-isp) I use the following script

{% highlight bash %}
#!/bin/sh
[ -e "/dev/tty.usbserial-A8008VmU" ] && PORT=/dev/tty.usbserial-A8008VmU || PORT=/dev/ttyUSB0
avrdude -F -P $PORT -p attiny45 -c avrisp -b 19200 -U flash:w:pong.hex
# fuses
# WARNING: fuses with PB5 as I/O port
#avrdude -F -P $PORT -p attiny45 -c avrisp -b 19200 -U flash:w:pong.hex -Ulfuse:w:0xce:m -Uhfuse:w:0x5f:m -Uefuse:w:0xff:m
{% endhighlight %}

Uncomment the last line to program the fuses.

## Code

The source code is almost the same as the first version, but I fixed a few bugs. First, the Bresenham&#8217;s algorithm was wrongly implemented, and since I needed it only to calculate the next position of the ball when the angle is 22.5 or 67.5, I realized that was easier to simply increment the X (or Y) coordinate when the ball is at an even position.

Regarding the button reading, it is done during the vertical blanking zone, 60 times per second which gives a pretty good responsiveness &#8211; see the `chkinput` label in the code for more details.

<iframe width="480" height="360" src="http://www.youtube.com/embed/VMZvNSRKmWE" allowfullscreen frameborder="0"></iframe>
---

<a title="Download Tiny Pong 2.0" markdown="0" href="https://github.com/fvicente/tinypong/archive/v2.0.zip" class="btn">Download Tiny Pong 2.0 Source</a>

[Tiny Pong on GitHub](https://github.com/fvicente/tinypong "Tiny Pong on GitHub"){:target="_blank"}

Enjoy!
