---
uid: 740
title: '3D Printer &#8211; Z Axis Calibration'
date: 2015-05-25T14:57:22+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=740
permalink: /2015/05/25/3d-printer-z-axis-calibration/
categories:
  - 3D Printer
  - Electronic Projects
tags:
  - 3d printer
  - axis
  - calibration
  - firmware
  - marlin
  - scale
  - shrink
comments: true
---
[<img src="{{ site.url }}/images/3d_printer_z_axis.jpg" alt="Z Axis Calibration"/>]({{ site.url }}/images/3d_printer_z_axis.jpg)

If your printed models are out of scale, in any axis, like it happened to me with the Z axis -you can clearly see in the picture that the hole looks like an oval instead of a circle- you might need to calibrate your firmware stepping parameter.

In the Marlin firmware there is a define called DEFAULT\_AXIS\_STEPS\_PER\_UNIT containing four factors corresponding to X, Y, Z axis and E (extrusion). Re-calculate the new steps using the following formula:

new\_steps = (expected mm / actual mm) * old\_steps

Compile and upload the firmware to the 3D printer.

More info:

[Quick guide to calibrating the scale of your 3D printer](http://reprage.com/post/46062359808/quick-guide-to-calibrating-the-scale-of-your-3d-printer/)
