---
uid: 153
title: Building ffmpeg in Debian (Lenny)
date: 2010-11-01T12:11:53+00:00
author: fvicente
layout: single
guid: ?p=153
categories:
  - Linux FAQ
  - Software Applications
tags:
  - aac
  - codec
  - convert
  - debian
  - ffmpeg
  - lame
  - linux
  - mp3
comments: true
excerpt: Building ffmpeg with AAC support might be trickier than you tought
---
I&#8217;ve adapted the instructions from [this Ubuntu thread](http://ubuntuforums.org/showthread.php?t=786095) for Debian Lenny.

The original idea was to build ffmpeg with AAC support, but then I&#8217;ve found some other problems with mp3 codec, so I needed to apply a patch and rebuild everything.

<!--more-->

So, I&#8217;m writing all the procedures in this post in a few steps, hoping that I&#8217;m not forgetting anything. I know that some steps could be avoided (e.g. building ffmpeg twice), but I&#8217;ll just transcribe everything the way that worked for me (as I remember). 

**First try**

First edit your aptitude sources list

{% highlight bash %}
nano /etc/apt/sources.list
{% endhighlight %}

Add the following repositories

{% highlight bash %}
deb http://www.debian-multimedia.org stable main
deb http://www.backports.org/debian lenny-backports main contrib non-free
{% endhighlight %}

Update

{% highlight bash %}
sudo apt-get update
{% endhighlight %}

Remove current ffmpeg (if installed)

{% highlight bash %}
sudo apt-get remove ffmpeg x264 libx264-dev
{% endhighlight %}

Install necessary packages for the build. Note that I&#8217;ve removed libvpx-dev from the original Ubuntu thread.

{% highlight bash %}
sudo apt-get install build-essential subversion git-core checkinstall yasm texi2html \
    libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libsdl1.2-dev \
    libtheora-dev libvorbis-dev libx11-dev libxfixes-dev libxvidcore-dev \
    zlib1g-dev
{% endhighlight %}

Build and install x264.

{% highlight bash %}
cd
git clone git://git.videolan.org/x264.git
cd x264
./configure
make
sudo checkinstall --pkgname=x264 --pkgversion "2:0.`grep X264_BUILD x264.h -m1 | \
    cut -d' ' -f3`.`git rev-list HEAD | wc -l`+git`git rev-list HEAD -n 1 | \
    head -c 7`" --backup=no --deldoc=yes --fstrans=no --default
{% endhighlight %}

Build and install ffmpeg. Note that I&#8217;ve removed the option &#8211;enable-libtheora and &#8211;enable-libvpx from the original Ubuntu thread.

{% highlight bash %}
cd
svn checkout svn://svn.ffmpeg.org/ffmpeg/trunk ffmpeg
cd ffmpeg
./configure --enable-gpl --enable-version3 --enable-nonfree --enable-postproc \
    --enable-libfaac --enable-libmp3lame --enable-libopencore-amrnb \
    --enable-libopencore-amrwb --enable-libvorbis \
    --enable-libx264 --enable-libxvid --enable-x11grab
make
sudo checkinstall --pkgname=ffmpeg --pkgversion "4:SVN-r`LANG=C svn info | \
    grep Revision | awk '{ print $NF }'`" --backup=no --deldoc=yes --fstrans=no \
    --default
hash x264 ffmpeg ffplay
{% endhighlight %}

**Almost there**

Ok, everything works perfectly until I try to convert a video and I get the following error:

`lame: output buffer too small`

After a bit of googling, I&#8217;ve found a patch in [this post](https://roundup.ffmpeg.org/issue803), with an interesting discussion regarding to whom should fix the problem (ffmpeg vs. lame). It is also suggested to downgrade the lame library, but I&#8217;ve tried that and didn&#8217;t worked for me, so in summary, let&#8217;s simply apply this ffmpeg patch and everything will be fine.

1. Go to the ffmpeg source directory and get the patch

{% highlight bash %}
cd
cd ffmpeg
wget --no-check-certificate https://roundup.ffmpeg.org/file831/ffmpeg-lame-flush.patch3
{% endhighlight %}

2. Apply the patch

{% highlight bash %}
patch < ffmpeg-lame-flush.patch3
## (when the file name is required enter: libavcodec/libmp3lame.c)
{% endhighlight %}

3. Build and install ffmpeg again

{% highlight bash %}
make
sudo checkinstall --pkgname=ffmpeg --pkgversion "4:SVN-r`LANG=C svn info | \
    grep Revision | awk '{ print $NF }'`" --backup=no --deldoc=yes --fstrans=no \
    --default
{% endhighlight %}

And now you're ready to go!
