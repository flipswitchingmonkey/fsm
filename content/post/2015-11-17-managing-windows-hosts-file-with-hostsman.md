---
title: Managing Windows hosts file with Hostsman
author: michael
layout: post
date: 2015-11-17
url: /2015/11/managing-windows-hosts-file-with-hostsman/
categories:
  - Peanuts
tags:
  - Networking
  - Windows

---
Many people have not heard of a hosts file. It&#8217;s basically just a text file that lists IP addresses and domain names. Before every connection to a named address, your application (e.g. a browser) will check in this list to find the corresponding IP to a website&#8217;s domain name. If it can&#8217;t find it in the file, it will ask a DNS, a domain name server.

Here&#8217;s why this is awesome: it lets you, very easily, block traffic to known malware or ad sites, without using any kind of third party software. Just add a new entry with the domain in question and let it point to 0.0.0.0 and you&#8217;re done. The request will never leave your machine.

Here&#8217;s why it&#8217;s not used more: it requires editing a configuration file that is hidden away (and for good reason, if you just start blocking everything).

To ease the pain, there are a number of tools available, like the free [Hostsman](http://www.abelhadigital.com/hostsman) which I can recommend, since it comes with a nice editor and a way to automatically update your hosts from several online collections. No more need for adblocking plugins and whatnot. The requests to those sites never even get that far.

Be aware though that useful sites may get caught in the crossfire, and what some consider ads or malware, others might consider perfectly acceptable (and indeed, some ads are perfectly acceptable AND they also pay most of our bills, so don&#8217;t just block everything please&#8230;)
