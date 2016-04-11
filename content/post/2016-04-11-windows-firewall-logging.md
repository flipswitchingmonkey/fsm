---
title: Windows Firewall Event Logging
author: michael
layout: post
date: 2016-04-11
categories:
  - Windows
tags:
  - Windows
  - Firewall
  - Network
image: "2016-04-11-windows-firewall-logging.png"
---

I've been testing [Windows Firewall control](http://www.binisoft.org/wfc.php) for some time now and it's a nice interface for the built-in Windows Firewall 
(quite similar to e.g. Little Snitch on the Mac, although not as refined yet). Anyway, something to keep in mind is, for its Notifications to work, you
need to have WF Event auditing aka. logging turned on, which it is not by default. The installer should, I think, do this automatically. Somehow though
it was reset at some point (update? some change I made? who knows.)

To make things easier in the future, I wrote three very, very simple batch files to turn these notifications on and off. You may want to turn the off if
you don't need them, because they will put your log files full of shit. Lots of it. Thousands of entries a minute, if you're a bit busy.

    enable-WF-logging.bat
``` batch
echo off
cls
echo Enable Firewall Auditing
auditpol /set /subcategory:"Filtering Platform Packet Drop" /success:enable /failure:enable
auditpol /set /subcategory:"Filtering Platform Connection" /success:enable /failure:enable
```

    enable-WF-failure-logging.bat
``` batch
echo off
cls
echo Enable Firewall Failure-only Auditing
auditpol /set /subcategory:"Filtering Platform Packet Drop" /success:disable /failure:enable
auditpol /set /subcategory:"Filtering Platform Connection" /success:disable /failure:enable
```

    disable-WF-logging.bat
``` batch
echo off
cls
echo Disable Firewall Auditing
auditpol /set /subcategory:"Filtering Platform Packet Drop" /success:disable /failure:disable
auditpol /set /subcategory:"Filtering Platform Connection" /success:disable /failure:disable
```

Simple enough.