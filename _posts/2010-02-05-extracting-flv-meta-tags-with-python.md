---
uid: 112
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
excerpt: A Python script to extract FLV meta tags from your pictures
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
