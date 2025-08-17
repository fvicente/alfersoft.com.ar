---
uid: 28
title: 'Asterisk PBX with X100P Clone (Part 1: Installation)'
date: 2008-11-01T20:08:54+00:00
author: alf
layout: single
guid: ?p=28
categories:
  - Software Applications
tags:
  - asterisk
  - clone
  - config
  - debian
  - install
  - intel
  - setup
  - ubuntu
  - x100p
comments: true
excerpt: Installing Asterisk PBX with an Intel X100P clone FXO to allow my extensions to make (and receive) phone calls to the telco line
---
## [<img class="size-full wp-image-78 alignnone" style="margin: 10px 0px;" title="asterisk" src="{{ site.baseurl }}/images/asterisk.png" alt="" width="162" height="91" />]({{ site.baseurl }}/images/asterisk.png)

## Introduction

Here you have the first part of my experience on installing Asterisk PBX with an Intel X100P clone FXO to allow my extensions to make (and receive) phone calls to the telco line.

<!--more-->

## Hardware

Though Digium (the Asterisk developer) recommends a Pentium 3 based PC as the minimum requirements to run a home PBX, I did it using a really old AMD K6-2 500 with 128Mb DIMM and a 4200rpm 3.5Gb hard disk, and it seems to do the job just fine considering the ultra-low traffic I&#8217;m having (only 4 extensions and they&#8217;re not using it all the time). If you&#8217;re planning to use it as a full PBX with dozens of extensions, be more generous on hardware.

If you&#8217;re just starting on Asterisk and you&#8217;re wondering about if your old modem would work to connect your PBX to the telco, let me explain you some basic things before move on.

To connect your PBX to the telco line you&#8217;ll need an [FXO](http://en.wikipedia.org/wiki/FXO) card. As Asterisk is a free and open-source project, one of Digium&#8217;s ways to get funds is selling propietary cards to do this job, but when the project started they were using some standard 56K modem/fax and they are still supported, but not every modem will work.

[As stated here](http://www.voip-info.org/wiki/view/X100P+clone), to use a modem as a FXO you&#8217;ll need one of these chipsets:

  * Intel 537PG and 537PU
  * Ambient MD3200
  * Motorola 62802

I&#8217;m using one with the Ambient MD3200 chipset, and works pretty good. These modems will provide you with a single port to connect it to a single line, but if you need more lines you could add more cheap modems, or just buy a real [FXO multiline analog card](http://www.digium.com/en/products/analog/) or even an [E1/T1 card](http://www.digium.com/en/products/digital/) which supports digital telephony channels, but these boards are out of the scope of this tutorial.

## Choosing the right installation

You have two options to install Asterisk on your hardware: install everything separately by hand, or just download AsteriskNOW. [AsteriskNOW](http://www.asterisknow.org/) is a Linux distribution which installs by default an Asterisk PBX along with its GUI. I&#8217;ve installed Asterisk by hand, as AsteriskNOW refused to install on my K6-2 because my &#8220;CPU too old for this kernel&#8221;.

I&#8217;ve tried this installation procedure on both [Debian 4 rc3](http://www.debian.org/) and [Ubuntu 8.04](http://www.ubuntu.com/) desktop, and worked the same on both.

First of all, install the Linux of your choice. If you got old hardware as I do, then I&#8217;ll recommend you Debian 4 NetInstall, because it will install just the software you want and will not fill your system memory with unused daemons. If your hardware is new (say an Intel Core2Duo or such) then go for Ubuntu, as it got more hardware support than Debian.

Once you&#8217;ve your server up and running, open a console and do `lspci` (if you don&#8217;t have it installed, do `aptitude install pciutils` as root), and you will see something like:

`00:0a.0 Communication controller: Tiger Jet Network Inc. Tiger3XX Modem/ISDN interface`

If everything is OK, then move on to the next step.

## Installing Asterisk

This guide was taken from [Chad&#8217;s Blog](http://blog.chadwollenberg.com/2008/08/26/asterisk-on-hardy-server/), and is a great step-by-step guide to install Asterisk from source. If you do copy+paste of the following code lines inside a terminal window you&#8217;ll have almost a zero-effort installation.

First of all, you need to be root. Do `sudo su`, insert your password, and then install the compiler packages:

{% highlight bash %}
apt-get install linux-headers-$(uname -r) build-essential automake autoconf bison flex libtool libncurses5-dev libssl-dev subversion
{% endhighlight %}

Now it&#8217;s time to download Asterisk, Zaptel (X100P driver) and Asterisk&#8217;s libraries LibPri:

{% highlight bash %}
wget http://downloads.digium.com/pub/asterisk/releases/asterisk-1.4.21.2.tar.gz
wget http://downloads.digium.com/pub/zaptel/releases/zaptel-1.4.12.tar.gz
wget http://downloads.digium.com/pub/libpri/releases/libpri-1.4.7.tar.gz
tar xfvz zaptel-1.4.12.tar.gz -C /usr/src/
tar xfvz asterisk-1.4.21.2.tar.gz -C /usr/src/
tar xfvz libpri-1.4.7.tar.gz -C /usr/src/
{% endhighlight %}

Start the compilation scripts of the three packages:

{% highlight bash %}
cd /usr/src/zaptel-1.4.12
./configure
make
make install
make config
cd ../libpri-1.4.7
make
make install
cd ../asterisk-1.4.21.2/
./configure
make
make install
make samples
{% endhighlight %}

And finally it&#8217;s time to install Asterisk GUI. This GUI will make your Asterisk configuration a piece of cake, and you&#8217;ll need it to follow the part 2 of this tutorial:

{% highlight bash %}
cd /usr/src/
svn co http://svn.digium.com/svn/asterisk-gui/branches/2.0 asterisk-gui
cd asterisk-gui
./configure
make
make install
{% endhighlight %}


## Basic configuration

Once you have everything compiled and installed, you can do `make checkconfig` at the prompt. You&#8217;ll see some warnings, because you need to make a couple changes in the configuration files to start Asterisk.

Always as root, go to `/etc/asterisk` and make sure these lines exists and are uncommented:

In `http.conf`:

{% highlight INI %}
enabled = yes
enablestatic = yes
bindaddr = 0.0.0.0
{% endhighlight %}

In `manager.conf`:

{% highlight INI %}
enabled = yes
webenabled = yes
bindaddr = 0.0.0.0
{% endhighlight %}

Also in `manager.conf` you should create an admin account to access the GUI:

{% highlight INI %}
[admin]
secret = yourpasswordhere
read = system,call,log,verbose,command,agent,config
write = system,call,log,verbose,command,agent,config
{% endhighlight %}

Finally, add the following to your `/etc/rc.local` file to load Zaptel driver and Asterisk at startup:

{% highlight bash %}
modprobe zaptel
modprobe ztdummy
asterisk –g
{% endhighlight %}

Now it&#8217;s time to run Asterisk for the first time. Reboot your server to make sure everything is loaded and working. If there were no FXO port detected, do:

{% highlight bash %}
ztscan > /etc/asterisk/ztscan.conf
{% endhighlight %}

And reboot again.
