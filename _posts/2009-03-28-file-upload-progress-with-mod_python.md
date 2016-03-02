---
uid: 97
title: File Upload Progress with mod_python
date: 2009-03-28T02:07:42+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=97
permalink: /2009/03/28/file-upload-progress-with-mod_python/
categories:
  - Software Development
tags:
  - ajax
  - files
  - iframe
  - javascript
  - mod_python
  - progress
  - progress bar
  - python
  - upload
comments: true
---
<figure>
	<img title="Upload Progress with mod_python" src="{{ site.baseurl }}/images/uprogress.jpg" alt="Upload Progress with mod_python" />
	<figcaption class="wp-caption-text">Upload Progress with mod_python</figcaption>
</figure>

Now that Gmail has added a progress bar for attachment uploads, everybody wants to do the same including myself  😀 

So, with the same method that everybody uses consisting in an upload form with a hidden <iframe> as target, and then a periodic Ajax request to update the progress, I made a simple implementation using mod_python on the server side&#8230; This is a veeery simple example, like everything that you do with Python.

Additionally I&#8217;ve added an upload size limit control based in examples that can be found in the mod_python forum site. Use the source Luke!

See <a title="Upload Progress Demo" href="http://www.alfersoft.com.ar/uprogress/" target="_blank">LIVE DEMO!</a>

<!--more-->

## Libraries that I used for this example

The fancy JavaScript progress bar is from <a title="Bramus!" href="http://www.bram.us/" target="_blank">Bramus</a>, I just changed the default options, renamed the main script file to progressbar.js and the images just because I like everything in lower case. See the copyright inside the file or visit the site for more information.

<a title="Prototype.js" href="http://www.prototypejs.org/" target="_blank">Prototype.js</a>: this well known library to avoid cross-browser compatibility issues, is small and practical. I use it in almost all my web projects. Also Bramus requires this library. If you don&#8217;t like it for some reason you will need to replace many many many functions like $() which is an improved getElementById(), or the Hash class that I use to track in-progress uploads, JSON encoding, etc. etc. You don&#8217;t want to remove it, seriously.

<a title="simplejson" href="http://code.google.com/p/simplejson/" target="_blank">simplejson</a>: A python library to encode/decode JSON objects. I use this one for the messaging between the server and the page. In my opinion JSON is more efficient than XML because it generates more compact and still human-readable outputs. But you can use XML or any other method by modifying the example as you like.

## Download example

Start by downloading the <a title="Source code for uprogress example" href="https://github.com/fvicente/uprogress/archive/master.zip" target="_blank">source code for this example</a>. I decided to release the code under BSD license, so you can use it for whatever you want.

The source is organized like the following illustration:

<figure>
	<img title="Upload Progress source directory structure" src="{{ site.baseurl }}/images/uprogress_src.jpg" alt="Upload Progress source directory structure" />
	<figcaption>Upload Progress source directory structure</figcaption>
</figure> 

**/src/img** contains images for JavaScript Bramus progress bar

**/src/js** contains JavaScript libraries Prototype.js and Bramus progress bar that I renamed to progressbar.js

**/src/temp** directory where the uploaded files will be stored. IMPORTANT: in unix like environments the group and permission of this folder must be configured properly in order to Apache to have write access on it.

**/src/\_upload\_limit.py** mod_python fixup handler that controls the file upload size limit, verifies if the file already exist in the server, stores file size and location information on the session and does the actual file upload after all the verifications.

**/src/index.py** The main python publisher handler. Exposed URLs are the main / &#8220;index&#8221; page that loads the test.html and /get\_upload\_progress function that returns the current % progress of the files being uploaded.

**/src/test.html** HTML page that contains all the JavaScript code used to create the upload forms, post the files, track the progress via Ajax, update the progress bar status, etc&#8230; This file is parsed by index.py as a PSP but does not contains any embedded python code currently.

**/lib/simplejson** simplejson python library to encode / decode JSON messages.

