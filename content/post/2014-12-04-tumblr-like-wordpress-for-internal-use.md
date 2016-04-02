---
title: Tumblr like WordPress for internal use
author: michael
layout: post
date: 2014-12-04
url: /2014/12/tumblr-like-wordpress-for-internal-use/
categories:
  - Peanuts

---
I was fed up with dozens of emails circling around the office containing links to interesting tutorials or other things, only to be lost in the black hole that is everyone&#8217;s inbox. Pages like Tumblr, Facebook etc. work great for this kind of link-dump, but I did not want to have to set up something public for the company that would a) let people know what we are currently looking at and b) have to have artists log in to post something. Every single extra click is a barrier to them actually using it, as I have learnt from several painful attempts to set up Wikis, Blogs, Knowledge Bases,&#8230; you name it.

So here&#8217;s the simple solution and so far it works like a charm:

We have a Synology NAS running here and it is great, truly. Among other things it comes with a one-click install of WordPress (version 4 currently). So that&#8217;s our basis.

On top of this comes this nice little theme called [Stumblr](www.eleventhemes.com/stumblr-theme/). Great for just posting Vimeo or Youtube clips!

Next I set up a guest user with post privileges and a default category for the posts (&#8220;Tutorials&#8221; in this case).

There&#8217;s a very nice plugin called [TT Guest Post Submit](https://wordpress.org/plugins/tt-guest-post-submit/) which handles the setup of a post form for guest users.

And that is basically it! I also modified the comment template a tiny bit, so that unnecessary fields won&#8217;t even show up, and it is done. Our artists can now just click on the &#8220;New post&#8221; link and fill out two fields &#8211; the title and content. And thanks to the smart theme, content only needs the actual youtube or vimeo link, no embed tags or so. Submit and it&#8217;s ready. Two clicks, two fields.

If this won&#8217;t work, then literally nothing will&#8230;
