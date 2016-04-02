---
title: Windows Firewall Notifier and MS Network Monitor
author: michael
layout: post
date: 2012-11-05
url: /2012/11/windows-firewall-notifier-and-ms-network-monitor/
categories:
  - Peanuts
tags:
  - Software
  - Windows

---
I just found [Windows Firewall Notifier][1], a tool very similar (minus the slick interface, but hey, it&#8217;s free) to tools known from OSX like Litte Snitch or Hands Off. Basically what it does is block everything going in and out, hijack the firewall notifications and lets you decide whether to create a rule &#8211; which it then does. It&#8217;s basically a convenience layer on top of the Windows Advanced Firewall interface.

I have only tested it for a short while, but so far I like it. And since it does no black magic in the background, but just creates rules for the regular Windows firewall, you can at any point just open the Windows Advanced Firewall interface and edit everything from there.

While I&#8217;m at it, another tool that&#8217;s not mentioned often enough is the [Microsoft Network Monitor][2], which is similar to Wireshark, but comes with a nicer interface. I especially like that it&#8217;s easy to filter by process.

 [1]: http://wokhan.online.fr/progs.php?sec=WFN
 [2]: http://www.microsoft.com/en-us/download/details.aspx?id=4865