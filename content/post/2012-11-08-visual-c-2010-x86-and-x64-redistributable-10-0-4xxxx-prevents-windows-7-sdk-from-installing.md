---
title: Visual C++ 2010 x86 and x64 Redistributable 10.0.4xxxx prevents Windows 7 SDK from installing
author: michael
layout: post
date: 2012-11-08
url: /2012/11/visual-c-2010-x86-and-x64-redistributable-10-0-4xxxx-prevents-windows-7-sdk-from-installing/
categories:
  - Peanuts
tags:
  - SDK
  - Windows

---
If your installation for the Windows 7 SDK (for example, to get 64 bit compilers with VS 2010 Express) fails on a recent system, check in your Programs and Features for Visual C++ Redistributables. Versions newer than 10.0.30319 will cause the installer to fail. You have to remove both 32 and 64 bit versions of them, then the installation should work.