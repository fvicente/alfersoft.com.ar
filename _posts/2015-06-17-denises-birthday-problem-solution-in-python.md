---
uid: 752
title: 'Denise&#8217;s birthday problem solution (in Python)'
date: 2015-06-17T11:16:19+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=752
categories:
  - Software Development
tags:
  - Denise
  - math
  - problem
  - python
  - script
  - solution
comments: true
excerpt: A python script to resolve Denise's birthday problem
---
<a href="https://www.reddit.com/r/puzzles/comments/3785h9/when_is_denises_birthday/" target="_blank">Denise&#8217;s birthday problem</a> solution (in Python)

{% highlight python %}
# -*- coding: utf-8 -*-
from collections import defaultdict

# (dd, mmm, yyyy)
dates = [
    (17, "Feb", 2001),
    (13, "Mar", 2001),
    (13, "Apr", 2001),
    (15, "May", 2001),
    (17, "Jun", 2001),
    (16, "Mar", 2002),
    (15, "Apr", 2002),
    (14, "May", 2002),
    (12, "Jun", 2002),
    (16, "Aug", 2002),
    (13, "Jan", 2003),
    (16, "Feb", 2003),
    (14, "Mar", 2003),
    (11, "Apr", 2003),
    (16, "Jul", 2003),
    (19, "Jan", 2004),
    (18, "Feb", 2004),
    (19, "May", 2004),
    (14, "Jul", 2004),
    (18, "Aug", 2004),
]

# Albert = mmm
# Bernard = dd
# Cheryl = yyyy
dd = 0
mmm = 1
yyyy = 2

def my_filter(dates, what_i_know, what_other_knows):
    # count repetitions
    other_counter = defaultdict(int)
    for d in dates:
        other_counter[d[what_other_knows]] += 1
    # group
    others_by_me = defaultdict(list)
    for d in dates:
        others_by_me[d[what_i_know]].append(d[what_other_knows])
    possible = []
    for k, v in others_by_me.iteritems():
        if len(filter(lambda x: other_counter[x] > 1, v)) == len(set(v)):
            possible.append(k)
    return filter(lambda x: x[what_i_know] in possible, dates)

# Albert: I don’t know when Denise’s birthday is, but I know that Bernard does not know.
dates = my_filter(dates, mmm, dd)
# Bernard: I still don’t know when Denise’s birthday is, but I know that Cheryl still does not know.
dates = my_filter(dates, dd, yyyy)
# Cheryl: I still don’t know when Denise’s birthday is, but I know that Albert still does not know.
dates = my_filter(dates, yyyy, mmm)

print dates
{% endhighlight %}

[See denise.py on gist](https://gist.github.com/fvicente/8f95f8b6c8a789de6ea5)
