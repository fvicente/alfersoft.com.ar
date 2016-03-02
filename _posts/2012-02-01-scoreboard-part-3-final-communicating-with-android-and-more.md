---
uid: 414
title: 'Scoreboard (Part 3: Final &#8211; Communicating with Android and more)'
date: 2012-02-01T19:23:12+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=414
permalink: /2012/02/01/scoreboard-part-3-final-communicating-with-android-and-more/
categories:
  - Android
  - Electronic Projects
tags:
  - android
  - assembler
  - attiny
  - attiny45
  - avr
  - bluetooth
  - scoreboard
  - VGA
comments: true
---
[<img src="{{ site.url }}/images/scoreboard/scoreboard_02.jpg" alt="Scoreboard - Final" title="Scoreboard - Final"/>]({{ site.url }}/images/scoreboard/scoreboard_02.jpg)

The Scoreboard project is now finished and working!

The idea of this project is pretty simple: control a ping-pong electronic scoreboard from an Android bluetooth-enabled device.

To do this, I used an ATtiny45 which main function is to display the current scores in a VGA monitor while reading from a bluetooth module UART interface waiting for &#8220;commands&#8221; that will tell it what to display. The Android device sends the commands via bluetooth, running an application specially designed for this project.

As usual, the whole project is open source, including schematics, AVR firmware and the Android application.

<!--more-->

## Schematic and Board

Schematic and board were made with Eagle CAD software.

Note: In this schematic I&#8217;ve used an ATtiny13, because I could not find the ATtiny45 in my Eagle library. As it was said before, in the project I ended using an ATtiny45. I guess the memory of ATtiny13 is enough to run the scoreboard firmware but can&#8217;t say for sure.

<figure class="half">
	<a title="Scoreboard Schematic" href="{{ site.url }}/images/scoreboard/scoreboard_schematic.png" target="_blank"><img src="{{ site.url }}/images/scoreboard/scoreboard_schematic.png" alt="Scoreboard Schematic" /></a>
	<a title="Scoreboard PCB" href="{{ site.url }}/images/scoreboard/scoreboard_pcb.png" target="_blank"><img src="{{ site.url }}/images/scoreboard/scoreboard_pcb.png" alt="Scoreboard PCB" /></a>
</figure>

## List of materials

Qty|Component|Sch. Code|Datasheet|Price (avg. US$)|
--:|---------|---------|---------|---------------:|
1  |3.3v Regulator|IC1|[tlv1117-33]({{ site.url }}/files/datasheets/tlv1117-33.pdf)|0.79|
1  |Atmel ATtiny45 microcontroller|IC2||2.31|
1  |Bluetooth module|P$1|[bluetooth]({{ site.url }}/files/datasheets/bluetoothddatasheet.pdf)|6.60 (on <a href="http://www.dealextreme.com/p/wireless-bluetooth-rs232-ttl-transceiver-module-80711" title="Bluetooth module on DealExtreme">DealExtreme</a>)|
2  |104 Ceramic capacitor|C1, C4||0.05 (each)|
2  |10uf Electrolytic capacitor|C2, C3||0.05 (each)|
2  |22pF Ceramic capacitor|C5, C6||0.05 (each)|
1  |20Mhz Crystal oscillator|Q1||0.65|
1  |Led|LED1||0.15|
1  |470R Resistor|R1||0.01|
1  |VGA DB-15 connector|||2.28|
<s>1</s>|<s>10K Resistor</s>|<s>R2</s>|||
1  |Plain PCB / Printout / Iron chloride|||3.00|

## Circuit

