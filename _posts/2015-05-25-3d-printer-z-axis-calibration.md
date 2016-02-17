---
id: 740
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
[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2015/05/20150516_134258-300x169.jpg" alt="Z Axis Calibration" width="300" height="169" class="alignleft size-medium wp-image-741" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2015/05/20150516_134258-300x169.jpg 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2015/05/20150516_134258-1024x576.jpg 1024w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2015/05/20150516_134258-700x393.jpg 700w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2015/05/20150516_134258-332x187.jpg 332w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2015/05/20150516_134258.jpg)
  
If your printed models are out of scale, in any axis, like it happened to me with the Z axis -you can clearly see in the picture that the hole looks like an oval instead of a circle- you might need to calibrate your firmware stepping parameter.
  
In the Marlin firmware there is a define called DEFAULT\_AXIS\_STEPS\_PER\_UNIT containing four factors corresponding to X, Y, Z axis and E (extrusion). Re-calculate the new steps using the following formula:

new\_steps = (expected mm / actual mm) * old\_steps

Compile and upload the firmware to the 3D printer.

More info:
  
http://reprage.com/post/46062359808/quick-guide-to-calibrating-the-scale-of-your-3d-printer/
