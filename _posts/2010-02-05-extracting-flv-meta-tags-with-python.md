---
id: 112
title: Extracting FLV meta tags with Python
date: 2010-02-05T21:07:36+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=112
permalink: /2010/02/05/extracting-flv-meta-tags-with-python/
categories:
  - Programming FAQ
tags:
  - flash
  - flv
  - metadata
  - python
  - video
comments: true
---
I&#8217;ve stolen this code from <a href="http://code.activestate.com/recipes/457406/" target="_blank">http://code.activestate.com/recipes/457406/</a> which in turn was stolen / ported from <a rel="nofollow" href="http://inlet-media.de/flvtool2">http://inlet-media.de/flvtool2</a>, made some small fixes in bugs related to the date conversion function, object and array unmarshaling, file name hardcoded, etc&#8230; If you want to learn more about the FLV format and metatags read Acrobat&#8217;s <a title="Video File Format Specification Version 10" href="http://www.adobe.com/devnet/flv/pdf/video_file_format_spec_v10.pdf" target="_blank">Video File Format Specification Version 10</a>
  
<!--more-->

{% highlight python %}
from struct import unpack
from datetime import datetime


class FLVReader(dict):
    """
    Reads metadata from FLV files
    """

    # Tag types
    AUDIO = 8
    VIDEO = 9
    META = 18
    UNDEFINED = 0

    def __init__(self, filename):
        """
        Pass the filename of an flv file and it will return a dictionary of meta
        data.
        """
        # Lock on to the file
        self.file = open(filename, 'rb')
        self.signature = self.file.read(3)
        assert self.signature == 'FLV', 'Not an flv file'
        self.version = self.readbyte()
        self.typeFlags = self.readbyte()
        self.dataOffset = self.readint()
        extraDataLen = self.dataOffset - self.file.tell()
        self.extraData = self.file.read(extraDataLen)
        self.readtag()

    def readtag(self):
        self.readint()  # unknown
        tagType = self.readbyte()
        self.read24bit()  # dataSize
        self.read24bit()  # timeStamp
        self.readint()  # unknown
        if tagType == self.AUDIO:
            print "Can't handle audio tags yet"
        elif tagType == self.VIDEO:
            print "Can't handle video tags yet"
        elif tagType == self.META:
            # endpos = self.file.tell() + dataSize
            self.event = self.readAMFData()
            metaData = self.readAMFData()
            # We got the meta data.
            # Our job is done.
            # We are complete
            self.update(metaData)
        elif tagType == self.UNDEFINED:
            print "Can't handle undefined tags yet"

    def readint(self):
        data = self.file.read(4)
        return unpack('>I', data)[0]

    def readshort(self):
        data = self.file.read(2)
        return unpack('>H', data)[0]

    def readbyte(self):
        data = self.file.read(1)
        return unpack('B', data)[0]

    def read24bit(self):
        b1, b2, b3 = unpack('3B', self.file.read(3))
        return (b1 << 16) + (b2 << 8) + b3

    def readAMFData(self, dataType=None):
        if dataType is None:
            dataType = self.readbyte()
        funcs = {
            0: self.readAMFDouble,
            1: self.readAMFBoolean,
            2: self.readAMFString,
            3: self.readAMFObject,
            8: self.readAMFMixedArray,
            10: self.readAMFArray,
            11: self.readAMFDate
        }
        func = funcs[dataType]
        if callable(func):
            return func()

    def readAMFDouble(self):
        return unpack('>d', self.file.read(8))[0]

    def readAMFBoolean(self):
        return self.readbyte() == 1

    def readAMFString(self):
        size = self.readshort()
        return self.file.read(size)

    def readAMFObject(self):
        result = {}
        data = True
        while data:
            size = self.readshort()
            key = self.file.read(size)
            dataType = self.readbyte()
            if not key and dataType == 9:
                break
            data = self.readAMFData(dataType)
            result[key] = data
        return result

    def readAMFMixedArray(self):
        size = self.readint()
        result = {}
        i = 0
        while i < size:
            key = self.readAMFString()
            dataType = self.readbyte()
            if not key and dataType == 9:
                break
            result[key] = self.readAMFData(dataType)
            i += 1
        return result

    def readAMFArray(self):
        size = self.readint()
        result = []
        i = 0
        while i < size:
            result.append(self.readAMFData())
            i += 1
        return result

    def readAMFDate(self):
        date = self.readAMFDouble() / 1000
        self.readshort()  # localoffset
        return datetime.fromtimestamp(date)

if __name__ == '__main__':
    import sys
    from pprint import pprint
    if len(sys.argv) == 1:
        print 'Usage: %s filename [filename]...' % sys.argv[0]
        print 'Where filename is a .flv file'
        print 'eg. %s myfile.flv' % sys.argv[0]
    for fn in sys.argv[1:]:
        x = FLVReader(fn)
        pprint(x)
{% endhighlight %}

