---
id: 350
title: 'jQuery Lightbox: Adjust big images to the window size'
date: 2011-12-09T09:21:27+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=350
permalink: /2011/12/09/jquery-lightbox-adjust-big-images-to-the-window-size/
categories:
  - Software Development
tags:
  - customization
  - image size
  - javascript
  - jquery
  - lightbox
  - limit size
  - photo size
  - plugin
comments: true
---
[<img src="{{ site.url }}/images/jquery-lightbox-alfersoft-mod.png" alt="" title="jQuery lightBox AlferSoft Mod"/>]({{ site.url }}/images/jquery-lightbox-alfersoft-mod.png)

<a href="http://leandrovieira.com/projects/jquery/lightbox/" title="jQuery lightBox plugin" target="_blank">jQuery lightBox</a> is a great plugin inspired in the well-known <a href="http://www.huddletogether.com/projects/lightbox2/" title="LightBox JS" target="_blank">Lightbox JS</a> by Lokesh Dhakar.

After starting using it I realized that things became messy with big images, so I made a small modification to limit the image size to the current browser window size.

Also I added an option to hide the image information. The images used to navigate between images and close the preview are in English, so I modified the images to be more neutral (just icons)

<!--more-->

## Code

Two new options were added:

{% highlight javascript %}
fitToWindow: true,		// option to fit preview to window size if the
                                // image is too big
showInfo:    true		// option to show or hide image info
{% endhighlight %}

To fit the preview to the window size, I&#8217;ve replaced the call to \_resize\_container\_image\_box() with \_adjust\_image_size().

Here is the definition of the new function:

{% highlight javascript %}
function _adjust_image_size(objImagePreloader) {
    // get image size
    var imgWidth = objImagePreloader.width;
    var imgHeight = objImagePreloader.height;

    var arrayPageSize = ___getPageSize();
    // calculate proportions
    var imageProportion = imgWidth / imgHeight;
    var winProportion = arrayPageSize[2] / arrayPageSize[3];
    if (imageProportion > winProportion) {
        // calculate max width base on page width
        var maxWidth = arrayPageSize[2] - (settings.containerBorderSize * 2) - (arrayPageSize[2] / 10);
        var maxHeight = Math.round(maxWidth / imageProportion);
    } else {
        // calculate max height base on page height
        var maxHeight = arrayPageSize[3] - (settings.containerBorderSize * 2) - (arrayPageSize[3] / 10) - 40;
        var maxWidth = Math.round(maxHeight * imageProportion);
    }
    if (imgWidth > maxWidth || imgHeight > maxHeight) {
        imgWidth = maxWidth;
        imgHeight = maxHeight;
    }
    $('#lightbox-image').attr('width', imgWidth);
    $('#lightbox-image').attr('height', imgHeight);
    _resize_container_image_box(imgWidth, imgHeight);
};
{% endhighlight %}

Finally, to optionally hide or show the image info, just canged the following line:

{% highlight javascript %}
if ( settings.imageArray.length > 1 ) {
{% endhighlight %}

to:

{% highlight javascript %}
if ( settings.showInfo && (settings.imageArray.length > 1) ) {
{% endhighlight %}

<a title="Download jQuery lightBox 0.5 AlferSoft Mod" markdown="0" href="{{ site.url }}/files/jquery-lightbox-0.5-alfersoft-mod.zip" class="btn">Download jQuery lightBox 0.5 AlferSoft Mod</a>
