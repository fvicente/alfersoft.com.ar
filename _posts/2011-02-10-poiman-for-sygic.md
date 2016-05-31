---
uid: 168
title: POIMan for Sygic
date: 2011-02-10T12:57:59+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=168
categories:
  - Android
tags:
  - android
  - pdi
  - poi
  - poiman
  - sygic
  - update
comments: true
excerpt: An Android application that lets you keep your Sygic's Point of Interests (POIs) up to date and manage them (add / remove) in an easy way
---
[<img src="{{ site.baseurl }}/images/poiman-256.png" alt="" title="POIMan for Sygic" />]({{ site.baseurl }}/images/poiman-256.png)

POIMan for Sygic is an Android application that lets you keep your Sygic&#8217;s Point of Interests (POIs) up to date and manage them (add / remove) in an easy way.

This application will download the POI&#8217;s list from a definable URL (by default <http://www.todo-poi.es/TodoPOI.xml>) and let the user select and download the available POIs from that list. Since the POIs are in TomTom format (\*.ov2), POIMan will convert them to the Sygic format (\*.upi). Also the BMPs are downloaded and resized to a suitable size that Sygic displays properly.

<a title="Download POIMan for Sygic" markdown="0" href="https://github.com/fvicente/poiman-for-sygic/archive/master.zip" class="btn">Download Source Code</a>

[POIMan for Sygic on GitHub](https://github.com/fvicente/poiman-for-sygic "POIMan for Sygic on GitHub"){:target="_blank"}

## Screenshots

<figure class="half">
	<img src="{{ site.baseurl }}/images/poiman-ss01.png" alt="Screenshot #1"/>
	<img src="{{ site.baseurl }}/images/poiman-ss02.png" alt="Screenshot #2"/>
</figure>
<figure class="half">
	<img src="{{ site.baseurl }}/images/poiman-ss03.png" alt="Screenshot #3"/>
	<img src="{{ site.baseurl }}/images/poiman-ss04.png" alt="Screenshot #4"/>
</figure>

<!--more-->

## About the UPI format (Sygic POIs)

It was pretty difficult to find information on the Internet about Sygic&#8217;s Mobile Maps (SMM) POIs file format (\*.upi). Different from the TomTom OV2 format which is well documented and easy to find (see <http://www.tomtom.com/lib/doc/ttnavsdk3_manual.pdf> and <http://www.opentom.org/Ov2>).

One of the goals of POIMan for Sygic was converting the OV2 to UPI, in order to get the most popular POIs to work in SMM. So, the only way to do it without documentation was comparing an OV2 binary to its already converted UPI (converted using another existing tool &#8212; most people uses PoiEdit <http://www.poiedit.com/>) and try to discover what the differences are.

### Differences between OV2 and UPI

* All the strings in the UPI files seems to be stored as inverted wide chars. By inverted I mean that each character occupies 16 bits, but the byte order is swapped (I guess is like a little-endian wide char string). And all the strings are null-terminated (two null bytes).
* There is a header in the UPI file. Contrarily to OV2 that starts directly with records, the UPI has a header and the format is the following:
	* First byte is the length of the POI file name (without extension) that will follow this byte including the null character (remember: everything multiplied by 2 since it is wide char)
	* A string with the name of the POI file without extension
	* Something that seems to be a filler record, composed by one byte 0x02 followed by 9 null bytes (0x00)
	* A byte with the length of the BMP file name that will follow this byte (including null character)
	* A string with the BMP file name including the extension
* The simple POI record in OV2 is the type 0x02 while in the UPI format is type 0x03
* The skipper record (type 0x01 in both formats) seem pretty much the same in both files, they are &#8216;rectangles&#8217; composed by four coordinates (lat1, lon1) (lat2, lon2). However in the OV2 you can find skipper records following other skipper records, like if you subdivide the rectangles into smaller groups where the POIs that will follow belongs, but this does not works well when you convert them to UPI. So, in other words the POIs should be contained in only one rectangle, so the simples solutions was to use the outer rectangle and skip the rest.

## List of tested XML&#8217;s

In the preferences page, you can configure the URL that will be used to download the POIs list. The URL usually points to an XML file, which schema is pretty much standard. This XML contains the links to the OV2 files (TomTom POIs), BMPs, map and group to which each POI belongs, etc. However, I&#8217;ve found differences regarding the contents of the tags, for example: sometimes the URL to the BMPs are relative to the URL list, sometimes it is the full path; also I&#8217;ve found that some pages may require a user name and password to download the OV2 so in the URL you&#8217;ll find the %Username% and %Password% variables that POIMan will replace with whatever the user inputs in its preferences page.

In theory, you should be able to use any XML of this page <http://www.poiedit.com/sites.htm> however, due to these differences I&#8217;ve found, it might not work correctly, let me know if you find any problem.

### Tested XML&#8217;s

This is the list of XML&#8217;s already tested. If you have more to add please let me know.

* <http://www.todo-poi.es/TodoPOI.xml> (Spain)
* <http://www.flitspaal.nl/poi_flitspalen.xml> (Netherlands) &#8211; This one requires to full-fill user name and password. The user name is the e-mail address used to register in the [www.flitspaal.nl](http://www.flitspaal.nl) site.
