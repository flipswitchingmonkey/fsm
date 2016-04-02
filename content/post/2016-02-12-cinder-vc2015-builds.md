---
title: Cinder VC2015 builds
author: michael
layout: post
date: 2016-02-12
url: /2016/02/cinder-vc2015-builds/
categories:
  - Code
tags:
  - C++
  - Cinder

---
If you&#8217;re working with Cinder on Windows, maybe these binary builds can be of some use to you.

I'll try to keep up with the development branches every now and then. For now, I have just added a new
branch to my [gitlab repo here](https://gitlab.com/flipswitchingmonkey/cinder_vc2015) with the latest 0.9.1_dev binary build, built in VC2015 for use in Visual Studio 2015.

If you are ever plagued by the `Unknown compiler version - please run the configure tests and report the results` message at build time, you need to update boosts visualc.hpp file under include\boost\config\compiler\visualc.hpp. Near the end there&#8217;s an if clause
  
`#if (_MSC_VER > 1800 && _MSC_FULL_VER > 190023506)`
  
You need to raise the versions to the correct one in that case (or just live with the warning which, as far as I know, does nothing else).

The repo:  
[https://gitlab.com/flipswitchingmonkey/cinder_vc2015](https://gitlab.com/flipswitchingmonkey/cinder_vc2015)