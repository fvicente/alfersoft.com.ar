---
uid: 380
title: Protoboard adapter for cheap Bluetooth module
date: 2012-01-22T15:52:46+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=380
permalink: /2012/01/22/protoboard-adapter-for-cheap-bluetooth-module/
categories:
  - Electronic Projects
tags:
  - adapter
  - bluetooth
  - breadboard
  - deal extreme
  - module
  - protoboard
comments: true
excerpt: Protoboard adapter for Wireless Bluetooth V2.0 RS232 TTL Transceiver Module
---
[<img src="{{ site.baseurl }}/images/bt_adaptor_01.jpg" alt="Protoboard adapter for Bluetooth module" title="Protoboard adapter for Bluetooth module"/>]({{ site.baseurl }}/images/bt_adaptor_01.jpg)

I finally received some stuff from DealExtreme.com including a <a href="http://www.dealextreme.com/p/wireless-bluetooth-rs232-ttl-transceiver-module-80711" title="Bluetooth Module" target="_blank">cheap Bluetooth Module</a> which I will use to finish my remote scoreboard project.

But first, I made this small PCB to adapt the module to a breadboard for testing and programming. I found on the Internet <a href="http://elasticsheep.com/2011/09/bluetooth-module-breakout-boards-are-back-in-stock/" title="Bluetooth module adapter" target="_blank">someone that already did this</a>, but I wanted to use the SPI programming interface too, then I needed to lay pins 14 to 21 let&#8217;s say vertically, to be able to plug it on the protoboard.

<!--more-->

The module is really small 2.7 cm x 1.3 cm x 0.1 cm, take a look to this picture in relation to a R$1 coin (Brazilian Real).

<figure style="text-align: center;">
	<a title="Stuff from dealextreme.com" href="{{ site.baseurl }}/images/bt_adaptor_02.jpg" target="_blank"><img src="{{ site.baseurl }}/images/bt_adaptor_02.jpg" alt="Stuff from dealextreme.com" /></a>
	<figcaption>Stuff from dealextreme.com</figcaption>
</figure>

Tip: Soldering the BT module to the adapter is easy, if you use <a href="http://en.wikipedia.org/wiki/Soldering#Flux" title="Soldering Flux (Wikipedia)" target="_blank">flux</a>.

<figure style="text-align: center;">
	<a title="Soldering BT module to the adapter" href="{{ site.baseurl }}/images/bt_adaptor_03.jpg" target="_blank"><img src="{{ site.baseurl }}/images/bt_adaptor_03.jpg" alt="Soldering BT module to the adapter" /></a>
	<figcaption>Soldering BT module to the adapter</figcaption>
</figure>

The PCB design was made in Eagle CAD, I made a library that represents the Bluetooth adapter.

I have two versions of the adapter:

* Version 1 (the one in the pictures) with a higher separation between the pins.
* Version 2 improved to use less space for smaller breadboards (I didn&#8217;t produce this one).

<figure style="text-align: center;">
	<a title="BT Module Adapter PDF" href="{{ site.baseurl }}/images/bt_adaptor_04.png" target="_blank"><img src="{{ site.baseurl }}/images/bt_adaptor_04.png" alt="BT Module Adapter PDF" /></a>
	<figcaption>BT Module Adapter PDF</figcaption>
</figure>

With this adapter, I was able to test and update the firmware of the bluetooth module. By the way, if you are looking for information about how to upgrade the firmware, check out this excellent <a href="http://byron76.blogspot.com/" title="Byron76 blog" target="_blank">blog</a> from Byron76.

To test, send commands and configure the module, you need to communicate with it via 3.3v serial interface (I used my Arduino Duemilanove), but if you just want to test the Bluetooth communication, you can simply interconnect TXD and RXD pins (pin # 1 and 2) and any character you send will be echoed. This fantastic page Elasticsheep.com gives <a href="http://elasticsheep.com/2011/05/serial-bluetooth-module-masterslave-connection/" title="Elasticsheep.com testing Bluetooth module" target="_blank">detailed instructions</a> on how to test the module.

<a title="Download schematics, Eagle CAD library and PDF" markdown="0" href="{{ site.baseurl }}/files/btadap.zip" class="btn">Download schematics, Eagle CAD library and PDF</a>

Feel free modify and use it however you want.

Good luck!
