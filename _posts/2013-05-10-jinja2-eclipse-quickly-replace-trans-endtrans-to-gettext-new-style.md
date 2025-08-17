---
uid: 604
title: 'Jinja2 + Eclipse: quickly replace trans / endtrans to gettext( ) new style'
date: 2013-05-10T22:02:58+00:00
author: fvicente
layout: single
guid: ?p=604
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
excerpt: A regular expression to quickly replace trans and endtrans old\'s jinja2 to gettext new style
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
