---
title: Make Unity open scripts in VS 2012
author: michael
layout: post
date: 2013-02-01
url: /2013/02/make-unity-open-scripts-in-vs-2012/
categories:
  - Peanuts
tags:
  - Unity
  - Windows

---
There are cases where Unity 4, even when explicitely told to through the Editor settings, won&#8217;t open scripts in VS 2012, but rather will open VS 2010 instead. This usually happens if VS 2010 SP1 was installed later. If that&#8217;s the case, the updater changes a few registry settings that screw up the Unity<>VS connection. But the fix is rather simple, luckily (provided you&#8217;re not to scared to fuck up your registry&#8230;)

    - Open your Registry Editor and find [HKEY_CLASSES_ROOTVisualStudio.DTE]
    - Notice there are [HKEY_CLASSES_ROOTVisualStudio.DTE.10.0] and [HKEY_CLASSES_ROOTVisualStudio.DTE.11.0] right below
    - [HKEY_CLASSES_ROOTVisualStudio.DTE] has two keys, CLSID and CurVer. Those two point to the [HKEY_CLASSES_ROOTVisualStudio.DTE.10.0] and [HKEY_CLASSES_ROOTVisualStudio.DTE.11.0] values.
    - [HKEY_CLASSES_ROOTVisualStudio.DTECurVer] probably says VisualStudio.DTE.10.0 right now. Change that to VisualStudio.DTE.11.0 and also [HKEY_CLASSES_ROOTVisualStudio.DTECLSID] to the one in [HKEY_CLASSES_ROOTVisualStudio.DTE.11.0CLSID]

Unity should now open VS 2012 correctly.