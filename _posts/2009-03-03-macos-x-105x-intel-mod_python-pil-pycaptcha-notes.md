---
id: 89
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
<img class="alignnone" title="Python" src="http://www.alfersoft.com.ar/files/python.jpg" alt="" width="300" height="225" />

If you have a Mac OS X Leopard on an Intel platform, and you are interested in installing mod\_python using your default apache setup and the python that comes with the Framework, even more if you want to give PyCAPTCHA a try together with mod\_python, then check out this post! I&#8217;m covering also the installation of required libraries for PyCAPTCHA under Leopard (FreeType2, libjpeg, PIL)

<!--more-->

## **mod_python**

<span style="text-decoration: underline;">Compile mod_python</span>

If you have Apple&#8217;s Xcode installed, compiling mod_python for Leopard to use with the system provided versions of httpd, python, etc. should be as simple as running these commands:

`<br />
$ mkdir -p ~/mod_python<br />
$ cd ~/mod_python<br />
$ svn co http://svn.apache.org/repos/asf/quetzalcoatl/mod_python/trunk mod_python-trunk<br />
$ cd mod_python-trunk<br />
$ ./configure --with-apxs=/usr/sbin/apxs<br />
$ make<br />
$ sudo make install<br />
$ cd test/<br />
$ python test.py`

