---
uid: 443
title: Parallel Port Sniffer for Linux!
date: 2012-02-23T14:20:20+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=443
permalink: /2012/02/23/parallel-port-sniffer-for-linux/
categories:
  - Linux FAQ
  - Software Applications
  - Software Development
tags:
  - linux
  - logic analyzer
  - parallel
  - parport
  - port
  - qemu
  - sniffer
  - virtual
  - windows
comments: true
---
[<img src="{{ site.baseurl }}/images/parportsnif.png" alt="Parallel Port Sniffer" title="Parallel Port Sniffer"/>]({{ site.baseurl }}/images/parportsnif.png)

This software will help you to capture the parallel port data using a small kernel module for Linux. But wait, not only Linux, it also works for Windows, if you use it in a Qemu Virtual Machine under Linux!.

The parallel port sniffer kernel module will create a virtual port for every &#8220;real&#8221; parallel port in your machine and act like a bridge, re-passing all the commands sent to the virtual port to the real one and capturing the data in real time. The generated output is in the <a href="http://dangerousprototypes.com/docs/Open_Bench_Logic_Sniffer" title="Open Bench Logic Sniffer" target="_blank">Open Bench Logic Sniffer</a> format and ready to be analyzed with tools like <a href="http://www.lxtreme.nl/ols/" title="Logic Sniffer" target="_blank">Logic Sniffer</a> which has tools to analyze some known protocols like 1-Wire, I2C, JTAG, SPI, UART, etc.

<!--more-->

## Details

Once the module is built for your kernel and loaded, it will create one character device for every real parallel port with the name `/dev/parportsnif*`. Just as a side note, the name parportsnif was deliberately chose, because Qemu only accepts names starting with &#8220;parport&#8221; to be parallel port!
  
The output will be sent to `/proc/parportlog`. Currently there is one output for all the ports, which will mess things up if you use the sniffer in more than one parallel port at the same time, so please make sure to use only one sniffer at the time. There is a limit of 10000 lines of output that are kept in memory because it is not a good idea to keep allocating memory indefinitely on the kernel, this means that you need to keep flushing the output (`cat /dev/parportlog`) so you don&#8217;t miss any signal change.

## How to build and load the kernel module

First build the kernel object, using the provided `Makefile`:

{% highlight bash %}
make
{% endhighlight %}

If you have any printer that might be using the parallel port, it is recommended to unload the `lp` module with the following command:

{% highlight bash %}
sudo rmmod lp
{% endhighlight %}

Then proceed to load the sniffer module:

{% highlight bash %}
sudo insmod ./parportsnif.ko
{% endhighlight %}

Now you have one `/dev/parportsnif*` corresponding to each of your `/dev/parport*`.

Additionally you must have the `/proc/parportlog` file created.

## Testing

I&#8217;ve included a small C program called `test.c` that will turn all the data bits on and off a couple times. This can be used to test the resulting output file.

Note that you may need to edit the file and change the `/dev/parportsnif0` to your appropriate port number.

Then build using the following command:

{% highlight bash %}
gcc test.c -o test
{% endhighlight %}

Before running the test program, you need to start capturing the output, you can use the `cat` and `tee` commands to check the output on the terminal and save an output file at the same time:

{% highlight bash %}
cat /proc/parportlog | tee dump.ols
{% endhighlight %}

Then run the test program:

{% highlight bash %}
sudo ./test
{% endhighlight %}

The generated output in the `dump.ols` will look like this:

{% highlight nasm %}
;Rate: 1000000
;Channels: 32
0000000055@100137
00000000AA@200220
0000000055@300344
00000000AA@400439
0000000000@500556
# [1330005184.245259999] 0 CLOSE
{% endhighlight %}

Remember to press Ctrl+C to stop the output capturing.

That&#8217;s it, now you are ready to open the dump.ols with Logic Sniffer:

[<img src="{{ site.baseurl }}/images/logic_sniffer.png" alt="Screenshot LogicSniffer Logic Analyzer Client" title="Screenshot LogicSniffer Logic Analyzer Client"/>]({{ site.baseurl }}/images/logic_sniffer.png)

## How to use it with Qemu / Windows

With Qemu you need to specify a parameter called `-parallel` to specify the `/dev/parportsnif*` (where \* is the port number of course). Also, you may need to change the parportsnif\* permissions, e.g. `chmod 777 /dev/parportsnif0`.

If you use the QtEmu front-end (like me), you can use the &#8220;Additional QEMU Options&#8221; in the Settings screen:

[<img src="{{ site.baseurl }}/images/qtemu.png" alt="Screenshot QtEmu" title="Screenshot QtEmu"/>]({{ site.baseurl }}/images/qtemu.png)

Since the format of the capture is made for logic analyzer devices, each entry has a specific point in time (emulating a 1Mhz logic analyzer), and the base time is taken when the port is opened for the first time. Qemu will open the parallel port sniffer when Windows is starting, you will receive a lot of signal changes on the port from Windows itself (maybe looking for a printer or something like that), and then you will run the program that you really want to capture and analyze. So, your final file will have the first two lines of header that starts with &#8220;#&#8221; and a few entries at the beginning that you may want to delete (Windows initialization). You can use the `rebase.py` script provided to set the timing to the beginning of the time-line.

## Special Thanks

To Rodrigo Zechin Rosauro who helped me to develop this module!!!

## Download

<a title="Download Parallel Port Sniffer" markdown="0" href="https://github.com/fvicente/parportsnif/archive/master.zip" class="btn">Download Source Code</a>

[Parallel Port Sniffer on GitHub](https://github.com/fvicente/parportsnif "Parallel Port Sniffer on GitHub"){:target="_blank"}