Download this code snippet from <a title="flv.py" href="https://gist.github.com/fvicente/d05e25b99c49e48e19b6" target="_blank">gist</a>

<div id="_mcePaste" style="overflow: hidden; position: absolute; left: -10000px; top: 55px; width: 1px; height: 1px;">
  from&nbsp;struct&nbsp;import&nbsp;unpack<br /> from&nbsp;datetime&nbsp;import&nbsp;datetime</p> 
  
  <p>
    class&nbsp;FLVReader(dict):<br /> &nbsp;&nbsp;&nbsp;&nbsp;&#8221;&#8221;&#8221;<br /> &nbsp;&nbsp;&nbsp;&nbsp;Reads&nbsp;metadata&nbsp;from&nbsp;FLV&nbsp;files<br /> &nbsp;&nbsp;&nbsp;&nbsp;&#8221;&#8221;&#8221;
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;Tag&nbsp;types<br /> &nbsp;&nbsp;&nbsp;&nbsp;AUDIO&nbsp;=&nbsp;8<br /> &nbsp;&nbsp;&nbsp;&nbsp;VIDEO&nbsp;=&nbsp;9<br /> &nbsp;&nbsp;&nbsp;&nbsp;META&nbsp;=&nbsp;18<br /> &nbsp;&nbsp;&nbsp;&nbsp;UNDEFINED&nbsp;=&nbsp;0
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;def&nbsp;__init__(self,&nbsp;filename):<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#8221;&#8221;&#8221;<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Pass&nbsp;the&nbsp;filename&nbsp;of&nbsp;an&nbsp;flv&nbsp;file&nbsp;and&nbsp;it&nbsp;will&nbsp;return&nbsp;a&nbsp;dictionary&nbsp;of&nbsp;meta<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;data.<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#8221;&#8221;&#8221;<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;Lock&nbsp;on&nbsp;to&nbsp;the&nbsp;file<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.file&nbsp;=&nbsp;open(filename,&nbsp;&#8217;rb&#8217;)<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.signature&nbsp;=&nbsp;self.file.read(3)<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;assert&nbsp;self.signature&nbsp;==&nbsp;&#8217;FLV&#8217;,&nbsp;&#8217;Not&nbsp;an&nbsp;flv&nbsp;file&#8217;<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.version&nbsp;=&nbsp;self.readbyte()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.typeFlags&nbsp;=&nbsp;self.readbyte()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.dataOffset&nbsp;=&nbsp;self.readint()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;extraDataLen&nbsp;=&nbsp;self.dataOffset&nbsp;-&nbsp;self.file.tell()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.extraData&nbsp;=&nbsp;self.file.read(extraDataLen)<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.readtag()
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;def&nbsp;readtag(self):<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;unknown&nbsp;=&nbsp;self.readint()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tagType&nbsp;=&nbsp;self.readbyte()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;dataSize&nbsp;=&nbsp;self.read24bit()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;timeStamp&nbsp;=&nbsp;self.read24bit()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;unknown&nbsp;=&nbsp;self.readint()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if&nbsp;tagType&nbsp;==&nbsp;self.AUDIO:<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print&nbsp;&#8221;Can&#8217;t&nbsp;handle&nbsp;audio&nbsp;tags&nbsp;yet&#8221;<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;elif&nbsp;tagType&nbsp;==&nbsp;self.VIDEO:<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print&nbsp;&#8221;Can&#8217;t&nbsp;handle&nbsp;video&nbsp;tags&nbsp;yet&#8221;<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;elif&nbsp;tagType&nbsp;==&nbsp;self.META:<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;endpos&nbsp;=&nbsp;self.file.tell()&nbsp;+&nbsp;dataSize<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.event&nbsp;=&nbsp;self.readAMFData()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;metaData&nbsp;=&nbsp;self.readAMFData()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;We&nbsp;got&nbsp;the&nbsp;meta&nbsp;data.<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;Our&nbsp;job&nbsp;is&nbsp;done.<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;We&nbsp;are&nbsp;complete<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.update(metaData)<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;elif&nbsp;tagType&nbsp;==&nbsp;self.UNDEFINED:<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print&nbsp;&#8221;Can&#8217;t&nbsp;handle&nbsp;undefined&nbsp;tags&nbsp;yet&#8221;
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;def&nbsp;readint(self):<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;data&nbsp;=&nbsp;self.file.read(4)<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;unpack(&#8216;>I&#8217;,&nbsp;data)[0]
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;def&nbsp;readshort(self):<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;data&nbsp;=&nbsp;self.file.read(2)<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;unpack(&#8216;>H&#8217;,&nbsp;data)[0]
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;def&nbsp;readbyte(self):<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;data&nbsp;=&nbsp;self.file.read(1)<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;unpack(&#8216;B&#8217;,&nbsp;data)[0]
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;def&nbsp;read24bit(self):<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;b1,&nbsp;b2,&nbsp;b3&nbsp;=&nbsp;unpack(&#8216;3B&#8217;,&nbsp;self.file.read(3))<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;(b1&nbsp;<<&nbsp;16)&nbsp;+&nbsp;(b2&nbsp;<<&nbsp;8)&nbsp;+&nbsp;b3
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;def&nbsp;readAMFData(self,&nbsp;dataType=None):<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if&nbsp;dataType&nbsp;is&nbsp;None:<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;dataType&nbsp;=&nbsp;self.readbyte()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;funcs&nbsp;=&nbsp;{<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0:&nbsp;self.readAMFDouble,<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1:&nbsp;self.readAMFBoolean,<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2:&nbsp;self.readAMFString,<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3:&nbsp;self.readAMFObject,<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8:&nbsp;self.readAMFMixedArray,<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;10:&nbsp;self.readAMFArray,<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;11:&nbsp;self.readAMFDate<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;func&nbsp;=&nbsp;funcs[dataType]<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if&nbsp;callable(func):<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;func()
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;def&nbsp;readAMFDouble(self):<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;unpack(&#8216;>d&#8217;,&nbsp;self.file.read(8))[0]
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;def&nbsp;readAMFBoolean(self):<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;self.readbyte()&nbsp;==&nbsp;1
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;def&nbsp;readAMFString(self):<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;size&nbsp;=&nbsp;self.readshort()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;self.file.read(size)
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;def&nbsp;readAMFObject(self):<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result&nbsp;=&nbsp;{}<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;data&nbsp;=&nbsp;True<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;while&nbsp;data:<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;size&nbsp;=&nbsp;self.readshort()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;key&nbsp;=&nbsp;self.file.read(size)<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;dataType&nbsp;=&nbsp;self.readbyte()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if&nbsp;not&nbsp;key&nbsp;and&nbsp;dataType&nbsp;==&nbsp;9:<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;break<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;data&nbsp;=&nbsp;self.readAMFData(dataType)<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result[key]&nbsp;=&nbsp;data<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;result
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;def&nbsp;readAMFMixedArray(self):<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;size&nbsp;=&nbsp;self.readint()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result&nbsp;=&nbsp;{}<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;i&nbsp;=&nbsp;0<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;while&nbsp;i&nbsp;<&nbsp;size:<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;key&nbsp;=&nbsp;self.readAMFString()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;dataType&nbsp;=&nbsp;self.readbyte()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if&nbsp;not&nbsp;key&nbsp;and&nbsp;dataType&nbsp;==&nbsp;9:<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;break<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result[key]&nbsp;=&nbsp;self.readAMFData(dataType)<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;i&nbsp;+=&nbsp;1<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;result
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;def&nbsp;readAMFArray(self):<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;size&nbsp;=&nbsp;self.readint()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result&nbsp;=&nbsp;[]<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;i&nbsp;=&nbsp;0<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;while&nbsp;i&nbsp;<&nbsp;size:<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result.append(self.readAMFData())<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;i&nbsp;+=&nbsp;1<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;result
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;def&nbsp;readAMFDate(self):<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;date&nbsp;=&nbsp;self.readAMFDouble()/1000<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;localoffset&nbsp;=&nbsp;self.readshort()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;datetime.fromtimestamp(date)
  </p>
  
  <p>
    if&nbsp;__name__&nbsp;==&nbsp;&#8217;__main__&#8217;:<br /> &nbsp;&nbsp;&nbsp;&nbsp;import&nbsp;sys<br /> &nbsp;&nbsp;&nbsp;&nbsp;from&nbsp;pprint&nbsp;import&nbsp;pprint<br /> &nbsp;&nbsp;&nbsp;&nbsp;if&nbsp;len(sys.argv)&nbsp;==&nbsp;1:<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print&nbsp;&#8217;Usage:&nbsp;%s&nbsp;filename&nbsp;[filename]&#8230;&#8217;&nbsp;%&nbsp;sys.argv[0]<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print&nbsp;&#8217;Where&nbsp;filename&nbsp;is&nbsp;a&nbsp;.flv&nbsp;file&#8217;<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print&nbsp;&#8217;eg.&nbsp;%s&nbsp;myfile.flv&#8217;&nbsp;%&nbsp;sys.argv[0]<br /> &nbsp;&nbsp;&nbsp;&nbsp;for&nbsp;fn&nbsp;in&nbsp;sys.argv[1:]:<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;x&nbsp;=&nbsp;FLVReader(fn)<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;pprint(x)
  </p>
</div>
