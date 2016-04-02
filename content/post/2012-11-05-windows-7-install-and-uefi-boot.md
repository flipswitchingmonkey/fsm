---
title: Windows 7 install and UEFI boot
author: michael
layout: post
date: 2012-11-05
url: /2012/11/windows-7-install-and-uefi-boot/
categories:
  - Peanuts
tags:
  - Hardware
  - Windows

---
A problem I just ran into whilst installing Windows 7: upon reaching the part where you select the target drive to install Windows on, you get the error message &#8220;Windows cannot be installed to this disk&#8221;. It&#8217;s a blank, fresh SSD, so there really was no reason &#8211; until I noticed that I had booted from a USB cd-rom drive, and in the BIOS you can select either &#8220;regular&#8221; or UEFI boot mode for that cd-rom drive. It appears that when you boot through UEFI, a regular old SSD is not recognised as a bootable device. So the solution is to make sure to boot into non-UEFI mode, after which I was able to install as usual.