---
title: Krakatoa for Cinema 4D – First impressions after two weeks
author: michael
layout: post
date: 2014-10-17
url: /2014/10/krakatoa-for-cinema-4d-first-impressions-after-two-weeks/
categories:
  - Cinema 4D
tags:
  - Cinema4D
  - Krakatoa

---
![](/uploads/2014/10/C4D_Krakatoa_X01_take3_v01.jpg)

For the last two weeks I spent a lot of time rendering a few billion particles (not at the same time, luckily), using [Krakatoa for C4D](http://www.thinkboxsoftware.com/krakatoa-c4d/). The particles were originally simulated in Houdini and exported as caches. Within Cinema 4D, the caches were imported through the PRT Loader object, so that&#8217;s what I used the most. We actually tried the X-Particles and TFD integration as well, but both turned out to be lacking in performance and features for our project. X-Particles (version 2.5 I think?) just wasn&#8217;t able to deal with the amount of particles we wanted, and there&#8217;s still not the amount of control that something like Houdini offers. TFD is great for fire and smoke, but again, comparatively little control. Also, the TFD <> Krakatoa integration was very, very crash-happy.

Here&#8217;s a render based on something like 50 Mio. Particles, read from a PRT cache. Rendertime was, iirc, less than a minute despite three lights. The slowest part about it was reading in those hughe cache files :) (at 120Mio with a density and velocity, the caches grew to over 3GB per frame&#8230;)

![](/uploads/2014/10/X01_take1_0080.jpg)

All in all Krakatoa for C4D is a bit of a mixed bag. First and foremost, the rendering capability of dealing with hundreds of millions of particles is fantastic, and render times are very reasonable. With enough particles, the resulting renders were crisp and clean too. There&#8217;s little to complain about here and I don&#8217;t think there&#8217;s any difference between the C4D version and the various other Krakatoa implementations when it comes to rendering. So with every bit of criticism following, please keep in mind that in total, Krakatoa was a godsend for us and worked exceptionally well and stable where it counted! Also, a big plus: the Thinkbox support is very responsive and capable. No copy&paste responses and you can usually expect one within the day with someone you can actually have a conversation with. It&#8217;s a bit sad that this has to be mentioned at all, but there are so many larger companies who basically do not have any support at all (in effect at least). S

That said, there are a few minor quibbles.

[Magma](http://www.thinkboxsoftware.com/krakatoa-magma-2/). We don&#8217;t have it. Not much more to say here, except we really want it! Right now you are limited to very basic operations like multipliers that affect the entire particle cache, which isn&#8217;t particularly helpful. Fading in Particles by age, for example, can not be done. You have to make sure to do this at simulation time or live with it. Which leads too&#8230;

Channels. There&#8217;s also little to no use for any channels other than Position, Density and Velocity. Maybe Normals, if you want the normal pass rendered out. All the others like Age, Life.. (or any custom pass) I have found no way to use or influence the render outcome. So when you are using Krakatoa in C4D, keep this in mind and make sure everything is sorted out within your simulation beforehand.

The viewport preview of caches can be a bit iffy. Even when telling the loader to display every Nth particle, there&#8217;s still a good chance you will be missing a piece of your sim somwhere in your viewport, so to place a cache correctly in your scene, you will have to do preview renders.

But, in any case, if you have to deal with lots of particles and your main app is Cinema 4D, you can&#8217;t really go wrong with Krakatoa.
