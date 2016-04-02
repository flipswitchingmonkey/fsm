---
title: 'Quicktip: Maya 2015 crashes at startup'
author: michael
layout: post
date: 2015-02-27
url: /2015/02/quicktip-maya-2015-crashes-at-startup/
categories:
  - Peanuts
tags:
  - Maya
  - QuickTip

---
Check the Output Window. If it says

`Invalid Python Environment: Python is unable to find Maya&#8217;s Python modules`

make sure you do not have any Python environment variables set. To check, just open a command line (cmd.exe) and type 

`set`

If you see any variables like PYTHONHOME, remove those by typing

`set PYTHONHOME=`

Note that this removes the variables only for this particular session, so if you open another cmd.exe, it&#8217;ll still be there.
  
Now that the variables are removed, start maya from **this** command line. If it now starts correctly it
means that maya is clashing with your Python installation. The easiest way to work around this
without breaking your install is to run maya from a small batch file instead of the .exe
and drop this as maya.bat or something into the maya bin folder and run maya this way.