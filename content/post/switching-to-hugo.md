---
title: Switching to Hugo, Bitbucket and Aerobatic
author: michael
layout: post
date: 2016-03-30
categories:
  - Peanuts

---

I've had enough of the constant updates necessary to keep Wordpress even remotely safe. And even then,
security faults were popping up left and center. Plus, I got the feeling that the whole thing got really
slow recently, which may as much be the fault of my hoster, but honestly in the end I don't even care any
more. Since this is just my personal site, I never really required all the bells and whistles.

This is when I ran into [Hugo](https://gohugo.io). Hugo is a Go-based static site generator. Basically, you feed it
markdown files and a theme template, and it generates your site from that, as static HTML with no database etc.
required. (Of course if you wanted to you could attach all those fancy things too - but the whole point of this
exercise was to simplify the backend).

I currently host everything on [Uberspace](https://uberspace.de) who I can not recommend highly enough. Seriously, 
if you're looking for a server with "pay what you want" pricing and lots of available services, try them out.

To just host some static HTML though, I really do not need a full account on a server. All my videos are hosted either 
on Vimeo or Youtube, and whatever else I host is usually really small. I then saw a blog post mentioning that 
[Aerobatic](https://www.aerobatic.com) would now support [continuous deployment of Hugo sites](https://www.aerobatic.com/blog/easy-hugo-continuous-deployment).

Basically they are pulling the markdown files directly from my [Bitbucket](http://www.bitbucket.org) account and
then build the site using Hugo. It is then hosted on a CDN for you, using your own domain. And the first two sites and first domain
are free. And since I only have one site... _profit_!

I'm still in the process of working through all the features that Hugo offers, plus I chose a rather basic theme that
I am slowly building up. Expect some random changes on the site (as if you even cared, admit it...)

Anyway. Welcome :)