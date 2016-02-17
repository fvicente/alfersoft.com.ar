---
id: 543
title: 'FAIL: How to NOT try to hack a cheap GPRS module to change its IMEI'
date: 2012-07-26T11:30:51+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=543
permalink: /2012/07/26/fail-how-to-not-try-to-hack-a-cheap-gprs-module-to-change-its-imei/
categories:
  - Electronic Projects
tags:
  - deal extreme
  - GPRS
  - hack
  - IMEI
  - modem
  - module
  - SMS
comments: true
---
<a href="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/07/IMG_2930.jpg" target="_blank"><img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/07/IMG_2930-300x200.jpg" alt="GPRS module before tear down" title="GPRS module before tear down" width="300" height="200" class="alignleft size-medium wp-image-546" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/07/IMG_2930-300x200.jpg 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/07/IMG_2930-1024x682.jpg 1024w" sizes="(max-width: 300px) 100vw, 300px" /></a>I&#8217;ve got this relatively [cheap GPRS modem from dealextreme](http://dx.com/p/usb-tri-band-gprs-modem-cell-phone-radio-gsm-900-1800-1900mhz-12057?item=5 "Cheap GPRS Modem DealExtreme") with the hope that I could hack it to send SMS&#8217;s through the serial interface.
  
Unfortunately the IMEI of this device is zeroed and thus invalid for local operators in this and other countries. I teared down the device in an attempt to hack it, but failed and I&#8217;ll show you why&#8230;
  
<!--more-->


  
According to the <a href="http://club.dx.com/reviews/text/12057/107065" title="comments about the modem on DX" target="_blank">comments in the DX</a> page, this modem was supposed to use a BENQ M32 module, and you can easily find some <a href="http://www.sure-electronics.net/rf,audio/GP-GC006-pdf.pdf" title="BENQ M32 datasheet" target="_blank">datasheets and pinout, etc</a> on the web. Well, **that&#8217;s wrong**, it&#8217;s not an M32 BUT an M31, which makes the story totally different. After reading this document, my idea was to take apart the module from the modem board and try to use the JTAG interface to read or alter the firmware/IMEI.
  
This is **what I was expecting** to see:
  
<figure id="attachment_548" style="width: 300px" class="wp-caption aligncenter">[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/07/BenQ_M32-300x228.png" alt="BenQ M32" title="BenQ M32" width="300" height="228" class="size-medium wp-image-548" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/07/BenQ_M32-300x228.png 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/07/BenQ_M32.png 450w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/07/BenQ_M32.png)<figcaption class="wp-caption-text">BenQ M32</figcaption></figure>
  
But after almost destroying the thing this is **what I&#8217;ve actually found**:
  
<figure id="attachment_547" style="width: 300px" class="wp-caption aligncenter">[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/07/IMG_2943-300x200.jpg" alt="BenQ M31" title="BenQ M31" width="300" height="200" class="size-medium wp-image-547" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/07/IMG_2943-300x200.jpg 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/07/IMG_2943-1024x682.jpg 1024w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2012/07/IMG_2943.jpg)<figcaption class="wp-caption-text">BenQ M31</figcaption></figure>
  
End of story, I couldn&#8217;t find the pinout of this chip anywhere on the Internet. You have been warned! don&#8217;t try this at home 😀
  
If you know the pinout, please let me know!
