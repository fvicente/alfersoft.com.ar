---
id: 552
title: 'Yet another conversor board for generic 16&#215;2 line displays to I2C'
date: 2013-04-05T01:10:30+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=552
permalink: /2013/04/05/yet-another-conversor-board-for-generic-16x2-line-displays-to-i2c/
categories:
  - Electronic Projects
tags:
  - Arduino
  - board
  - conversor
  - Eagle Cad
  - I2C
  - lbr
  - LCD
  - MGD1602B
  - module
comments: true
---
<a href="{{ site.url }}/images/lcd_i2c.jpg" target="_blank"><img src="{{ site.url }}/images/lcd_i2c.jpg" alt="LCD to I2C" title="LCD to I2C"/></a>

If you are trying to convert your 16&#215;2 display to I<sup>2</sup>C, you&#8217;ve probably googled and found many resources on the Internet. But if you happen to have a MGD1602B series LCD module, this may be your lucky day, because I&#8217;ve already designed a board in Eagle Cad for this purpose. Keep reading and download converter schematic and board. All for free, as (almost) always. Bonus: Eagle Cad library (.lbr) for this LCD module.

<!--more-->

First of all, please note that I didn&#8217;t invent anything new except for the board itself that was designed for this specific LCD module.

The circuit was stolen from <a href="http://hmario.home.xs4all.nl/arduino/LiquidCrystal_I2C/" title="Mario_H's page" target="_blank">Mario_H&#8217;s page</a>. In this page you will also find an Arduino library to drive the LCD via I<sup>2</sup>C.

## MGD1602B Series LCD module

This generic 16&#215;2 module has the following pinout. Please always verify if your LCD match this one, there are a lot of line displays out there from different manufacturers and specs totally different.

The + and &#8211; are pins 15 and 16 respectively.

<figure style="text-align: center;">
	<a href="{{ site.url }}/images/lcd_i2c_01.png" target="_blank"><img src="{{ site.url }}/images/lcd_i2c_01.png" alt="MGD1602B Pinout" title="MGD1602B Pinout"/></a>
	<figcaption>MGD1602B Pinout</figcaption>
</figure>

In this peculiar LCD pins 15 and 16 are not following the linear order of the rest of the pins from 1 to 14!

<figure style="text-align: center;">
	<a href="{{ site.url }}/images/lcd_i2c_02.png" target="_blank"><img src="{{ site.url }}/images/lcd_i2c_02.png" alt="MGD1602B Pin distribution" title="MGD1602B Pin distribution"/></a>
	<figcaption>MGD1602B Pin distribution</figcaption>
</figure>

And here is the [MGD1602B-NS(W)-BBS BACK AZUL Datasheet]({{ site.url }}/files/datasheets/MGD1602B-NSW-BBS-BACK-AZUL.pdf), just in case.

## Converter board

<figure class="half">
	<a href="{{ site.url }}/images/lcd_i2c_schematic.png" target="_blank"><img src="{{ site.url }}/images/lcd_i2c_schematic.png" alt="Converter Schematic" title="Converter Schematic"/></a>
	<a href="{{ site.url }}/images/lcd_i2c_board.png" target="_blank"><img src="{{ site.url }}/images/lcd_i2c_board.png" alt="Converter Board" title="Converter Board"/></a>
</figure>

## Arduino Duemilanove connection

SDA (inner board pin, not being 5v) -> A4

SCL (outer board pin, not being ground) -> A5

## Testing code (using LiquidCrystal_I2C library)

Note that the address is 0x38 (all 3 address pins grounded).

{% highlight cpp %}
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x38, 16, 2);  // set the LCD address to 0x20 for a 16 chars and 2 line display

void setup()
{
  lcd.init();                      // initialize the lcd
  // Print a message to the LCD.
  lcd.backlight();
  lcd.print("Hello, world!");
}

void loop()
{
}
{% endhighlight %}

<a title="Download converter schematic, board and LCD eagle library" markdown="0" href="https://github.com/fvicente/lcd-i2c/archive/master.zip" class="btn">Download converter schematic, board and LCD eagle library</a>

[lcd-i2c on GitHub](https://github.com/fvicente/lcd-i2c "lcd-i2c on GitHub"){:target="_blank"}

Good luck!
