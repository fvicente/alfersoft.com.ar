---
id: 604
title: 'Jinja2 + Eclipse: quickly replace trans / endtrans to gettext( ) new style'
date: 2013-05-10T22:02:58+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=604
permalink: /2013/05/10/jinja2-eclipse-quickly-replace-trans-endtrans-to-gettext-new-style/
categories:
  - Programming FAQ
  - Software Development
tags:
  - eclipse
  - endtrans
  - gettext
  - jinja2
  - replace
  - trans
comments: true
---
<!--more-->

Open find/replace dialog and check &#8216;Regular expressions&#8217;

Find:

{% highlight bash %}
\{\% trans \%\}([^\{]*)\{\% endtrans \%\}
{% endhighlight %}

Replace with:

{% highlight bash %}
{% raw  %}
{{ gettext('$1') }}
{% endraw %}
{% endhighlight %}
