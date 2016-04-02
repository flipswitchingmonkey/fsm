---
title: Stop Parallels from syncing keyboard layout
author: michael
layout: post
date: 2013-01-24
url: /2013/01/stop-parallels-from-syncing-keyboard-layout/
categories:
  - Peanuts
tags:
  - OSX
  - VM
  - Windows

---
While probably a good idea in principle, the keyboard layout sync in Parallels got on my nerves. I&#8217;m using a custom layout on OSX and every time I powered up the Windows VM, parallels would swap the OSX layout back to the vanilla UK layout.

Changing this behaviour is easy. Shut down your VM and find the .pvm file containing it. &#8220;Show Package Contents&#8221; and open the config.pvs file in an editor (you probably want to backup it beforehand). Find the &#8220;KeyboardLayoutSync&#8221; tag and change &#8220;Enabled&#8221; from 1 to 0. Save. Done.