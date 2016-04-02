---
title: Krakatoa for Cinema 4D – Playback Graph troubles
author: michael
layout: post
date: 2014-10-28
url: /2014/10/krakatoa-for-cinema-4d-playback-graph-troubles/
categories:
  - Peanuts
tags:
  - Cinema4D
  - Krakatoa

---
Trying to slow down a simulation in Krakatoa, we ran into some issues. Namely, the playback graph feature is basically useless as it is right now. Why is that?

The non-subframe-interpolating method relies entirely on the velocity channel &#8211; and at that it relies on some specific amount of velocity that I haven&#8217;t quite been able to figure out. Worse, due to the missing Magma editor (see before) you can&#8217;t even modify your velocities properly. The Channel Scale tag only works after the fact it seems, so it is no help either. All you can do is re-export your entire sequence again. In my case I exported it again with scaled down velocity (0.042, since Thinkbox support thought it could be related to the framerate somehow factoring into the velocity scale (it was 24fps)). Make sure to keep the density and ID channel if you have it! Without ID you can&#8217;t later try the subframe interpolation&#8230;

Anyway, with the original cache this is what came out:

![](http://www.flipswitchingmonkey.com/wp-content/uploads/2014/10/krakatoa_playbackgraph.jpg)

Hardly what you&#8217;d call clean interpolation&#8230;

I tried leaving out the velocity entirely. That resulted in nothing happening until the half frame between two frames, at which point the pointcloud just snapped to the next frame (so frame 15.5 basically looks like 16&#8230; until 16.5, which then looks like 17 and so on).

With the re-exported cache, using no subframe interpolation didn&#8217;t really work at all. Same issue with very noticable difference around the half-frame mark.

With subframe interpolation (make sure you have at least +/-2 frames cached, since those are needed for the interpolation!) it was slightly better, but still far from perfect.

![](/uploads/2014/10/retimeskip_1.jpg)
![](/uploads/2014/10/retimeskip_2.jpg)
![](/uploads/2014/10/retimeskip_3.jpg)
![](/uploads/2014/10/retimeskip_4.jpg)
![](/uploads/2014/10/retimeskip_5.jpg)

In the end, even if it were to work as intended and all the caches&#8217; channels were of optimal scale, there&#8217;d still be a noticeable jump around the half frame mark, which really makes things difficult to use. Plus, at this moment, only P and Vel are interpolated at all. Density is not! Thus, you&#8217;ll have a jump in density at every frame unless you override it and just set it to 1 for all particles. Not ideal.

So. I&#8217;m still liking Krakatoa for C4D on the whole, but it is far behind its more mature brethren for Max and Maya. There are also a number of features that just don&#8217;t work that well, like the frame interpolation. Its basic rendering functionality is still great though. Which means that for now, the best way to use it is to make sure that everything you want to see in the end is sorted out during simulation time, and to make sure your caches contain all the timings and values you require before rendering them. Currently I think the C4D plugin is also quite a bit cheaper than the Maya and Max version, which may be related to the missing features. In my opinion it should stay cheaper until those are implemented or fixed.
