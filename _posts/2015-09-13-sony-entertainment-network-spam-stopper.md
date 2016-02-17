---
id: 770
title: Sony Entertainment Network spam stopper
date: 2015-09-13T00:53:20+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=770
permalink: /2015/09/13/sony-entertainment-network-spam-stopper/
categories:
  - Programming FAQ
  - Software Development
tags:
  - network
  - playstation
  - python
  - sony
  - spam
comments: true
---
I don&#8217;t have a Sony PlayStation. But someone nicknamed &#8220;STRONDHA&#8221; has one, and somehow he managed to use my personal e-mail address to create his account. Oddly, Sony didn&#8217;t send any e-mail asking to confirm my e-mail as the owner of the account, and starting spamming every time this person bought a game in their virtual store.

<!--more-->

[<img src="http://www.alfersoft.com.ar/blog/wp-content/uploads/2015/09/2015-09-13-12.27.56-am-300x220.png" alt="Sony sending unwanted e-mails" width="300" height="220" class="aligncenter size-medium wp-image-771" srcset="http://www.alfersoft.com.ar/blog/wp-content/uploads/2015/09/2015-09-13-12.27.56-am-300x220.png 300w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2015/09/2015-09-13-12.27.56-am-1024x751.png 1024w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2015/09/2015-09-13-12.27.56-am-700x513.png 700w, http://www.alfersoft.com.ar/blog/wp-content/uploads/2015/09/2015-09-13-12.27.56-am-332x243.png 332w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.alfersoft.com.ar/blog/wp-content/uploads/2015/09/2015-09-13-12.27.56-am.png)

### Stop it

My name is too common, and is not the first time I get subscribed to some site I didn&#8217;t ask to. Sometimes, I just click on the &#8220;unsubscribe&#8221; link, or even on &#8220;forgot my password&#8221; and do whatever I need to stop receiving undesired e-mails.
  
So, I tried the &#8220;forget my password&#8221; trick with Sony, and after some captcha validation, I received an e-mail with a link. The problem is that they required the birthday in order to reset the password, and of course, I don&#8217;t know the birthday of the person that created the account.

Enters python script to try every possible birthday date from 1/1/1960 until today.
  
Notes:
  
&#8211; You will need to change the base URL to whatever you received in the password reset e-mail from Sony
  
&#8211; You might need to change the &#8216;wrong date&#8217; message to match your local language &#8211; mine is in portuguese (just try any date on the browser, and -unless you&#8217;re so lucky to select the actual birth date- copy and paste the error message on the code)

<div class="oembed-gist">
  <noscript>
    View the code on <a href="https://gist.github.com/fvicente/75980d0d00d759b06e50">Gist</a>.
  </noscript>
</div>

STHRONDHA was born on January 23 of 1983 (or at least that&#8217;s the date he choose when creating his account). So, now I was able to change the password and stop receiving e-mails from Sony. I guess he will need to create a new account now muahahaha.
