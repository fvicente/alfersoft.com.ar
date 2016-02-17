---
id: 108
title: (HTML) Table cell not inheriting style
date: 2009-06-04T21:24:27+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=108
permalink: /2009/06/04/html-table-cell-not-inheriting-style/
categories:
  - Programming FAQ
comments: true
---
The goal of this new blog category called &#8220;Programming FAQ&#8221; is to publish short posts with day to day programming problems and their solutions, or at least provide a tip to find the proper solution to your problem. And today we start with this simple and stupid problem that can happen to anyone with poor HTML experience like me. &#8220;My HTML table does not seems to inherit the style from a parent element&#8221;, here is the example:
  
<!--more-->

<pre>&lt;html&gt;
&lt;body&gt;
&lt;div style="color: blue;"&gt;
	&lt;table&gt;
	&lt;tbody&gt;
		&lt;tr&gt;
			&lt;td&gt;BLUE TEXT&lt;/td&gt;
		&lt;/tr&gt;
	&lt;/tbody&gt;
	&lt;/table&gt;
&lt;div&gt;
&lt;/body&gt;
&lt;/html&gt;</pre>

In the below example I would expect to see the &#8220;BLUE TEXT&#8221; in blue, but is not (at least in FireFox). So what the hell is wrong with my code? probably many things but because I&#8217;m not interested in following/reading the W3C recommendations for my uncle&#8217;s home page, here is one possible solution: Add the transitional DOCTYPE to your document, or the DOCTYPE that adequate better to your page (<a title="W3C DOCTYPE" href="http://www.w3.org/QA/2002/04/valid-dtd-list.html" target="_blank">read more here</a>). E.g.:

<pre>&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"&gt;</pre>

So, now you know. Never forget the DOCTYPE it is very important&#8230; Why? <a title="Don't forget to add a doctype" href="http://www.w3.org/QA/Tips/Doctype" target="_blank">read this</a>&#8230; It has to do with poor standards, browsers and all that crap.
