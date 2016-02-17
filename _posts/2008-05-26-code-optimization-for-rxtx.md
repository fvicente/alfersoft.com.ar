---
id: 6
title: Code optimization for RXTX parallel port implementation
date: 2008-05-26T23:02:13+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=6
permalink: /2008/05/26/code-optimization-for-rxtx/
categories:
  - Software Development
tags:
  - code
  - optimization
  - parallel
  - port
  - rxtx
comments: true
---
<img src="http://www.alfersoft.com.ar/files/db25.jpg" alt="DB-25 Connector" width="300" height="225" />

<a title="RXTX" href="http://www.rxtx.org/" target="_blank">RXTX</a> is a native lib providing serial and parallel communication for the Java Development Toolkit (JDK). All deliverables are under the gnu LGPL license. The native part is developed in C.

If you are using the parallel port functions of this project for version rxtx-2.1-7r2 or older, you may be interested in this code optimization, it is a small change but it may improve the resource usage in certain cases.

<!--more-->

Let&#8217;s take a look to readArray function in ParallelImp.c, this is a summary of the current implementation:

<pre class="brush: cpp; title: ; notranslate" title="">buffer = (unsigned char *)malloc( sizeof( unsigned char ) * length );
bytes = read_byte_array( fd, buffer, length, threshold, timeout );
for( i = 0; i &lt; bytes; i++ ) body[ i + offset ] = buffer[ i ];
free( buffer );
</pre>

In C we can increment the output buffer pointer to the position we want to start to write (offset) directly, avoiding the necessity of allocating a new buffer and copying the contents, for example:

<pre class="brush: cpp; title: ; notranslate" title="">//buffer = (unsigned char *)malloc( sizeof( unsigned char ) * length );
buffer = body+offset
bytes = read_byte_array( fd, buffer, length, threshold, timeout );
//for( i = 0; i &lt; bytes; i++ ) body[ i + offset ] = buffer[ i ];
//free( buffer );
</pre>

This kind optimization can be also applied to the writeArray function in the same source file.

I&#8217;m going to send this information to the RXTX project mailing list, hopefully it will be available in next version!

###### <a title="db25 picture" href="http://commons.wikimedia.org/wiki/Image:Scsi_extern_db25_st.jpg" target="_blank">picture source</a>
