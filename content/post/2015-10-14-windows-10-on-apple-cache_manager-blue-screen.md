---
title: Windows 10 on Apple – CACHE_MANAGER blue screen
author: michael
layout: post
date: 2015-10-14
url: /2015/10/windows-10-on-apple-cache_manager-blue-screen/
categories:
  - Peanuts
tags:
  - OSX
  - Windows

---
We&#8217;ve just tried to install Windows 10 (and 8.1 previously, same problem) on an older Mac Pro. Whatever we did, we ended up with a Blue Screen shortly after starting up Windows, citing an error in the cache_manager.

Cache_Manager errors point towards either problems with your drives (or, aha!, the drivers trying to access those drives!) or memory. Since it happens on an otherwise (on OSX) running machine, we assumed that both drives and memory were ok. The problem were the Bootcamp drivers, more specifically the driver to access HFS formatted drives. Since this was called every time Windows found the OSX partition, it would crash that very moment.

So the solution is to not load that driver and all is good. It means you can&#8217;t access your OSX partition, but in our case that didnt matter since the data drives were NTFS formatted anyway and everything else is on the network.

The driver in question is called AppleHFS.sys. You can find it on your system drive under `Windows\system32\drivers\AppleHFS.sys`

Boot into the system recovery console and open the command prompt, then navigate to that folder and just rename the file to e.g. &#8220;ren AppleHFS.sys AppleHFS.sysBAK&#8221;. Make sure you changed to the (presumably) C: drive! Because by default the recovery console mounts its on recovery version of Windows, which itself has a Windows\system32\drivers\ folder, but a) won&#8217;t have the Apple drivers and b) won&#8217;t have any effect on your actual system anyway.

Reboot. Done. Happytime.