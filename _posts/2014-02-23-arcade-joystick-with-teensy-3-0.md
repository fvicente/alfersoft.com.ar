---
uid: 681
title: Arcade Joystick with Teensy 3.0
date: 2014-02-23T23:14:50+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=681
permalink: /2014/02/23/arcade-joystick-with-teensy-3-0/
categories:
  - Electronic Projects
tags:
  - arcade
  - joystick
  - keyboard
  - MAME
  - teensy
  - USB
comments: true
excerpt: DIY Arcade Joystick with Teensy
---
Do you have a spare Teensy 3.0, like I did, and you want to (under) use it? this might be a good one-afternoon DIY project: an Arcade Joystick.
  
Besides the ARM powered board, you will need a 4-switch arcade joystick, buttons, and a box that you need to still from your wife.

The firmware emulates an USB keyboard, so all you have to do is connect the switches from the stick and the buttons to the teensy digital inputs. No other external components are needed. For convenience, I&#8217;ve used inputs 0 to 7. I&#8217;m using only 4 push buttons, but you can easily extend it if you want, just adjust the `NUM_BUTTONS` and the `keys` array accordingly.

Joystick switches emulates the arrow keys, and the buttons, well, you need to program them to whatever fits better for your games, in my case [Enter], [Space], [Left Alt] and [Z] did the trick for Super Frog HD. I should think in putting an internal switch to change the button&#8217;s configuration to something more standard like the MAME layout.

Just for reference, this is the list of materials I&#8217;ve used for this project:

Qty|Name|Link|Price (U$S)|
--:|----|----|----------:|
1|Teensy 3.0|<a href="http://www.pjrc.com/store/teensy3.html" target="_blank">www.pjrc.com</a>|19.00|
1|4-Ways Red Ball Arcade Joystick with 4-Switch|<a href="http://dx.com/p/repair-parts-replacement-4-ways-red-ball-arcade-joystick-with-4-switch-37485#.UwqIFnkWyao" target="_blank">dx.com</a>|11.55|
4|Arcade Joystick Button|<a href="http://dx.com/p/sanwa-obsf-30-arcade-joystick-button-black-231437#.UwqIGnkWyao" target="_blank">dx.com</a> and <a href="http://dx.com/p/sanwa-obsf-30-arcade-joystick-button-white-231350#.UwqIG3kWyao" target="_blank">dx.com</a>|3.59 each|
1|Box|-||

## The Code

Of course, I am using the existing Teensy USB keyboard libraries, so the code is pretty simple &#8211; 32 lines of code.

{% highlight cpp %}
/* Arcade Keyboard-Joystick */

#include <usb_keyboard.h>

#define NUM_BUTTONS 8
int keys[NUM_BUTTONS] = {KEY_RIGHT, KEY_UP, KEY_DOWN, KEY_LEFT, KEY_ENTER, KEY_SPACE, KEY_LEFT_ALT, KEY_Z};
int mask = 0;
int i = 0;

void setup() {
	for (i = 0; i < NUM_BUTTONS; i++) {
		pinMode(i, INPUT_PULLUP);
	}
	delay(1000);
}

void loop() {
	for (i = 0; i < NUM_BUTTONS; i++) {
		if (digitalRead(i) == LOW) {
			if (!(mask & (1 << i))) {
				Keyboard.press(keys[i] | (0x40 << 8));
				mask |= (1 << i);
			}
		} else {
			if ((mask & (1 << i))) {
				Keyboard.release(keys[i] | (0x40 << 8));
				mask &= ~(1 << i);
			}
		}
	}
	delay(2);
}
{% endhighlight %}

[See joystick.ino on gist](https://gist.github.com/fvicente/515d08aabf5616f710cd)

## Connection Diagram

No explanation needed<figure id="attachment_700" style="width: 300px" class="wp-caption aligncenter">

<figure style="text-align: center;">
	<a href="{{ site.baseurl }}/images/joystick.png" target="_blank"><img src="{{ site.baseurl }}/images/joystick.png" alt="Joystick Connection Diagram" title="Joystick Connection Diagram"/></a>
	<figcaption class="wp-caption-text">Joystick Connection Diagram</figcaption>
</figure> 

## Pictures

<figure class="third">
	<a title="Joystick connections" href="{{ site.baseurl }}/images/joystick_01.jpg" target="_blank"><img src="{{ site.baseurl }}/images/joystick_01.jpg" alt="Joystick connections" /></a>
	<a title="Box holes" href="{{ site.baseurl }}/images/joystick_02.jpg" target="_blank"><img src="{{ site.baseurl }}/images/joystick_02.jpg" alt="Box holes" /></a>
	<a title="Inside Joystick" href="{{ site.baseurl }}/images/joystick_03.jpg" target="_blank"><img src="{{ site.baseurl }}/images/joystick_03.jpg" alt="Inside Joystick" /></a>
</figure>

<figure class="third">
	<a title="Inside Joystick" href="{{ site.baseurl }}/images/joystick_04.jpg" target="_blank"><img src="{{ site.baseurl }}/images/joystick_04.jpg" alt="Inside Joystick" /></a>
	<a title="Joystick" href="{{ site.baseurl }}/images/joystick_05.jpg" target="_blank"><img src="{{ site.baseurl }}/images/joystick_05.jpg" alt="Joystick" /></a>
	<a title="Joystick testing with uncle Alf" href="{{ site.baseurl }}/images/joystick_06.jpg" target="_blank"><img src="{{ site.baseurl }}/images/joystick_06.jpg" alt="Joystick testing with uncle Alf" /></a>
</figure>
