---
uid: 394
title: 'Scoreboard (Part 2: Reading UART from the Bluetooth Module)'
date: 2012-01-23T17:41:03+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=394
categories:
  - Electronic Projects
tags:
  - attiny
  - attiny45
  - bluetooth
  - module
  - scoreboard
  - uart
  - VGA
comments: true
excerpt: Make ATtiny read UART from cheap Wireless Bluetooth V2.0 RS232 TTL Transceiver Module
---
<img src="{{ site.baseurl }}/images/bt_small.jpg" alt="Bluetooth Module" title="Bluetooth Module"/>

In this post, you will find how the <a href="http://www.dealextreme.com/p/wireless-bluetooth-rs232-ttl-transceiver-module-80711" title="Cheap Bluetooth Module" target="_blank">Bluetooth module</a> interacts with the ATtiny45 in the <a href="{{ site.baseurl }}/2011/08/30/scoreboard-part-1-vga-signal-from-an-attiny45/" title="Scoreboard (Part 1: VGA signal from an ATtiny45)" target="_blank">VGA Scoreboard</a> project.

The Bluetooth module will wait for a connection from a device (e.g. an Android phone) and will act as an SPP (Serial Port Profile) re-passing everything received from the device to the UART interface. In our case the ATtiny will read the data but won&#8217;t &#8220;speak back&#8221; to the module, so it&#8217;s really a one way communication from that point of view.

<!--more-->

Electrically speaking, we just need one wire to connect the TXD pin of the bluetooth module (PIN 1) to the PB5 (PIN 1) of the ATtiny, which will be configured as an input.

Now the tricky part is on the microcontroller&#8217;s firmware side, since we need to read the input without disrupting the VGA timing. Our ATtiny is running at 20Mhz which gives 0.05μs per cycle, in the program the main loop is based on the VGA horizontal timings so we can read the input every 635 cycles (31.75μs). On the other side the BT module is configured at 2400 bauds which means that every bit will last for 416.66μs. So, doing a quick math, 416.66 / 31.75 = 13.12, meaning each bit will be read about 13 times. We will assume the read #7 as the valid one and we wont do any kind of checks.

The UART will be sending a &#8220;high&#8221; signal when is idle, as soon as we get a &#8220;low&#8221; we know that the &#8220;start bit&#8221; is arriving, so we set a global flag and start reading the 8 bits and putting them into a register (r19). You can read more about UART on <a href="http://en.wikipedia.org/wiki/Universal_asynchronous_receiver/transmitter" title="Wikipedia UART" target="_blank">this Wikipedia article</a>. We use another register (r25) as a counter, to know when to read and stop reading.

This is how the code looks like:

{% highlight nasm %}
	;
	; UART read input
	; This code is executed every 635 cycles in the Scoreboard code
	;
	in r18, _SFR_IO_ADDR(PINB)		; 1 clock

	cpi r25, 0
	breq waitlow
	inc r25
	mov r16, r25
	andi r16, 0x0F
	cpi r16, 7
	brne chkfornext5togo
	lsr r19
	sbrc r18, INPUT
	ori r19, 0x80
	nop
	rjmp chkfornext
chkfornext5togo:
	nop
	nop
	nop
	nop
	nop
chkfornext:
	cpi r16, 14
	brne chkend3togo
	subi r25, 0xEF	; add 17
	andi r25, 0xF1
	rjmp chkend
chkend3togo:
	nop
	nop
	nop
chkend:
	nop
	rjmp hsyncoff

waitlow:
	; waiting for low
	sbrs r18, INPUT
	inc r25
	nop
	delay3x 4
	nop
	nop
	rjmp hsyncoff

hsyncoff:
	...... HSYNC code ......
        ...... if r25 == 0x91 then the UART was read and stored in r19......
	...... process and reset both registers ....
{% endhighlight %}

Since we read one byte at the time, it seems pretty convenient for our project to define a set of &#8220;commands&#8221; (of one byte long), to set the characters that must be displayed (four commands to set each one of characters), and additionally another command to display little square on the corners of the screen which will indicate who is currently serving.

Here is our list of commands:

{% highlight bash %}
0 0 0 1 D D D D		(character 1) [0-D] -> 0 1 2 3 4 5 6 7 8 9 (SPACE) B O G
0 0 1 0 D D D D		(character 2)
0 0 1 1 D D D D		(character 3)
0 1 0 0 D D D D		(character 4)
0 1 0 1 0 0 0 1		(serve A top)
0 1 0 1 0 0 1 0		(serve B bottom)
0 1 0 1 0 1 0 0		(serve A top)
0 1 0 1 1 0 0 0		(serve b bottom)
{% endhighlight %}

All we need now is a device to connect to the bluetooth module and send the commands.

Don&#8217;t miss the final part of this project, that I will publish soon including source codes and a controller developed for Android platforms.

## Configuring the Bluetooth Module with Arduino

The bluetooth module comes with a firmware called &#8220;linvor1.5&#8221; by default (at least the one on DX). To program it, you need to send a series of AT commands via the TX/RX pins, Arduino seems to be ideal for this task. Connect two cables between the TX/RX of the Arduino and the RX/TX of the Bluetooth module (they need to be crossed, RX with TX and TX with RX). Then put the following code on the Arduino:

{% highlight cpp %}
int incomingByte = 0;	// for incoming serial data

void setup() {
  Serial.begin(9600);
  Serial.print("AT");
}

void loop() {
	// send data only when you receive data:
	if (Serial.available() > 0) {
		// read the incoming byte:
		incomingByte = Serial.read();
		// say what you got:
		Serial.write(incomingByte);
	}
}
{% endhighlight %}

Upload it to the Arduino and click on the Monitor, you will get the following response:

[<img src="{{ site.baseurl }}/images/bt_serial.png" alt="" title="ATOK"/>]({{ site.baseurl }}/images/bt_serial.png)

Now we are going to change the default name to &#8220;scoreboard&#8221;, to do so, replace the `Serial.print("AT")` with `Serial.print("AT+NAMEscoreboard")` and repeat the procedure.

Finally, we need to set the baud rate to 2400, using the following command: `Serial.print("AT+BAUD2")`

Comments and questions are welcome!

## Related Posts

[Scoreboard (Part 1: VGA signal from an ATtiny45)]({{ site.baseurl }}/2011/08/30/scoreboard-part-1-vga-signal-from-an-attiny45/ "Scoreboard (Part 1: VGA signal from an ATtiny45)")
