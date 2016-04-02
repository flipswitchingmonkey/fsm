---
title: Windows 7 and Wacom Intuos – input lag
author: michael
layout: post
date: 2012-11-05
url: /2012/11/windows-7-and-wacom-intuos-input-lag/
categories:
  - Peanuts
tags:
  - Hardware
  - Wacom
  - Windows

---
Those using a Wacom tablet on Microsoft Vista/Win7 will notice that Wacom decided that their drivers are to fully support the Microsoft TabletPC scheme. Admittedly this is nice for those who do want to use their tablet for text recognition and everyday use. You also probably will not notice much of a problem if you are only using the left mousebutton (meaning none of the pen buttons) and only use the pen button to open the context menu. However the problems start if you use the tablet for 3D related work, as most applications require you to use and hold down all three mouse buttons (not at the same time of course…) and move the pen. In Maya right mouse+move would be the zoom, for example. With the latest Wacom drivers you will see a noticable lag with this action. The zoom will stutter for half a second, and then catch up. This is even worse than not reacting at all, as all of a sudden you are staring at pixel level zoom.
  
After talking to the Wacom support and prowling forums, the solution is rather simple and straightforward, but will mean that you lose TabletPC support (the Ink features, Flick etc. – I can live without them, others may not):
  
&#8211; Go to your Device Manager
  
&#8211; Disable (don’t delete!) the Wacom Virtual HID device
  
&#8211; Restart Windows
  
&#8211; Enjoy