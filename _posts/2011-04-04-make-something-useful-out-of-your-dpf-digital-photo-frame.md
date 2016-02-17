---
id: 221
title: Make something useful out of your DPF (Digital Photo Frame)
date: 2011-04-04T13:52:05+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=221
permalink: /2011/04/04/make-something-useful-out-of-your-dpf-digital-photo-frame/
categories:
  - Electronic Projects
  - Software Development
tags:
  - circuit
  - digital
  - DIY
  - dpf
  - frame
  - hack
  - parallel
  - photo
  - serial
  - switch
  - USB
comments: true
---
[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110401_171720-150x150.jpg" alt="" title="IMG_20110401_171720" width="100" height="100" class="alignleft size-thumbnail wp-image-232" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110401_171720.jpg)We had this great idea of using an old digital photo frame from [smartparts](http://smartpartsproducts.com/content/index.html) to display some useful reports at work.
  
In this case, useful will mean that the reports (pictures in the frame) needs to be automatically updated, with no human intervention.
  
Take a look to this post, you may find something useful for your own DIY project!
  
<!--more-->


  
There are probably several ways to update the pictures dynamically, we have chosen the easiest one: connect it to an old Itona computer (we&#8217;ve plenty of these crappy machines 300Mhz processor, 512Mb of hard disk) running Ubuntu Linux and then make a script that will periodically (cron) gather the info we want to show from the Internet convert it to JPG images, and update the photo frame.

[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110404_105718-225x300.jpg" alt="" title="IMG_20110404_105718" width="225" height="300" class="alignnone size-medium wp-image-225" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110404_105718.jpg)
  
Too easy? Ok, a real ninja hacker solution would had been taking apart the photo frame, put a Linux on the nice ARC processor through the JTAG interface and then plug some Wi-Fi dongle and do Everything directly on the DPF itself. But that&#8217;s just dreaming, we don&#8217;t have experience nor time for that.

Now you may ask, why didn&#8217;t you plug a monitor on the machine and just display whatever you want directly on the monitor? and the first reason is, we don&#8217;t have spare LCD monitors for this purpose (in fact we have many CRT monitors that nobody uses, but that requires just too much space). The second reason is that we wanted to do something more exiting, like hacker things :D.

So we setup everything: we made the scripts to generate the images, installed Ubuntu Linux 10.4 on the Itona and actually we borrowed another 512mb (flash) hard disk form another Itona and put it in the secondary IDE to get a RAID configuration with 1GB of space, just enough for Ubuntu and our script. Then realized of a small problem, as soon as you plug the DPF on the USB port, it enters in &#8220;transfer mode&#8221; and stops displaying the images. No matter if you eject, unmount, safe remove, shut down the computer, etc. the photo frame wont show the pictures until you physically unplug the USB cable (cut off the +5V). Here is were our ingenious circuit enters in action. A **USB interrupter controlled by the parallel port** (I guess serial RS-232 would work fine also using one of the signal pins).
  
After googling, I&#8217;ve found [a circuit](http://forum.allaboutcircuits.com/showthread.php?t=38544&page=2) that seemed usable for this purpose. Just a couple transistors and a few resistors is everything we need.
  
[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110331_185113-300x225.jpg" alt="" title="IMG_20110331_185113" width="300" height="225" class="alignnone size-medium wp-image-226" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110331_185113.jpg)
  
Crop the board and put it in a case&#8230;
  
[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110331_191457-300x225.jpg" alt="" title="IMG_20110331_191457" width="300" height="225" class="alignnone size-medium wp-image-227" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110331_191457.jpg)
  
[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110331_191507-300x225.jpg" alt="" title="IMG_20110331_191507" width="300" height="225" class="alignnone size-medium wp-image-228" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110331_191507.jpg)
  
[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110331_192915-300x225.jpg" alt="" title="IMG_20110331_192915" width="300" height="225" class="alignnone size-medium wp-image-229" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110331_192915.jpg)
  
Finally, the whole thing mounted:
  
The Itona hidden behind the desk&#8230;
  
[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110404_103336-225x300.jpg" alt="" title="IMG_20110404_103336" width="225" height="300" class="alignnone size-medium wp-image-230" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110404_103336.jpg)
  
And the DPF showing the reports!! 😀
  
[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110401_171707-300x225.jpg" alt="" title="IMG_20110401_171707" width="300" height="225" class="alignnone size-medium wp-image-231" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110401_171707.jpg)
  
[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110401_171720-300x225.jpg" alt="" title="IMG_20110401_171720" width="300" height="225" class="alignnone size-medium wp-image-232" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/IMG_20110401_171720.jpg)

For the circuit you will need:

  * 1k resistor (2)
  * 10k resistor (2)
  * PNP transistor
  * NPN transistor
  * USB connectors, 1 to plug into the computer, the other whatever is better for your DPF
  * DB-25 male connector (if you decided to do it via parallel too)

**IMPORTANT: following image was borrowed from http://forum.allaboutcircuits.com/showthread.php?t=38544&page=2**
  
[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/5V-Switch-300x216.png" alt="" title="5V-Switch" width="300" height="216" class="alignnone size-medium wp-image-238" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/5V-Switch-300x216.png 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/5V-Switch.png 695w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2011/04/5V-Switch.png)
  
I&#8217;ve connected the control-in to the PIN #2 of the DB-25, and the PIN #20 to the ground.
  
To turn the parallel port signal on/off I&#8217;ve made the following program in C (para.c):

<pre>#include &lt;sys/ioctl.h>
#include &lt;sys/types.h>
#include &lt;unistd.h>
#include &lt;fcntl.h>
#include &lt;stdio.h>
#include &lt;string.h>
#include &lt;stdlib.h>

#include &lt;linux/ppdev.h>

static int pfd;

int main(int argc, char* argv[])
{
	unsigned char data=0;

	if (argc != 3) {
		printf("Usage:\n\tpara [port] [val]\n\nport: parallel port e.g. /dev/parport0\nval:  1 to activate all data bits in parallel port, or\n      0 to disable all data bits in parallel port\n\n");
		return 1;
	}
	if (argv[2][0] == '1') {
		data=0xff;
	}
	pfd = open(argv[1], O_RDWR);
	if (pfd &lt; 0) {
		perror("Failed to open port");
		exit(0);
	}
	if ((ioctl(pfd, PPEXCL) &lt; 0) || (ioctl(pfd, PPCLAIM) &lt; 0)) {
		perror("Failed to lock port");
		close(pfd);
		exit(0);
	}
	printf("Setting %s data bits to: %.2X\n", argv[1], data);
	ioctl(pfd, PPWDATA, &#038;data);
	close(pfd);
	return 0;
}
</pre>
