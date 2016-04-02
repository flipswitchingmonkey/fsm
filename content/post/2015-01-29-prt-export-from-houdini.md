---
title: PRT Export from Houdini
author: michael
layout: post
date: 2015-01-29
url: /2015/01/prt-export-from-houdini/
categories:
  - Houdini
tags:
  - Houdini
  - Krakatoa

---
Houdini, unfortunately, has no native support for the PRT file format used by Krakatoa. There&#8217;s 
an [old plugin from Thinkbox that comes in as a Python module](http://www.thinkboxsoftware.com/news/2014/1/9/prt-exporter-python-module-source-for-houdini-available-on-g.html).
It compiles, but I never got it to actually do anything. If you get it to work, please let me know!

Luckily the good people from [Robotika and Fiction released their own exporter for Houdini 12](https://github.com/FictionIO/PRTRop) some while back.

I forked this and [modified it slightly](https://github.com/krautsourced/PRTRop_VS) to compile with HDK14 and VS. 

These were compiled and tested with the Indie version on Windows 7. I&#8217;d assume they work with the full version and maybe Apprentice too, but I don&#8217;t know.

Here are the DLLs for a few builds. I&#8217;ll update this post if new versions come along. No warranty or anything of course. It&#8217;ll either work or it won&#8217;t&#8230;

**Download**

Houdini 13.0.547: [PRTRop_13.0.547.rar](/files/houdini/PRTRop_13.0.547.rar)

Houdini 13.0.582: [PRTRop_13.0.547.rar](/files/houdini/PRTRop_13.0.582.rar)

Houdini 14.0.201.13: [PRTRop_14.0.201.13.rar](/files/houdini/PRTRop_14.0.201.13.rar)

Houdini 14.0.291: [PRTRop_14.0.201.13.rar](/files/houdini/PRTRop_14.0.291.rar)

Houdini 14.0.335: [PRTRop_14.0.335.zip](/files/houdini/PRTRop_14.0.335.zip)

Houdini 15.0.244.16: [PRTRop_15.0.244.16.zip](/files/houdini/PRTRop_15.0.244.16.zip)