Circuit is pretty straight as you can see. An external C/C power supply is needed to power the circuit. There is a voltage regulator so the supply in this case can be up to 15V (I didn&#8217;t measure consumption yet). The regulator has its respective capacitors in the input and output, used as filters. The bluetooth module and the microcontroller are connected through a single wire between module&#8217;s UART TXD and the PB5 pin of the ATtiny fused as an input (read more on [Part 2]({{ site.url }}/2012/01/23/scoreboard-part-2-reading-uart-from-the-bluetooth-module/ "Scoreboard (Part 2: Reading UART from the Bluetooth Module)") of this series).

A blue led with a resistor is attached to PIN24 of the bluetooth module, it will blink while the module is waiting for a connection and keep on when a connection is established, that&#8217;s using the default module firmware linvor1.5 that comes pre-programmed from DealExtreme, if you buy a different module or use a different firmware, you will need to review all the pin connections. According to the bluetooth module datasheet (at least the one that is supposed to be the correct one) you should put a 10K resistor from the reset pin to the ground, BUT actually I had to remove it to get the module working, otherwise it won&#8217;t even turn on. So **DO NOT PUT R2.**

The meaning of the pads in the PCB is the following:

* PAD1: Input supply V+
* PAD2: V- (ground)
* PAD3: HSYNC to the VGA connector DB-15 pin 13
* PAD4: VSYNC to the VGA connector DB-15 pin 14
* PAD5: RGB to the VGA connector DB-15 pin 1 to 3
* PAD6: Ground to the VGA connector DB-15 pin 5 to 10

<figure>
	<a title="Scoreboard on the breadboard" href="{{ site.url }}/images/scoreboard/scoreboard_proto_02.jpg" target="_blank"><img src="{{ site.url }}/images/scoreboard/scoreboard_proto_02.jpg" alt="Scoreboard on the breadboard"/></a>
	<figcaption>Scoreboard on the breadboard</figcaption>
</figure>

<figure>
	<a title="Scoreboard on PCB (top view)" href="{{ site.url }}/images/scoreboard/scoreboard_03.jpg" target="_blank"><img src="{{ site.url }}/images/scoreboard/scoreboard_03.jpg" alt="Scoreboard on PCB (top view)"/></a>
	<figcaption>Scoreboard on PCB (top view)</figcaption>
</figure>

<figure>
	<a title="Scoreboard on PCB (bottom view)" href="{{ site.url }}/images/scoreboard/scoreboard_04.jpg" target="_blank"><img src="{{ site.url }}/images/scoreboard/scoreboard_04.jpg" alt="Scoreboard on PCB (bottom view)"/></a>
	<figcaption>Scoreboard on PCB (bottom view)</figcaption>
</figure>


## Android Application

Thanks to CarlosBar!!! that made the program for me. He used the Android chat example as a base, changed the UUID to be able to connect using the SPP serial bluetooth (after all is what the DX bluetooth module is, an SPP serial port). Then he designed a nice GUI to control the scores, with an internal stack to support an Undo functionality in case you score on the wrong team.

Also, you can control the scores with the Volume Up/Down keys for convenience.

We have created a public repository on <a href="https://bitbucket.org/fvicente/scoreboard" title="BitBucket repository for Scoreboard Android Application" target="_blank">https://bitbucket.org/fvicente/scoreboard</a> where you can checkout the source code.

Also it is available on the Android Market (search for &#8220;Scoreboard SPP&#8221;), I recommend to check out other CarlosBar project like the excellent <a href="http://code.google.com/p/ttsaid/" title="TTSAid" target="_blank">TTSAid</a>.

<figure>
	<a title="Scoreboard Android Application" href="{{ site.url }}/images/scoreboard/scoreboard_app.jpg" target="_blank"><img src="{{ site.url }}/images/scoreboard/scoreboard_app.jpg" alt="Scoreboard Android Application"/></a>
	<figcaption>Scoreboard Android Application</figcaption>
</figure>

## Related Posts

You can find more details on the ATtiny firmware, the UART communication and the VGA output, on previous posts:

[Scoreboard (Part 1: VGA signal from an ATtiny45)]({{ site.url }}/2011/08/30/scoreboard-part-1-vga-signal-from-an-attiny45/ "Scoreboard (Part 1: VGA signal from an ATtiny45)")

[Scoreboard (Part 2: Reading UART from the Bluetooth Module)]({{ site.url }}/2012/01/23/scoreboard-part-2-reading-uart-from-the-bluetooth-module/ "Scoreboard (Part 2: Reading UART from the Bluetooth Module)")

## Download

<a title="Download Final Scoreboard" markdown="0" href="https://github.com/fvicente/scoreboard/archive/master.zip" class="btn">Download Final Scoreboard Source</a>

[Scoreboard on GitHub](https://github.com/fvicente/scoreboard "Scoreboard on GitHub"){:target="_blank"}

Enjoy!
