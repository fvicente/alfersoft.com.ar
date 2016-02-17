---
id: 681
title: Arcade Joystick with Teensy 3.0
date: 2014-02-23T23:14:50+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=681
permalink: /2014/02/23/arcade-joystick-with-teensy-3-0/
categories:
  - Electronic Projects
tags:
  - arcade
  - joystick
  - keyboard
  - MAME
  - teensy
  - USB
comments: true
---
Do you have a spare Teensy 3.0, like I did, and you want to (under) use it? this might be a good one-afternoon DIY project: an Arcade Joystick.
  
Besides the ARM powered board, you will need a 4-switch arcade joystick, buttons, and a box that you need to still from your wife.

The firmware emulates an USB keyboard, so all you have to do is connect the switches from the stick and the buttons to the teensy digital inputs. No other external components are needed. For convenience, I&#8217;ve used inputs 0 to 7. I&#8217;m using only 4 push buttons, but you can easily extend it if you want, just adjust the `NUM_BUTTONS` and the `keys` array accordingly.

Joystick switches emulates the arrow keys, and the buttons, well, you need to program them to whatever fits better for your games, in my case [Enter], [Space], [Left Alt] and [Z] did the trick for Super Frog HD. I should think in putting an internal switch to change the button&#8217;s configuration to something more standard like the MAME layout.

Just for reference, this is the list of materials I&#8217;ve used for this project:
  
[table id=1 /]

**The Code**

Of course, I am using the existing Teensy USB keyboard libraries, so the code is pretty simple &#8211; 32 lines of code.
  
[See joystick.ino on gist](https://gist.github.com/fvicente/515d08aabf5616f710cd)

<div class="oembed-gist">
  <noscript>
    View the code on <a href="https://gist.github.com/fvicente/515d08aabf5616f710cd">Gist</a>.
  </noscript>
</div>

**Connection Diagram**

No explanation needed<figure id="attachment_700" style="width: 300px" class="wp-caption aligncenter">

[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/joystick-300x300.png" alt="Joystick Connection Diagram" width="300" height="300" class="size-medium wp-image-700" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/joystick-150x150.png 150w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/joystick-300x300.png 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/joystick-700x700.png 700w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/joystick-332x332.png 332w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/joystick-432x432.png 432w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/joystick-268x268.png 268w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/joystick.png 768w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/joystick.png)<figcaption class="wp-caption-text">Joystick Connection Diagram</figcaption></figure> 

**Pictures**<figure id="attachment_692" style="width: 300px" class="wp-caption aligncenter">

[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2559-300x225.jpg" alt="Joystick connections" width="300" height="225" class="size-medium wp-image-692" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2559-300x225.jpg 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2559-700x525.jpg 700w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2559-332x249.jpg 332w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2559.jpg 1024w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2559.jpg)<figcaption class="wp-caption-text">Joystick connections</figcaption></figure> <figure id="attachment_693" style="width: 300px" class="wp-caption aligncenter">[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2562-300x225.jpg" alt="Box holes" width="300" height="225" class="size-medium wp-image-693" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2562-300x225.jpg 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2562-700x525.jpg 700w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2562-332x249.jpg 332w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2562.jpg 1024w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2562.jpg)<figcaption class="wp-caption-text">Box holes</figcaption></figure> <figure id="attachment_697" style="width: 300px" class="wp-caption aligncenter">[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2583-300x225.jpg" alt="Inside Joystick" width="300" height="225" class="size-medium wp-image-697" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2583-300x225.jpg 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2583-700x525.jpg 700w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2583-332x249.jpg 332w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2583.jpg 1024w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2583.jpg)<figcaption class="wp-caption-text">Inside Joystick</figcaption></figure> <figure id="attachment_695" style="width: 300px" class="wp-caption aligncenter">[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2579-300x225.jpg" alt="Inside Joystick" width="300" height="225" class="size-medium wp-image-695" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2579-300x225.jpg 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2579-700x525.jpg 700w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2579-332x249.jpg 332w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2579.jpg 1024w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2579.jpg)<figcaption class="wp-caption-text">Inside Joystick</figcaption></figure> <figure id="attachment_696" style="width: 300px" class="wp-caption aligncenter">[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2581-300x225.jpg" alt="Joystick" width="300" height="225" class="size-medium wp-image-696" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2581-300x225.jpg 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2581-700x525.jpg 700w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2581-332x249.jpg 332w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2581.jpg 1024w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2581.jpg)<figcaption class="wp-caption-text">Joystick</figcaption></figure> <figure id="attachment_694" style="width: 300px" class="wp-caption aligncenter">[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2566-300x225.jpg" alt="Joystick testing with uncle Alf" width="300" height="225" class="size-medium wp-image-694" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2566-300x225.jpg 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2566-700x525.jpg 700w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2566-332x249.jpg 332w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2566.jpg 1024w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2014/02/IMG_2566.jpg)<figcaption class="wp-caption-text">Joystick testing with uncle Alf</figcaption></figure>
