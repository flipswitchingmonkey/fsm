---
title: Berlin Geo Data (or, why can nothing ever be easy?)
author: michael
layout: post
date: 2015-05-28
url: /2015/05/berlin-geo-data-or-why-can-nothing-ever-be-easy/
categories:
  - Houdini
tags:
  - DEM
  - Houdini

---
Following my previous adventures with geo data, today I had to deal with some from Berlin. First of all, it is fantastic that pretty much everything you want is available for free. So that&#8217;s a plus.

Not so cool: finding the right pieces of data is a bit of a hassle.

First step seemed straight forward: find the right coordinates. I used 
[this page](http://www.orchids.de/haynold/tkq/KoordinatenErmittler.php) to do it. I was expecting 
Gauß-Krüger coordinates to be used, and they looked similar enough to [this](http://fbinter.stadt-berlin.de/fb/berlin/service.jsp?id=a_dgm2@senstadt&type=FEED) 
to be right. Not so. They are using UTM WGS84, so make sure you&#8217;re looking them up correctly.

The actual data is here: [Stadtentwicklung Berlin Geoinformation](http://www.stadtentwicklung.berlin.de/geoinformation/geodateninfrastruktur/de/geodienste/atom.shtml)

What I wanted could be found under &#8220;ATKIS® DGM (2m-Rasterweite)&#8221; (which gives you XYZ based elevation data) and &#8220;Digitale farbige Orthophotos 2014 (DOP20RGB)&#8221; which gives you ortophotos of the Google Maps style variety. Yay! But: they are in the ECW format, ye olde trusty &#8220;ERDAS Compressed Wavelets&#8221;. Right. Totally. Which nothing can read. Irfan View tried and failed in 90%.

So, next issue: how to convert ECW to something usable? gdal_translate is what you need. In theory. Command line tool. Deal with it. Get the binary package at [gisinternals](http://www.gisinternals.com/query.html?content=filelist&file=release-1800-x64-gdal-1-11-1-mapserver-6-4-1.zip)

You can then find it under bin\gdal\apps\gdal_translate.exe. Copy it back to the bin folder, since otherwise it can&#8217;t find the DLLs (unless you want to add the folder to your path).

[Here&#8217;s how to use it](http://www.gdal.org/gdal_translate.html).

All good? Nice. In my case however, gdal_translate would not read the bloody ecw files, no matter what.

Turns out there is a much simpler solution: [XnView](http://www.xnview.com)! There&#8217;s a plugin for it that reads and converts the files. Hi-Fives all around! Get it [here](http://www.xnview.com/en/xnview/#addons)!

Feed it all into Houdini via the XYZ Python node and an Attribute From Map node and you get yourself a point cloud with the aerial picture assigned to the color values.

Then find out that, alas, the resolution still isn&#8217;t nearly enough for what you planned to do with it. 

The end. (;_;)
