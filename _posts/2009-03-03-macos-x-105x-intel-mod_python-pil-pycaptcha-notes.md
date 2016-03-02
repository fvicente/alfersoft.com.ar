---
uid: 89
title: MacOS X 10.5.x (Intel) + mod_python + PIL + PyCAPTCHA notes
date: 2009-03-03T23:04:10+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=89
permalink: /2009/03/03/macos-x-105x-intel-mod_python-pil-pycaptcha-notes/
categories:
  - Software Development
tags:
  - freetype2
  - intel
  - leopard
  - libjpeg
  - macosx
  - mod_python
  - pil
  - pycaptcha
  - python
comments: true
---
<figure>
	<img title="Python" alt="Python" src="{{ site.url }}/images/python.jpg">
	<figcaption>Photo Credit: <a title="Python" href="http://commons.wikimedia.org/wiki/File:Boomslang_(443113819).jpg" target="_blank">Wikimedia Commons</a></figcaption>
</figure>
Photo Credit: [Wikimedia Commons](http://commons.wikimedia.org/wiki/File:Boomslang_(443113819).jpg "Python"){:target="_blank"}

If you have a Mac OS X Leopard on an Intel platform, and you are interested in installing mod\_python using your default apache setup and the python that comes with the Framework, even more if you want to give PyCAPTCHA a try together with mod\_python, then check out this post! I&#8217;m covering also the installation of required libraries for PyCAPTCHA under Leopard (FreeType2, libjpeg, PIL)

<!--more-->

## mod_python

### Compile mod_python

If you have Apple&#8217;s Xcode installed, compiling mod_python for Leopard to use with the system provided versions of httpd, python, etc. should be as simple as running these commands:

{% highlight bash %}
mkdir -p ~/mod_python
cd ~/mod_python
svn co http://svn.apache.org/repos/asf/quetzalcoatl/mod_python/trunk mod_python-trunk
cd mod_python-trunk
./configure --with-apxs=/usr/sbin/apxs
make
sudo make install
cd test/
python test.py
{% endhighlight %}

[Original source](http://www.modpython.org/pipermail/mod_python/2008-March/025012.html)

### Configure Apache to load mod_python

Open `/private/etc/apache2/httpd.conf` with your favorite editor and add the following line:

{% highlight apache %}
LoadModule python_module libexec/apache2/mod_python.so
{% endhighlight %}

<figure style="text-align: center;">
	<figcaption>Example of how to configure Apache to load a mod_python application</figcaption>
</figure>


This is just an example, I recommend reading the [mod_python manual](http://www.modpython.org/live/current/doc-html/){:target="_blank"} for more information.

Open /private/etc/apache2/users/_username_.conf with your favorite editor and add:

{% highlight apache %}
Alias / "/Users/username/htdocs/"
<Directory "/Users/username/htdocs/">
	SetHandler python-program
	PythonHandler mod_python.publisher
	# Disable following line when in production
	PythonDebug On
	<Files ~ "\.(gif|jpg|png|ico|js|css)$">
		SetHandler default-handler
	</Files>
	Order allow,deny
	Allow from all
</Directory>
{% endhighlight %}

Restart Apache with the command below:

{% highlight bash %}
/usr/sbin/apachectl restart
{% endhighlight %}


## PyCAPTCHA

In order to build and install PyCAPTCHA you&#8217;ll need to compile, or make sure that you have these required libraries:

  * FreeType2
  * libjpeg
  * PIL (Python Imaging Library)

Let&#8217;s see how to set up these libraries under a Leopard / Intel environment.


## FreeType2

This library is required for PyCAPTCHA, as far as I know it is already installed with the SDK, to check the directory where the library is installed use the command:

{% highlight bash %}
locate libfreetype.dylib
{% endhighlight %}

A version of this library should appear under `/Developer/SDKs/MacOSX10.5.sdk/usr/X11/`


## libjpeg

Download [jpeg source](http://www.ijg.org/ "libjpeg"){:target="_blank"} and uncompress it in any directory, e.g. ~/jpeg-6b then:

{% highlight bash %}
cd ~/jpeg-6b
chmod -R 777 *
./configure
{% endhighlight %}

Edit Makefile and change the following line from:

{% highlight makefile %}
CFLAGS= -O2 -I$(srcdir)
{% endhighlight %}

to:

{% highlight makefile %}
CFLAGS= -arch ppc -arch i386 -arch ppc64 -arch x86_64 -O2 -I$(srcdir)
{% endhighlight %}

Finally, build and install:
{% highlight bash %}
make
sudo make install-lib
{% endhighlight %}


## PIL

Download [PIL source](http://www.pythonware.com/products/pil/ "PIL source"){:target="_blank"} and uncompress it in any directory, e.g. `~/imaging-1.1.6` then:

{% highlight bash %}
cd ~/imaging-1.1.6
{% endhighlight %}

Edit setup.py and change the following lines from:

{% highlight python %}
    FREETYPE_ROOT = None
{% endhighlight %}

to (note that I&#8217;m using the FreeType2 directory discovered previously with the locate command):

{% highlight python %}
    FREETYPE_ROOT = libinclude("/Developer/SDKs/MacOSX10.5.sdk/usr/X11/")
{% endhighlight %}

and then following line from:

{% highlight python %}
    JPEG_ROOT = None
{% endhighlight %}

to:

{% highlight python %}
    JPEG_ROOT = libinclude("/usr/local")
{% endhighlight %}

Locate these lines:

{% highlight python %}
from distutils import sysconfig
from distutils.core import Extension, setup
from distutils.command.build_ext import build_ext
{% endhighlight %}

And add the following lines (correctly indented from column 0):

{% highlight python %}
OrigExtension = Extension

def Extension(*args, **kwargs):
    extra_args = ['-arch', 'ppc', '-arch', 'ppc64', '-arch', 'i386', '-arch', 'x86_64']
    kwargs['extra_compile_args'] = extra_args + kwargs.get('extra_compile_args', [])
    kwargs['extra_link_args'] = extra_args + kwargs.get('extra_link_args', [])
    return OrigExtension(*args, **kwargs)
{% endhighlight %}

Now proceed to compile and install the library:

{% highlight bash %}
sudo python setup.py install
{% endhighlight %}


## Install PyCAPTCHA

Now you are ready to build and install PyCAPTCHA. Checkout the source code from [this link](http://svn.navi.cx/misc/trunk/pycaptcha/ "PyCAPTCHA"){:target="_blank"} and run the following command:

{% highlight bash %}
sudo python setup.py install
{% endhighlight %}

That&#8217;s it!