## Configuring Apache

I&#8217;m assuming that you already have mod_python installed on your Apache. Take a look to the sample configuration:

{% highlight apache %}
Alias / "/Users/fvicente/workspace/uprogress/src/"
<Directory "/Users/fvicente/workspace/uprogress/src/">
    SetHandler python-program
    PythonHandler mod_python.publisher
    PythonDebug On
    PythonFixupHandler _upload_limit
    <Files ~ "\.(gif|jpg|png|ico|js|css)$">
        SetHandler default-handler
    </Files>
    Order allow,deny
    Allow from all
</Directory>
{% endhighlight %}

mod\_python manual has a lot of information about the configuration options, so I&#8217;m not going to describe this in too much depth, just remark that I&#8217;m using the very useful mod\_python publisher, and more &#8220;unusual&#8221; option PythonFixupHandler pointing to the \_upload\_limit.py file.

## Interesting parts of the code

**/_upload/_limit.py**

The fixuphandler function contained in this file does the checking for the size limit of the file being uploaded by reading the http header Content-Length sent by the browser allowing you to reject an upload even before starting the actual transfer&#8230; very clever!. I found \_upload\_limit.py <a title="mod_python forum example" href="http://www.modpython.org/pipermail/mod_python/2007-February/023158.html" target="_blank">example in the mod_python forum</a>, and used it as a based adding the specific code for the upload progress handling. \_upload\_limit.py also stores the file size and name in the current session to be used later by the get\_upload\_progress function defined in index.py. Note that every time that the Session is used it will be accessed with the option lock=False thus using the session.lock() and session.unlock() methods manually when necessary, if you don&#8217;t do this that way all the parallel requests will be held until the upload finishes making impossible to monitor the progress via parallel Ajax request.

**index.py**

The exposed function get\_upload\_progress calculates the current upload percent by making a file-stat to see how much of the file has been stored so far. It is requested by the page test.html via Ajax in intervals of one seconds while the file is being uploaded. The expected parameter is a dictionary where the keys are &#8220;upload slots&#8221; that the JavaScript generates and understand, and the values are the name of the files to get the upload percent.

**test.html**

This one is 90% JavaScript code. But is not so bad, only 100 lines of code. In a very brief description you will see 4 functions:

**addUploadSlot()** creates an HTML code dynamically with an &#8220;upload slot&#8221; which is a form with the file to upload, the hidden <iframe> that is the form target, the progress bar control, and the &#8220;Upload&#8221; button. It uses a global counter called slotCnt to give a unique id to each slot. This id will be the &#8220;key&#8221; in the get\_upload\_progress dictionary (see index.py description).

**sendFile(slot)** shows the progress bar for given slot, adds the file into a global Hash() table called &#8220;uploading&#8221;, starts the periodic Ajax progress monitor (I used JavaScript function setInterval() to do the periodic Ajax) and sends the form to the server. Finally it will disable the controls for that slot and add a new slot dynamically for other uploads.

**updateProgressBars()** This is the function called by setInterval() while the files are uploading that does the actual Ajax.Request to /get\_upload\_progress in order to get the % progress for each file. The parameter sent with the request is a dictionary where the keys are the slots Id&#8217;s and the values the file names stored in the hash table.

**uploadDone(slot)** Will be called by the <iframe> when the upload finishes (successfully or not). Here I remove the file from the hash and stop the interval if no longer necessary.

## Final comments

Remember that this is just an example and may contain errors. Use it at your own risk, I&#8217;m not responsable of any possible damage or data lost, blah, blah, etc. etc. etc.

The code is poorly commented, but at least is well formatted :D. I recommend you to read it, it is very short and you may find interesting chunks.

<a title="Download uprogress" markdown="0" href="https://github.com/fvicente/uprogress/archive/master.zip" class="btn">Download Source Code</a>

[uprogress on GitHub](https://github.com/fvicente/uprogress "uprogress on GitHub"){:target="_blank"}

Enjoy it!
