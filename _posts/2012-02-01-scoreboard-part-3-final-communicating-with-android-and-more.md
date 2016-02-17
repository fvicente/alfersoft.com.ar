---
id: 414
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
[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/IMG_2715-300x200.jpg" alt="Scoreboard - Final" title="Scoreboard - Final" width="300" height="200" class="alignleft size-medium wp-image-416" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/IMG_2715-300x200.jpg 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/IMG_2715-1024x682.jpg 1024w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/IMG_2715.jpg)The Scoreboard project is now finished and working!
  
The idea of this project is pretty simple: control a ping-pong electronic scoreboard from an Android bluetooth-enabled device.
  
To do this, I used an ATtiny45 which main function is to display the current scores in a VGA monitor while reading from a bluetooth module UART interface waiting for &#8220;commands&#8221; that will tell it what to display. The Android device sends the commands via bluetooth, running an application specially designed for this project.
  
As usual, the whole project is open source, including schematics, AVR firmware and the Android application.
  
<!--more-->


  
**Schematic and Board**
  
Schematic and board were made with Eagle CAD software.
  
Note: In this schematic I&#8217;ve used an ATtiny13, because I could not find the ATtiny45 in my Eagle library. As it was said before, in the project I ended using an ATtiny45. I guess the memory of ATtiny13 is enough to run the scoreboard firmware but can&#8217;t say for sure.
  
<figure id="attachment_419" style="width: 300px" class="wp-caption aligncenter">[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/scoreboard_schematic-300x300.png" alt="Scoreboard Schematic" title="Scoreboard Schematic" width="300" height="300" class="size-medium wp-image-419" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/scoreboard_schematic-150x150.png 150w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/scoreboard_schematic-300x300.png 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/scoreboard_schematic-1024x1024.png 1024w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/scoreboard_schematic.png 1100w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/scoreboard_schematic.png)<figcaption class="wp-caption-text">Scoreboard Schematic</figcaption></figure>
  
<figure id="attachment_420" style="width: 300px" class="wp-caption aligncenter">[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/scoreboard_pcb-300x191.png" alt="Scoreboard PCB" title="Scoreboard PCB" width="300" height="191" class="size-medium wp-image-420" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/scoreboard_pcb-300x191.png 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/scoreboard_pcb.png 732w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/scoreboard_pcb.png)<figcaption class="wp-caption-text">Scoreboard PCB</figcaption></figure>
  
**List of materials**

<table style="background-color: white; border: 1px solid #C3C3C3; border-collapse: collapse; width: 100%;">
  <tr>
    <th>
      Qty
    </th>
    
    <th>
      Component
    </th>
    
    <th>
      Sch. Code
    </th>
    
    <th>
      Datasheet
    </th>
    
    <th>
      Price (avg. US$)
    </th>
  </tr>
  
  <tr>
    <td>
      1
    </td>
    
    <td>
      3.3v Regulator
    </td>
    
    <td>
      IC1
    </td>
    
    <td>
      <a href='http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/02/tlv1117-33.pdf'>tlv1117-33</a>
    </td>
    
    <td>
      0.79
    </td>
  </tr>
  
  <tr>
    <td>
      1
    </td>
    
    <td>
      Atmel ATtiny45 microcontroller
    </td>
    
    <td>
      IC2
    </td>
    
    <td>
    </td>
    
    <td>
      2.31
    </td>
  </tr>
  
  <tr>
    <td>
      1
    </td>
    
    <td>
      Bluetooth module
    </td>
    
    <td>
      P$1
    </td>
    
    <td>
      <a href='http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/02/bluetoothddatasheet.pdf'>bluetooth</a>
    </td>
    
    <td>
      6.60 (on <a href="http://www.dealextreme.com/p/wireless-bluetooth-rs232-ttl-transceiver-module-80711" title="Bluetooth module on DealExtreme">DealExtreme</a>)
    </td>
  </tr>
  
  <tr>
    <td>
      2
    </td>
    
    <td>
      104 Ceramic capacitor
    </td>
    
    <td>
      C1, C4
    </td>
    
    <td>
    </td>
    
    <td>
      0.05 (each)
    </td>
  </tr>
  
  <tr>
    <td>
      2
    </td>
    
    <td>
      10uf Electrolytic capacitor
    </td>
    
    <td>
      C2, C3
    </td>
    
    <td>
    </td>
    
    <td>
      0.05 (each)
    </td>
  </tr>
  
  <tr>
    <td>
      2
    </td>
    
    <td>
      22pF Ceramic capacitor
    </td>
    
    <td>
      C5, C6
    </td>
    
    <td>
    </td>
    
    <td>
      0.05 (each)
    </td>
  </tr>
  
  <tr>
    <td>
      1
    </td>
    
    <td>
      20Mhz Crystal oscillator
    </td>
    
    <td>
      Q1
    </td>
    
    <td>
    </td>
    
    <td>
      0.65
    </td>
  </tr>
  
  <tr>
    <td>
      1
    </td>
    
    <td>
      Led
    </td>
    
    <td>
      LED1
    </td>
    
    <td>
    </td>
    
    <td>
      0.15
    </td>
  </tr>
  
  <tr>
    <td>
      1
    </td>
    
    <td>
      470R Resistor
    </td>
    
    <td>
      R1
    </td>
    
    <td>
    </td>
    
    <td>
      0.01
    </td>
  </tr>
  
  <tr>
    <td>
      1
    </td>
    
    <td>
      VGA DB-15 connector
    </td>
    
    <td>
    </td>
    
    <td>
    </td>
    
    <td>
      2.28
    </td>
  </tr>
  
  <tr>
    <td>
      <del datetime="2012-01-31T21:31:08+00:00">1</del>
    </td>
    
    <td>
      <del datetime="2012-01-31T21:31:08+00:00">10K Resistor</del>
    </td>
    
    <td>
      <del datetime="2012-01-31T21:31:08+00:00">R2</del>
    </td>
    
    <td>
    </td>
    
    <td>
    </td>
  </tr>
  
  <tr>
    <td>
      1
    </td>
    
    <td>
      Plain PCB / Printout / Iron chloride
    </td>
    
    <td>
    </td>
    
    <td>
    </td>
    
    <td>
      3.00
    </td>
  </tr>
</table>

**Circuit**
  
Circuit is pretty straight as you can see. An external C/C power supply is needed to power the circuit. There is a voltage regulator so the supply in this case can be up to 15V (I didn&#8217;t measure consumption yet). The regulator has its respective capacitors in the input and output, used as filters. The bluetooth module and the microcontroller are connected through a single wire between module&#8217;s UART TXD and the PB5 pin of the ATtiny fused as an input (read more on [Part 2](http://www.alfersoft.com.ar/blog/2012/01/23/scoreboard-part-2-reading-uart-from-the-bluetooth-module/ "Scoreboard (Part 2: Reading UART from the Bluetooth Module)") of this series).
  
A blue led with a resistor is attached to PIN24 of the bluetooth module, it will blink while the module is waiting for a connection and keep on when a connection is established, that&#8217;s using the default module firmware linvor1.5 that comes pre-programmed from DealExtreme, if you buy a different module or use a different firmware, you will need to review all the pin connections. According to the bluetooth module datasheet (at least the one that is supposed to be the correct one) you should put a 10K resistor from the reset pin to the ground, BUT actually I had to remove it to get the module working, otherwise it won&#8217;t even turn on. So **DO NOT PUT R2.**

The meaning of the pads in the PCB is the following:

  * PAD1: Input supply V+
  * PAD2: V- (ground)
  * PAD3: HSYNC to the VGA connector DB-15 pin 13
  * PAD4: VSYNC to the VGA connector DB-15 pin 14
  * PAD5: RGB to the VGA connector DB-15 pin 1 to 3
  * PAD6: Ground to the VGA connector DB-15 pin 5 to 10

<figure id="attachment_426" style="width: 300px" class="wp-caption aligncenter">[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/IMG_2721-300x200.jpg" alt="Scoreboard on the breadboard" title="Scoreboard on the breadboard" width="300" height="200" class="size-medium wp-image-426" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/IMG_2721-300x200.jpg 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/IMG_2721-1024x682.jpg 1024w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/IMG_2721.jpg)<figcaption class="wp-caption-text">Scoreboard on the breadboard</figcaption></figure>
  
<figure id="attachment_427" style="width: 300px" class="wp-caption aligncenter">[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/IMG_2725-300x200.jpg" alt="Scoreboard on PCB (top view)" title="Scoreboard on PCB (top view)" width="300" height="200" class="size-medium wp-image-427" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/IMG_2725-300x200.jpg 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/IMG_2725-1024x682.jpg 1024w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/IMG_2725.jpg)<figcaption class="wp-caption-text">Scoreboard on PCB (top view)</figcaption></figure>
  
<figure id="attachment_432" style="width: 300px" class="wp-caption aligncenter">[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/02/IMG_2728-300x200.jpg" alt="Scoreboard on PCB (bottom view)" title="Scoreboard on PCB (bottom view)" width="300" height="200" class="size-medium wp-image-432" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/02/IMG_2728-300x200.jpg 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/02/IMG_2728-1024x682.jpg 1024w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/02/IMG_2728.jpg)<figcaption class="wp-caption-text">Scoreboard on PCB (bottom view)</figcaption></figure>
  
**Android Application**
  
Thanks to CarlosBar!!! that made the program for me. He used the Android chat example as a base, changed the UUID to be able to connect using the SPP serial bluetooth (after all is what the DX bluetooth module is, an SPP serial port). Then he designed a nice GUI to control the scores, with an internal stack to support an Undo functionality in case you score on the wrong team.
  
Also, you can control the scores with the Volume Up/Down keys for convenience.
  
We have created a public repository on <a href="https://bitbucket.org/fvicente/scoreboard" title="BitBucket repository for Scoreboard Android Application" target="_blank">https://bitbucket.org/fvicente/scoreboard</a> where you can checkout the source code.
  
Also it is available on the Android Market (search for &#8220;Scoreboard SPP&#8221;), I recommend to check out other CarlosBar project like the excellent <a href="http://code.google.com/p/ttsaid/" title="TTSAid" target="_blank">TTSAid</a>.
  
<figure id="attachment_424" style="width: 300px" class="wp-caption aligncenter">[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/IMG_2716-300x200.jpg" alt="Scoreboard Android Application" title="Scoreboard Android Application" width="300" height="200" class="size-medium wp-image-424" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/IMG_2716-300x200.jpg 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/IMG_2716-1024x682.jpg 1024w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/01/IMG_2716.jpg)<figcaption class="wp-caption-text">Scoreboard Android Application</figcaption></figure>
  
**Related Posts**
  
You can find more details on the ATtiny firmware, the UART communication and the VGA output, on previous posts:
  
[Scoreboard (Part 1: VGA signal from an ATtiny45)](http://www.alfersoft.com.ar/blog/2011/08/30/scoreboard-part-1-vga-signal-from-an-attiny45/ "Scoreboard (Part 1: VGA signal from an ATtiny45)")
  
[Scoreboard (Part 2: Reading UART from the Bluetooth Module)](http://www.alfersoft.com.ar/blog/2012/01/23/scoreboard-part-2-reading-uart-from-the-bluetooth-module/ "Scoreboard (Part 2: Reading UART from the Bluetooth Module)")
  
**Download**
  
[Scoreboard Final Source Code](http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/02/scoreboard_final.zip)
  
Enjoy!