(original source: http://www.modpython.org/pipermail/mod_python/2008-March/025012.html)

<span style="text-decoration: underline;">Configure Apache to load mod_python</span>

Open /private/etc/apache2/httpd.conf with your favorite editor and add the following line:

``<img class="alignnone" title="Python" src="http://www.alfersoft.com.ar/files/python.jpg" alt="" width="300" height="225" />

If you have a Mac OS X Leopard on an Intel platform, and you are interested in installing mod\_python using your default apache setup and the python that comes with the Framework, even more if you want to give PyCAPTCHA a try together with mod\_python, then check out this post! I&#8217;m covering also the installation of required libraries for PyCAPTCHA under Leopard (FreeType2, libjpeg, PIL)

<!--more-->

## **mod_python**

<span style="text-decoration: underline;">Compile mod_python</span>

If you have Apple&#8217;s Xcode installed, compiling mod_python for Leopard to use with the system provided versions of httpd, python, etc. should be as simple as running these commands:

`<br />
$ mkdir -p ~/mod_python<br />
$ cd ~/mod_python<br />
$ svn co http://svn.apache.org/repos/asf/quetzalcoatl/mod_python/trunk mod_python-trunk<br />
$ cd mod_python-trunk<br />
$ ./configure --with-apxs=/usr/sbin/apxs<br />
$ make<br />
$ sudo make install<br />
$ cd test/<br />
$ python test.py`

(original source: http://www.modpython.org/pipermail/mod_python/2008-March/025012.html)

<span style="text-decoration: underline;">Configure Apache to load mod_python</span>

Open /private/etc/apache2/httpd.conf with your favorite editor and add the following line:

`` 

<span style="text-decoration: underline;">Example of how to configure Apache to load a mod_python application</span>

This is just an example, I recommend reading the <a title="mod_python manual" href="http://www.modpython.org/live/current/doc-html/" target="_blank">mod_python manual</a> for more information.

Open /private/etc/apache2/users/_username_.conf with your favorite editor and add:

<pre>Alias / "/Users/<em>username</em>/<em>htdocs</em>/"
&lt;Directory "/Users/<em>username</em>/<em>htdocs</em>/"&gt;
    SetHandler python-program
    PythonHandler mod_python.publisher
    # Disable following line when in production
    PythonDebug On
    &lt;Files ~ "\.(gif|jpg|png|ico|js|css)$"&gt;
        SetHandler default-handler
    &lt;/Files&gt;
    Order allow,deny
    Allow from all
&lt;/Directory&gt;</pre>

<span style="text-decoration: underline;">Restart Apache with the command below:</span>

<pre>$ /usr/sbin/apachectl restart</pre>



## PyCAPTCHA

In order to build and install PyCAPTCHA you&#8217;ll need to compile, or make sure that you have these required libraries:

  * FreeType2
  * libjpeg
  * PIL (Python Imaging Library)

Let&#8217;s see how to set up these libraries under a Leopard / Intel environment.
  


## FreeType2

This library is required for PyCAPTCHA, as far as I know it is already installed with the SDK, to check the directory where the library is installed use the command:

`$ locate libfreetype.dylib`

A version of this library should appear under /Developer/SDKs/MacOSX10.5.sdk/usr/X11/
  


## libjpeg

Download <a title="libjpeg" href="http://www.ijg.org/" target="_blank">jpeg source</a> and uncompress it in any directory, e.g. ~/jpeg-6b then:

```<img class="alignnone" title="Python" src="http://www.alfersoft.com.ar/files/python.jpg" alt="" width="300" height="225" />

If you have a Mac OS X Leopard on an Intel platform, and you are interested in installing mod\_python using your default apache setup and the python that comes with the Framework, even more if you want to give PyCAPTCHA a try together with mod\_python, then check out this post! I&#8217;m covering also the installation of required libraries for PyCAPTCHA under Leopard (FreeType2, libjpeg, PIL)

<!--more-->

## **mod_python**

<span style="text-decoration: underline;">Compile mod_python</span>

If you have Apple&#8217;s Xcode installed, compiling mod_python for Leopard to use with the system provided versions of httpd, python, etc. should be as simple as running these commands:

`<br />
$ mkdir -p ~/mod_python<br />
$ cd ~/mod_python<br />
$ svn co http://svn.apache.org/repos/asf/quetzalcoatl/mod_python/trunk mod_python-trunk<br />
$ cd mod_python-trunk<br />
$ ./configure --with-apxs=/usr/sbin/apxs<br />
$ make<br />
$ sudo make install<br />
$ cd test/<br />
$ python test.py`

(original source: http://www.modpython.org/pipermail/mod_python/2008-March/025012.html)

<span style="text-decoration: underline;">Configure Apache to load mod_python</span>

Open /private/etc/apache2/httpd.conf with your favorite editor and add the following line:

``<img class="alignnone" title="Python" src="http://www.alfersoft.com.ar/files/python.jpg" alt="" width="300" height="225" />

If you have a Mac OS X Leopard on an Intel platform, and you are interested in installing mod\_python using your default apache setup and the python that comes with the Framework, even more if you want to give PyCAPTCHA a try together with mod\_python, then check out this post! I&#8217;m covering also the installation of required libraries for PyCAPTCHA under Leopard (FreeType2, libjpeg, PIL)

<!--more-->

## **mod_python**

<span style="text-decoration: underline;">Compile mod_python</span>

If you have Apple&#8217;s Xcode installed, compiling mod_python for Leopard to use with the system provided versions of httpd, python, etc. should be as simple as running these commands:

`<br />
$ mkdir -p ~/mod_python<br />
$ cd ~/mod_python<br />
$ svn co http://svn.apache.org/repos/asf/quetzalcoatl/mod_python/trunk mod_python-trunk<br />
$ cd mod_python-trunk<br />
$ ./configure --with-apxs=/usr/sbin/apxs<br />
$ make<br />
$ sudo make install<br />
$ cd test/<br />
$ python test.py`

(original source: http://www.modpython.org/pipermail/mod_python/2008-March/025012.html)

<span style="text-decoration: underline;">Configure Apache to load mod_python</span>

Open /private/etc/apache2/httpd.conf with your favorite editor and add the following line:

`` 

<span style="text-decoration: underline;">Example of how to configure Apache to load a mod_python application</span>

This is just an example, I recommend reading the <a title="mod_python manual" href="http://www.modpython.org/live/current/doc-html/" target="_blank">mod_python manual</a> for more information.

Open /private/etc/apache2/users/_username_.conf with your favorite editor and add:

<pre>Alias / "/Users/<em>username</em>/<em>htdocs</em>/"
&lt;Directory "/Users/<em>username</em>/<em>htdocs</em>/"&gt;
    SetHandler python-program
    PythonHandler mod_python.publisher
    # Disable following line when in production
    PythonDebug On
    &lt;Files ~ "\.(gif|jpg|png|ico|js|css)$"&gt;
        SetHandler default-handler
    &lt;/Files&gt;
    Order allow,deny
    Allow from all
&lt;/Directory&gt;</pre>

<span style="text-decoration: underline;">Restart Apache with the command below:</span>

<pre>$ /usr/sbin/apachectl restart</pre>



## PyCAPTCHA

In order to build and install PyCAPTCHA you&#8217;ll need to compile, or make sure that you have these required libraries:

  * FreeType2
  * libjpeg
  * PIL (Python Imaging Library)

Let&#8217;s see how to set up these libraries under a Leopard / Intel environment.
  


## FreeType2

This library is required for PyCAPTCHA, as far as I know it is already installed with the SDK, to check the directory where the library is installed use the command:

`$ locate libfreetype.dylib`

A version of this library should appear under /Developer/SDKs/MacOSX10.5.sdk/usr/X11/
  


## libjpeg

Download <a title="libjpeg" href="http://www.ijg.org/" target="_blank">jpeg source</a> and uncompress it in any directory, e.g. ~/jpeg-6b then:

``` 

Edit Makefile and change the following line from:

<pre>CFLAGS= -O2 -I$(srcdir)</pre>

to:

<pre>CFLAGS= -arch ppc -arch i386 -arch ppc64 -arch x86_64 -O2 -I$(srcdir)</pre>

Finally, build and install:
  
`<br />
$ make<br />
$ sudo make install-lib`
  


## PIL

Download <a title="PIL source" href="http://www.pythonware.com/products/pil/" target="_blank">PIL source</a> and uncompress it in any directory, e.g. ~/imaging-1.1.6 then:

````<img class="alignnone" title="Python" src="http://www.alfersoft.com.ar/files/python.jpg" alt="" width="300" height="225" />

If you have a Mac OS X Leopard on an Intel platform, and you are interested in installing mod\_python using your default apache setup and the python that comes with the Framework, even more if you want to give PyCAPTCHA a try together with mod\_python, then check out this post! I&#8217;m covering also the installation of required libraries for PyCAPTCHA under Leopard (FreeType2, libjpeg, PIL)

<!--more-->

## **mod_python**

<span style="text-decoration: underline;">Compile mod_python</span>

If you have Apple&#8217;s Xcode installed, compiling mod_python for Leopard to use with the system provided versions of httpd, python, etc. should be as simple as running these commands:

`<br />
$ mkdir -p ~/mod_python<br />
$ cd ~/mod_python<br />
$ svn co http://svn.apache.org/repos/asf/quetzalcoatl/mod_python/trunk mod_python-trunk<br />
$ cd mod_python-trunk<br />
$ ./configure --with-apxs=/usr/sbin/apxs<br />
$ make<br />
$ sudo make install<br />
$ cd test/<br />
$ python test.py`

(original source: http://www.modpython.org/pipermail/mod_python/2008-March/025012.html)

<span style="text-decoration: underline;">Configure Apache to load mod_python</span>

Open /private/etc/apache2/httpd.conf with your favorite editor and add the following line:

``<img class="alignnone" title="Python" src="http://www.alfersoft.com.ar/files/python.jpg" alt="" width="300" height="225" />

If you have a Mac OS X Leopard on an Intel platform, and you are interested in installing mod\_python using your default apache setup and the python that comes with the Framework, even more if you want to give PyCAPTCHA a try together with mod\_python, then check out this post! I&#8217;m covering also the installation of required libraries for PyCAPTCHA under Leopard (FreeType2, libjpeg, PIL)

<!--more-->

## **mod_python**

<span style="text-decoration: underline;">Compile mod_python</span>

If you have Apple&#8217;s Xcode installed, compiling mod_python for Leopard to use with the system provided versions of httpd, python, etc. should be as simple as running these commands:

`<br />
$ mkdir -p ~/mod_python<br />
$ cd ~/mod_python<br />
$ svn co http://svn.apache.org/repos/asf/quetzalcoatl/mod_python/trunk mod_python-trunk<br />
$ cd mod_python-trunk<br />
$ ./configure --with-apxs=/usr/sbin/apxs<br />
$ make<br />
$ sudo make install<br />
$ cd test/<br />
$ python test.py`

(original source: http://www.modpython.org/pipermail/mod_python/2008-March/025012.html)

<span style="text-decoration: underline;">Configure Apache to load mod_python</span>

Open /private/etc/apache2/httpd.conf with your favorite editor and add the following line:

`` 

<span style="text-decoration: underline;">Example of how to configure Apache to load a mod_python application</span>

This is just an example, I recommend reading the <a title="mod_python manual" href="http://www.modpython.org/live/current/doc-html/" target="_blank">mod_python manual</a> for more information.

Open /private/etc/apache2/users/_username_.conf with your favorite editor and add:

<pre>Alias / "/Users/<em>username</em>/<em>htdocs</em>/"
&lt;Directory "/Users/<em>username</em>/<em>htdocs</em>/"&gt;
    SetHandler python-program
    PythonHandler mod_python.publisher
    # Disable following line when in production
    PythonDebug On
    &lt;Files ~ "\.(gif|jpg|png|ico|js|css)$"&gt;
        SetHandler default-handler
    &lt;/Files&gt;
    Order allow,deny
    Allow from all
&lt;/Directory&gt;</pre>

<span style="text-decoration: underline;">Restart Apache with the command below:</span>

<pre>$ /usr/sbin/apachectl restart</pre>



## PyCAPTCHA

In order to build and install PyCAPTCHA you&#8217;ll need to compile, or make sure that you have these required libraries:

  * FreeType2
  * libjpeg
  * PIL (Python Imaging Library)

Let&#8217;s see how to set up these libraries under a Leopard / Intel environment.
  


## FreeType2

This library is required for PyCAPTCHA, as far as I know it is already installed with the SDK, to check the directory where the library is installed use the command:

`$ locate libfreetype.dylib`

A version of this library should appear under /Developer/SDKs/MacOSX10.5.sdk/usr/X11/
  


## libjpeg

Download <a title="libjpeg" href="http://www.ijg.org/" target="_blank">jpeg source</a> and uncompress it in any directory, e.g. ~/jpeg-6b then:

```<img class="alignnone" title="Python" src="http://www.alfersoft.com.ar/files/python.jpg" alt="" width="300" height="225" />

If you have a Mac OS X Leopard on an Intel platform, and you are interested in installing mod\_python using your default apache setup and the python that comes with the Framework, even more if you want to give PyCAPTCHA a try together with mod\_python, then check out this post! I&#8217;m covering also the installation of required libraries for PyCAPTCHA under Leopard (FreeType2, libjpeg, PIL)

<!--more-->

## **mod_python**

<span style="text-decoration: underline;">Compile mod_python</span>

If you have Apple&#8217;s Xcode installed, compiling mod_python for Leopard to use with the system provided versions of httpd, python, etc. should be as simple as running these commands:

`<br />
$ mkdir -p ~/mod_python<br />
$ cd ~/mod_python<br />
$ svn co http://svn.apache.org/repos/asf/quetzalcoatl/mod_python/trunk mod_python-trunk<br />
$ cd mod_python-trunk<br />
$ ./configure --with-apxs=/usr/sbin/apxs<br />
$ make<br />
$ sudo make install<br />
$ cd test/<br />
$ python test.py`

(original source: http://www.modpython.org/pipermail/mod_python/2008-March/025012.html)

<span style="text-decoration: underline;">Configure Apache to load mod_python</span>

Open /private/etc/apache2/httpd.conf with your favorite editor and add the following line:

``<img class="alignnone" title="Python" src="http://www.alfersoft.com.ar/files/python.jpg" alt="" width="300" height="225" />

If you have a Mac OS X Leopard on an Intel platform, and you are interested in installing mod\_python using your default apache setup and the python that comes with the Framework, even more if you want to give PyCAPTCHA a try together with mod\_python, then check out this post! I&#8217;m covering also the installation of required libraries for PyCAPTCHA under Leopard (FreeType2, libjpeg, PIL)

<!--more-->

## **mod_python**

<span style="text-decoration: underline;">Compile mod_python</span>

If you have Apple&#8217;s Xcode installed, compiling mod_python for Leopard to use with the system provided versions of httpd, python, etc. should be as simple as running these commands:

`<br />
$ mkdir -p ~/mod_python<br />
$ cd ~/mod_python<br />
$ svn co http://svn.apache.org/repos/asf/quetzalcoatl/mod_python/trunk mod_python-trunk<br />
$ cd mod_python-trunk<br />
$ ./configure --with-apxs=/usr/sbin/apxs<br />
$ make<br />
$ sudo make install<br />
$ cd test/<br />
$ python test.py`

(original source: http://www.modpython.org/pipermail/mod_python/2008-March/025012.html)

<span style="text-decoration: underline;">Configure Apache to load mod_python</span>

Open /private/etc/apache2/httpd.conf with your favorite editor and add the following line:

`` 

<span style="text-decoration: underline;">Example of how to configure Apache to load a mod_python application</span>

This is just an example, I recommend reading the <a title="mod_python manual" href="http://www.modpython.org/live/current/doc-html/" target="_blank">mod_python manual</a> for more information.

Open /private/etc/apache2/users/_username_.conf with your favorite editor and add:

<pre>Alias / "/Users/<em>username</em>/<em>htdocs</em>/"
&lt;Directory "/Users/<em>username</em>/<em>htdocs</em>/"&gt;
    SetHandler python-program
    PythonHandler mod_python.publisher
    # Disable following line when in production
    PythonDebug On
    &lt;Files ~ "\.(gif|jpg|png|ico|js|css)$"&gt;
        SetHandler default-handler
    &lt;/Files&gt;
    Order allow,deny
    Allow from all
&lt;/Directory&gt;</pre>

<span style="text-decoration: underline;">Restart Apache with the command below:</span>

<pre>$ /usr/sbin/apachectl restart</pre>



## PyCAPTCHA

In order to build and install PyCAPTCHA you&#8217;ll need to compile, or make sure that you have these required libraries:

  * FreeType2
  * libjpeg
  * PIL (Python Imaging Library)

Let&#8217;s see how to set up these libraries under a Leopard / Intel environment.
  


## FreeType2

This library is required for PyCAPTCHA, as far as I know it is already installed with the SDK, to check the directory where the library is installed use the command:

`$ locate libfreetype.dylib`

A version of this library should appear under /Developer/SDKs/MacOSX10.5.sdk/usr/X11/
  


## libjpeg

Download <a title="libjpeg" href="http://www.ijg.org/" target="_blank">jpeg source</a> and uncompress it in any directory, e.g. ~/jpeg-6b then:

``` 

Edit Makefile and change the following line from:

<pre>CFLAGS= -O2 -I$(srcdir)</pre>

to:

<pre>CFLAGS= -arch ppc -arch i386 -arch ppc64 -arch x86_64 -O2 -I$(srcdir)</pre>

Finally, build and install:
  
`<br />
$ make<br />
$ sudo make install-lib`
  


## PIL

Download <a title="PIL source" href="http://www.pythonware.com/products/pil/" target="_blank">PIL source</a> and uncompress it in any directory, e.g. ~/imaging-1.1.6 then:

```` 

Edit setup.py and change the following lines from:

<pre>FREETYPE_ROOT = None</pre>

to (note that I&#8217;m using the FreeType2 directory discovered previously with the locate command):

<pre>FREETYPE_ROOT = libinclude("/Developer/SDKs/MacOSX10.5.sdk/usr/X11/")</pre>

and then following line from:

<pre>JPEG_ROOT = None</pre>

to:

<pre>JPEG_ROOT = libinclude("/usr/local")</pre>

Locate these lines:`````<img class="alignnone" title="Python" src="http://www.alfersoft.com.ar/files/python.jpg" alt="" width="300" height="225" />

If you have a Mac OS X Leopard on an Intel platform, and you are interested in installing mod\_python using your default apache setup and the python that comes with the Framework, even more if you want to give PyCAPTCHA a try together with mod\_python, then check out this post! I&#8217;m covering also the installation of required libraries for PyCAPTCHA under Leopard (FreeType2, libjpeg, PIL)

<!--more-->

## **mod_python**

<span style="text-decoration: underline;">Compile mod_python</span>

If you have Apple&#8217;s Xcode installed, compiling mod_python for Leopard to use with the system provided versions of httpd, python, etc. should be as simple as running these commands:

`<br />
$ mkdir -p ~/mod_python<br />
$ cd ~/mod_python<br />
$ svn co http://svn.apache.org/repos/asf/quetzalcoatl/mod_python/trunk mod_python-trunk<br />
$ cd mod_python-trunk<br />
$ ./configure --with-apxs=/usr/sbin/apxs<br />
$ make<br />
$ sudo make install<br />
$ cd test/<br />
$ python test.py`

(original source: http://www.modpython.org/pipermail/mod_python/2008-March/025012.html)

<span style="text-decoration: underline;">Configure Apache to load mod_python</span>

Open /private/etc/apache2/httpd.conf with your favorite editor and add the following line:

``<img class="alignnone" title="Python" src="http://www.alfersoft.com.ar/files/python.jpg" alt="" width="300" height="225" />

If you have a Mac OS X Leopard on an Intel platform, and you are interested in installing mod\_python using your default apache setup and the python that comes with the Framework, even more if you want to give PyCAPTCHA a try together with mod\_python, then check out this post! I&#8217;m covering also the installation of required libraries for PyCAPTCHA under Leopard (FreeType2, libjpeg, PIL)

<!--more-->

## **mod_python**

<span style="text-decoration: underline;">Compile mod_python</span>

If you have Apple&#8217;s Xcode installed, compiling mod_python for Leopard to use with the system provided versions of httpd, python, etc. should be as simple as running these commands:

`<br />
$ mkdir -p ~/mod_python<br />
$ cd ~/mod_python<br />
$ svn co http://svn.apache.org/repos/asf/quetzalcoatl/mod_python/trunk mod_python-trunk<br />
$ cd mod_python-trunk<br />
$ ./configure --with-apxs=/usr/sbin/apxs<br />
$ make<br />
$ sudo make install<br />
$ cd test/<br />
$ python test.py`

(original source: http://www.modpython.org/pipermail/mod_python/2008-March/025012.html)

<span style="text-decoration: underline;">Configure Apache to load mod_python</span>

Open /private/etc/apache2/httpd.conf with your favorite editor and add the following line:

`` 

<span style="text-decoration: underline;">Example of how to configure Apache to load a mod_python application</span>

This is just an example, I recommend reading the <a title="mod_python manual" href="http://www.modpython.org/live/current/doc-html/" target="_blank">mod_python manual</a> for more information.

Open /private/etc/apache2/users/_username_.conf with your favorite editor and add:

<pre>Alias / "/Users/<em>username</em>/<em>htdocs</em>/"
&lt;Directory "/Users/<em>username</em>/<em>htdocs</em>/"&gt;
    SetHandler python-program
    PythonHandler mod_python.publisher
    # Disable following line when in production
    PythonDebug On
    &lt;Files ~ "\.(gif|jpg|png|ico|js|css)$"&gt;
        SetHandler default-handler
    &lt;/Files&gt;
    Order allow,deny
    Allow from all
&lt;/Directory&gt;</pre>

<span style="text-decoration: underline;">Restart Apache with the command below:</span>

<pre>$ /usr/sbin/apachectl restart</pre>



## PyCAPTCHA

In order to build and install PyCAPTCHA you&#8217;ll need to compile, or make sure that you have these required libraries:

  * FreeType2
  * libjpeg
  * PIL (Python Imaging Library)

Let&#8217;s see how to set up these libraries under a Leopard / Intel environment.
  


## FreeType2

This library is required for PyCAPTCHA, as far as I know it is already installed with the SDK, to check the directory where the library is installed use the command:

`$ locate libfreetype.dylib`

A version of this library should appear under /Developer/SDKs/MacOSX10.5.sdk/usr/X11/
  


## libjpeg

Download <a title="libjpeg" href="http://www.ijg.org/" target="_blank">jpeg source</a> and uncompress it in any directory, e.g. ~/jpeg-6b then:

```<img class="alignnone" title="Python" src="http://www.alfersoft.com.ar/files/python.jpg" alt="" width="300" height="225" />

If you have a Mac OS X Leopard on an Intel platform, and you are interested in installing mod\_python using your default apache setup and the python that comes with the Framework, even more if you want to give PyCAPTCHA a try together with mod\_python, then check out this post! I&#8217;m covering also the installation of required libraries for PyCAPTCHA under Leopard (FreeType2, libjpeg, PIL)

<!--more-->

## **mod_python**

<span style="text-decoration: underline;">Compile mod_python</span>

If you have Apple&#8217;s Xcode installed, compiling mod_python for Leopard to use with the system provided versions of httpd, python, etc. should be as simple as running these commands:

`<br />
$ mkdir -p ~/mod_python<br />
$ cd ~/mod_python<br />
$ svn co http://svn.apache.org/repos/asf/quetzalcoatl/mod_python/trunk mod_python-trunk<br />
$ cd mod_python-trunk<br />
$ ./configure --with-apxs=/usr/sbin/apxs<br />
$ make<br />
$ sudo make install<br />
$ cd test/<br />
$ python test.py`

(original source: http://www.modpython.org/pipermail/mod_python/2008-March/025012.html)

<span style="text-decoration: underline;">Configure Apache to load mod_python</span>

Open /private/etc/apache2/httpd.conf with your favorite editor and add the following line:

``<img class="alignnone" title="Python" src="http://www.alfersoft.com.ar/files/python.jpg" alt="" width="300" height="225" />

If you have a Mac OS X Leopard on an Intel platform, and you are interested in installing mod\_python using your default apache setup and the python that comes with the Framework, even more if you want to give PyCAPTCHA a try together with mod\_python, then check out this post! I&#8217;m covering also the installation of required libraries for PyCAPTCHA under Leopard (FreeType2, libjpeg, PIL)

<!--more-->

## **mod_python**

<span style="text-decoration: underline;">Compile mod_python</span>

If you have Apple&#8217;s Xcode installed, compiling mod_python for Leopard to use with the system provided versions of httpd, python, etc. should be as simple as running these commands:

`<br />
$ mkdir -p ~/mod_python<br />
$ cd ~/mod_python<br />
$ svn co http://svn.apache.org/repos/asf/quetzalcoatl/mod_python/trunk mod_python-trunk<br />
$ cd mod_python-trunk<br />
$ ./configure --with-apxs=/usr/sbin/apxs<br />
$ make<br />
$ sudo make install<br />
$ cd test/<br />
$ python test.py`

(original source: http://www.modpython.org/pipermail/mod_python/2008-March/025012.html)

<span style="text-decoration: underline;">Configure Apache to load mod_python</span>

Open /private/etc/apache2/httpd.conf with your favorite editor and add the following line:

`` 

<span style="text-decoration: underline;">Example of how to configure Apache to load a mod_python application</span>

This is just an example, I recommend reading the <a title="mod_python manual" href="http://www.modpython.org/live/current/doc-html/" target="_blank">mod_python manual</a> for more information.

Open /private/etc/apache2/users/_username_.conf with your favorite editor and add:

<pre>Alias / "/Users/<em>username</em>/<em>htdocs</em>/"
&lt;Directory "/Users/<em>username</em>/<em>htdocs</em>/"&gt;
    SetHandler python-program
    PythonHandler mod_python.publisher
    # Disable following line when in production
    PythonDebug On
    &lt;Files ~ "\.(gif|jpg|png|ico|js|css)$"&gt;
        SetHandler default-handler
    &lt;/Files&gt;
    Order allow,deny
    Allow from all
&lt;/Directory&gt;</pre>

<span style="text-decoration: underline;">Restart Apache with the command below:</span>

<pre>$ /usr/sbin/apachectl restart</pre>



## PyCAPTCHA

In order to build and install PyCAPTCHA you&#8217;ll need to compile, or make sure that you have these required libraries:

  * FreeType2
  * libjpeg
  * PIL (Python Imaging Library)

Let&#8217;s see how to set up these libraries under a Leopard / Intel environment.
  


## FreeType2

This library is required for PyCAPTCHA, as far as I know it is already installed with the SDK, to check the directory where the library is installed use the command:

`$ locate libfreetype.dylib`

A version of this library should appear under /Developer/SDKs/MacOSX10.5.sdk/usr/X11/
  


## libjpeg

Download <a title="libjpeg" href="http://www.ijg.org/" target="_blank">jpeg source</a> and uncompress it in any directory, e.g. ~/jpeg-6b then:

``` 

Edit Makefile and change the following line from:

<pre>CFLAGS= -O2 -I$(srcdir)</pre>

to:

<pre>CFLAGS= -arch ppc -arch i386 -arch ppc64 -arch x86_64 -O2 -I$(srcdir)</pre>

Finally, build and install:
  
`<br />
$ make<br />
$ sudo make install-lib`
  


## PIL

Download <a title="PIL source" href="http://www.pythonware.com/products/pil/" target="_blank">PIL source</a> and uncompress it in any directory, e.g. ~/imaging-1.1.6 then:

````<img class="alignnone" title="Python" src="http://www.alfersoft.com.ar/files/python.jpg" alt="" width="300" height="225" />

If you have a Mac OS X Leopard on an Intel platform, and you are interested in installing mod\_python using your default apache setup and the python that comes with the Framework, even more if you want to give PyCAPTCHA a try together with mod\_python, then check out this post! I&#8217;m covering also the installation of required libraries for PyCAPTCHA under Leopard (FreeType2, libjpeg, PIL)

<!--more-->

## **mod_python**

<span style="text-decoration: underline;">Compile mod_python</span>

If you have Apple&#8217;s Xcode installed, compiling mod_python for Leopard to use with the system provided versions of httpd, python, etc. should be as simple as running these commands:

`<br />
$ mkdir -p ~/mod_python<br />
$ cd ~/mod_python<br />
$ svn co http://svn.apache.org/repos/asf/quetzalcoatl/mod_python/trunk mod_python-trunk<br />
$ cd mod_python-trunk<br />
$ ./configure --with-apxs=/usr/sbin/apxs<br />
$ make<br />
$ sudo make install<br />
$ cd test/<br />
$ python test.py`

(original source: http://www.modpython.org/pipermail/mod_python/2008-March/025012.html)

<span style="text-decoration: underline;">Configure Apache to load mod_python</span>

Open /private/etc/apache2/httpd.conf with your favorite editor and add the following line:

``<img class="alignnone" title="Python" src="http://www.alfersoft.com.ar/files/python.jpg" alt="" width="300" height="225" />

If you have a Mac OS X Leopard on an Intel platform, and you are interested in installing mod\_python using your default apache setup and the python that comes with the Framework, even more if you want to give PyCAPTCHA a try together with mod\_python, then check out this post! I&#8217;m covering also the installation of required libraries for PyCAPTCHA under Leopard (FreeType2, libjpeg, PIL)

<!--more-->

## **mod_python**

<span style="text-decoration: underline;">Compile mod_python</span>

If you have Apple&#8217;s Xcode installed, compiling mod_python for Leopard to use with the system provided versions of httpd, python, etc. should be as simple as running these commands:

`<br />
$ mkdir -p ~/mod_python<br />
$ cd ~/mod_python<br />
$ svn co http://svn.apache.org/repos/asf/quetzalcoatl/mod_python/trunk mod_python-trunk<br />
$ cd mod_python-trunk<br />
$ ./configure --with-apxs=/usr/sbin/apxs<br />
$ make<br />
$ sudo make install<br />
$ cd test/<br />
$ python test.py`

(original source: http://www.modpython.org/pipermail/mod_python/2008-March/025012.html)

<span style="text-decoration: underline;">Configure Apache to load mod_python</span>

Open /private/etc/apache2/httpd.conf with your favorite editor and add the following line:

`` 

<span style="text-decoration: underline;">Example of how to configure Apache to load a mod_python application</span>

This is just an example, I recommend reading the <a title="mod_python manual" href="http://www.modpython.org/live/current/doc-html/" target="_blank">mod_python manual</a> for more information.

Open /private/etc/apache2/users/_username_.conf with your favorite editor and add:

<pre>Alias / "/Users/<em>username</em>/<em>htdocs</em>/"
&lt;Directory "/Users/<em>username</em>/<em>htdocs</em>/"&gt;
    SetHandler python-program
    PythonHandler mod_python.publisher
    # Disable following line when in production
    PythonDebug On
    &lt;Files ~ "\.(gif|jpg|png|ico|js|css)$"&gt;
        SetHandler default-handler
    &lt;/Files&gt;
    Order allow,deny
    Allow from all
&lt;/Directory&gt;</pre>

<span style="text-decoration: underline;">Restart Apache with the command below:</span>

<pre>$ /usr/sbin/apachectl restart</pre>



## PyCAPTCHA

In order to build and install PyCAPTCHA you&#8217;ll need to compile, or make sure that you have these required libraries:

  * FreeType2
  * libjpeg
  * PIL (Python Imaging Library)

Let&#8217;s see how to set up these libraries under a Leopard / Intel environment.
  


## FreeType2

This library is required for PyCAPTCHA, as far as I know it is already installed with the SDK, to check the directory where the library is installed use the command:

`$ locate libfreetype.dylib`

A version of this library should appear under /Developer/SDKs/MacOSX10.5.sdk/usr/X11/
  


## libjpeg

Download <a title="libjpeg" href="http://www.ijg.org/" target="_blank">jpeg source</a> and uncompress it in any directory, e.g. ~/jpeg-6b then:

```<img class="alignnone" title="Python" src="http://www.alfersoft.com.ar/files/python.jpg" alt="" width="300" height="225" />

If you have a Mac OS X Leopard on an Intel platform, and you are interested in installing mod\_python using your default apache setup and the python that comes with the Framework, even more if you want to give PyCAPTCHA a try together with mod\_python, then check out this post! I&#8217;m covering also the installation of required libraries for PyCAPTCHA under Leopard (FreeType2, libjpeg, PIL)

<!--more-->

## **mod_python**

<span style="text-decoration: underline;">Compile mod_python</span>

If you have Apple&#8217;s Xcode installed, compiling mod_python for Leopard to use with the system provided versions of httpd, python, etc. should be as simple as running these commands:

`<br />
$ mkdir -p ~/mod_python<br />
$ cd ~/mod_python<br />
$ svn co http://svn.apache.org/repos/asf/quetzalcoatl/mod_python/trunk mod_python-trunk<br />
$ cd mod_python-trunk<br />
$ ./configure --with-apxs=/usr/sbin/apxs<br />
$ make<br />
$ sudo make install<br />
$ cd test/<br />
$ python test.py`

(original source: http://www.modpython.org/pipermail/mod_python/2008-March/025012.html)

<span style="text-decoration: underline;">Configure Apache to load mod_python</span>

Open /private/etc/apache2/httpd.conf with your favorite editor and add the following line:

``<img class="alignnone" title="Python" src="http://www.alfersoft.com.ar/files/python.jpg" alt="" width="300" height="225" />

If you have a Mac OS X Leopard on an Intel platform, and you are interested in installing mod\_python using your default apache setup and the python that comes with the Framework, even more if you want to give PyCAPTCHA a try together with mod\_python, then check out this post! I&#8217;m covering also the installation of required libraries for PyCAPTCHA under Leopard (FreeType2, libjpeg, PIL)

<!--more-->

## **mod_python**

<span style="text-decoration: underline;">Compile mod_python</span>

If you have Apple&#8217;s Xcode installed, compiling mod_python for Leopard to use with the system provided versions of httpd, python, etc. should be as simple as running these commands:

`<br />
$ mkdir -p ~/mod_python<br />
$ cd ~/mod_python<br />
$ svn co http://svn.apache.org/repos/asf/quetzalcoatl/mod_python/trunk mod_python-trunk<br />
$ cd mod_python-trunk<br />
$ ./configure --with-apxs=/usr/sbin/apxs<br />
$ make<br />
$ sudo make install<br />
$ cd test/<br />
$ python test.py`

(original source: http://www.modpython.org/pipermail/mod_python/2008-March/025012.html)

<span style="text-decoration: underline;">Configure Apache to load mod_python</span>

Open /private/etc/apache2/httpd.conf with your favorite editor and add the following line:

`` 

<span style="text-decoration: underline;">Example of how to configure Apache to load a mod_python application</span>

This is just an example, I recommend reading the <a title="mod_python manual" href="http://www.modpython.org/live/current/doc-html/" target="_blank">mod_python manual</a> for more information.

Open /private/etc/apache2/users/_username_.conf with your favorite editor and add:

<pre>Alias / "/Users/<em>username</em>/<em>htdocs</em>/"
&lt;Directory "/Users/<em>username</em>/<em>htdocs</em>/"&gt;
    SetHandler python-program
    PythonHandler mod_python.publisher
    # Disable following line when in production
    PythonDebug On
    &lt;Files ~ "\.(gif|jpg|png|ico|js|css)$"&gt;
        SetHandler default-handler
    &lt;/Files&gt;
    Order allow,deny
    Allow from all
&lt;/Directory&gt;</pre>

<span style="text-decoration: underline;">Restart Apache with the command below:</span>

<pre>$ /usr/sbin/apachectl restart</pre>



## PyCAPTCHA

In order to build and install PyCAPTCHA you&#8217;ll need to compile, or make sure that you have these required libraries:

  * FreeType2
  * libjpeg
  * PIL (Python Imaging Library)

Let&#8217;s see how to set up these libraries under a Leopard / Intel environment.
  


## FreeType2

This library is required for PyCAPTCHA, as far as I know it is already installed with the SDK, to check the directory where the library is installed use the command:

`$ locate libfreetype.dylib`

A version of this library should appear under /Developer/SDKs/MacOSX10.5.sdk/usr/X11/
  


## libjpeg

Download <a title="libjpeg" href="http://www.ijg.org/" target="_blank">jpeg source</a> and uncompress it in any directory, e.g. ~/jpeg-6b then:

``` 

Edit Makefile and change the following line from:

<pre>CFLAGS= -O2 -I$(srcdir)</pre>

to:

<pre>CFLAGS= -arch ppc -arch i386 -arch ppc64 -arch x86_64 -O2 -I$(srcdir)</pre>

Finally, build and install:
  
`<br />
$ make<br />
$ sudo make install-lib`
  


## PIL

Download <a title="PIL source" href="http://www.pythonware.com/products/pil/" target="_blank">PIL source</a> and uncompress it in any directory, e.g. ~/imaging-1.1.6 then:

```` 

Edit setup.py and change the following lines from:

<pre>FREETYPE_ROOT = None</pre>

to (note that I&#8217;m using the FreeType2 directory discovered previously with the locate command):

<pre>FREETYPE_ROOT = libinclude("/Developer/SDKs/MacOSX10.5.sdk/usr/X11/")</pre>

and then following line from:

<pre>JPEG_ROOT = None</pre>

to:

<pre>JPEG_ROOT = libinclude("/usr/local")</pre>

Locate these lines:````` 
  
And add the following lines (correctly indented from column 0):

<pre>OrigExtension = Extension
def Extension(*args, **kwargs):
    extra_args = ['-arch', 'ppc', '-arch', 'ppc64', '-arch', 'i386', '-arch', 'x86_64']
    kwargs['extra_compile_args'] = extra_args + kwargs.get('extra_compile_args', [])
    kwargs['extra_link_args'] = extra_args + kwargs.get('extra_link_args', [])
    return OrigExtension(*args, **kwargs)</pre>

Now proceed to compile and install the library:
  
`<br />
$ sudo python setup.py install`
  


## Install PyCAPTCHA

Now you are ready to build and install PyCAPTCHA. Checkout the source code from <a title="PyCAPTCHA" href="http://svn.navi.cx/misc/trunk/pycaptcha/" target="_blank">this link</a> and run the following command:
  
`<br />
$ sudo python setup.py install`

That&#8217;s it!

###### <a title="Python" href="http://commons.wikimedia.org/wiki/File:Boomslang_(443113819).jpg" target="_blank">image source</a>
