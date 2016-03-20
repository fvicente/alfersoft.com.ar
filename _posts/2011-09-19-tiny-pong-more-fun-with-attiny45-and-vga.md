---
uid: 284
title: 'Tiny Pong: More fun with ATtiny45 and VGA'
date: 2011-09-19T23:54:13+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=284
permalink: /2011/09/19/tiny-pong-more-fun-with-attiny45-and-vga/
categories:
  - Electronic Projects
tags:
  - assembler
  - atmel
  - attiny
  - attiny45
  - avr
  - fun
  - game
  - pong
  - VGA
  - video
comments: true
excerpt: VGA Pong game based on ATtiny microcontroller
---
I&#8217;m still waiting for my cheap [Bluetooth module from China](http://www.dealextreme.com/p/wireless-bluetooth-rs232-ttl-transceiver-module-80711 "Bluetooth Module") which will serve as an input interface for my [scoreboard project]({{ site.baseurl }}/2011/08/30/scoreboard-part-1-vga-signal-from-an-attiny45/ "Scoreboard (Part 1: VGA signal from an ATtiny45)"). In the meantime, I&#8217;ll show you how to convert your ATtiny microcontroller into a Pong game (with no input so far).

<figure class="half">
	<a title="Tiny Pong" href="{{ site.baseurl }}/images/tinypong/tinypong_01.jpg" target="_blank"><img src="{{ site.baseurl }}/images/tinypong/tinypong_01.jpg" alt="" title="Tiny Pong"/></a>
	<a title="Tiny Pong in the protoboard" href="{{ site.baseurl }}/images/tinypong/tinypong_02.jpg" target="_blank"><img src="{{ site.baseurl }}/images/tinypong/tinypong_02.jpg" alt="" title="Tiny Pong in the protoboard"/></a>
</figure>

<!--more-->

## Schematic

So, I&#8217;ve used the scoreboard source as a base and changed a little bit the pinout.

{% highlight python %}
                       ATtiny45
                  +-----------------+
                  |                 |
           IN ----| 1 (PB5) (VCC) 8 |---- +5V
    22pf          |                 |
  +--||----+------| 2 (PB3) (PB2) 7 |---- HSYNC
  |       [ ] XTAL|                 |
  +--||----+------| 3 (PB4) (PB1) 6 |---- VSYNC
  | 22pf          |                 |
  +---------------| 4 (GND) (PB0) 5 |---- RGB
  |               |                 |
 GND              +-----------------+
{% endhighlight %}

Now the RGB is connected to PB0, and there is a good reason for this. I&#8217;m still using the same technique of storing what I want to render in registers but instead of 4, this time I&#8217;m using 15 so I can achieve an horizontal resolution of 120 by 96 to make the pixels somehow squared. Now, to be able to walk trough the 120 bits and turn the RGB pin on/off accordingly (and evenly) I needed to crop the code, removing the loops (so you will see a lot of similar code in the part that renders the line) and the conditional skip now replaced by an &#8220;add with carry&#8221; after the shift into a temporary register that will be used with &#8220;out&#8221; which is less expensive than &#8220;sbi&#8221; and &#8220;cbi&#8221;.

So, in terms of code optimization, this:

{% highlight nasm %}
	; r1 bit 0
	cbr r16, 1
	lsl r1
	adc r16, r22
	out PORT, r16
	... repeated 120 time (8 times per bit and 15 times per register)
{% endhighlight %}

Is better than:

{% highlight nasm %}
	ldi r16, 0x08
line44:
	rol r8
	brcc rgboff44
	nop
	; RGB on
	sbi PORT, RGB		; sbi = 2 clocks
	rjmp cont44
rgboff44:
	; RGB off
	cbi PORT, RGB		; cbi = 2 clocks
	nop
	nop
cont44:
	dec r16
	nop
	nop
	nop
	nop
	nop
	brne line44
{% endhighlight %}

There are also other parts of the code that might be of interest. For example, I&#8217;ve use [LFSR](http://en.wikipedia.org/wiki/Linear_feedback_shift_register "Linear feedback shift register") to add some pseudo-random variables to the ball direction and the paddle &#8220;computer&#8221; movements. Also, I&#8217;ve used the [Bresenham&#8217;s line algorithm](http://en.wikipedia.org/wiki/Bresenham's_line_algorithm "Bresenham's line algorithm") to determine the ball position.

The missing part, is still the input. I&#8217;m not sure how this will work with only one pin available, but I guess I&#8217;ll work out something with the Bluetooth module and one of the synch signals (if even possible).

I&#8217;ve tried to add some intro screen or &#8220;splash&#8221;, but the program memory is so small that I&#8217;ve quickly exceeded the 4096 available bytes.

<iframe width="480" height="360" src="http://www.youtube.com/embed/8KlHqu1tnMg" allowfullscreen frameborder="0"></iframe>

---

<a title="Download Tiny Pong 1.0" markdown="0" href="https://github.com/fvicente/tinypong/archive/v1.0.zip" class="btn">Download Tiny Pong 1.0 Source</a>

[Tiny Pong on GitHub](https://github.com/fvicente/tinypong "Tiny Pong on GitHub"){:target="_blank"}
