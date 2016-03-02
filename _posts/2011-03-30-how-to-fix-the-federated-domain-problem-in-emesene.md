---
uid: 215
title: 'How to fix the &#8220;federated domain&#8221; problem in emesene'
date: 2011-03-30T14:36:23+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=215
permalink: /2011/03/30/how-to-fix-the-federated-domain-problem-in-emesene/
categories:
  - Linux FAQ
  - Software Applications
  - Software Development
tags:
  - bug
  - domain
  - emesene
  - federated
  - fix
  - issue
  - message
comments: true
---
<figure>
	<img src="{{ site.baseurl }}/images/emesene-logo.png">
</figure>

If your emesene keeps displaying the annoying message &#8220;User could not be added: Email Domain is IM Federated Contact LiveID xxx@yyy.com is federated domain.&#8221; no matter if you accept or reject the user, then you probably want to apply this patch.

This is not really a fix, but a workaround, is just a hack to avoid displaying the error message if you really like emesene and you want to use it. If you don&#8217;t like this kind of ugly solutions, there are always other options like Pidgin, aMSN, etc. &#8212; in other words if you don&#8217;t like programming forget it, or wait for a new emesene version.

<!--more-->

Ok, so, since we have the source code (emesene was developed in python) the file we need to hack is `/usr/share/emesene/Controller.py`

We just need to find the two lines that displays the messages, and bypass them if the user is our xxx@yyy.com. That would be (in the current version as 2011/03/30) lines 674 and 770.

in line 674 add:

{% highlight python %}
if mail in ("xxx@yyy.com", ): continue
{% endhighlight %}

then in line 770 add:

{% highlight python %}
if email in ("xxx@yyy.com", ): return
{% endhighlight %}

So the final Controller.py will look something like:

{% highlight python %}
...

    def checkPending(self):
        '''Check for users pending to be added'''

        if self.msn is None:
            return False

        if self.addBuddy is None:
            self.addBuddy = dialog.AddBuddy(self)

        users = self.msn.checkPending()
        if len(users) > 0:
            for mail in users:
                if mail in ("xxx@yyy.com", ): continue
                nick = self.msn.getUserDisplayName(mail)
                self.addBuddy.append(nick, mail)
        return False

...

    def addNotification(self, msnp, command, tid, params, email, nick):
        '''this method is called when a user adds you'''

        if self.addBuddy is None:
            self.addBuddy = dialog.AddBuddy(self)
        if email in ("xxx@yyy.com", ): return
        self.addBuddy.append(nick, email)

{% endhighlight %}
