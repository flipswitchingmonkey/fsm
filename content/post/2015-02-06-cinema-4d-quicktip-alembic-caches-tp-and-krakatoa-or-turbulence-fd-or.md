---
title: 'Cinema 4D Quicktip: Alembic Caches, TP and Krakatoa (or Turbulence FD, or…)'
author: michael
layout: post
date: 2015-02-06
url: /2015/02/cinema-4d-quicktip-alembic-caches-tp-and-krakatoa-or-turbulence-fd-or/
categories:
  - Cinema 4D
tags:
  - Alembic
  - Cinema4D
  - Krakatoa
  - QuickTip

---
Previously I posted a PRT ROP for Houdini, so that point clouds could be exported as PRT files to be then rendered in Cinema 4D via Krakatoa (or anything else that accepts PRT files of course). This is still a very valid workflow. You can stop reading now if you want <img src="http://www.flipswitchingmonkey.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

Now, I&#8217;m a big fan of the Alembic format. It&#8217;s just super convenient. And if you&#8217;re not dealing with a 100 million particles (actually&#8230; maybe even then? Haven&#8217;t tested it yet) it is very nice to have your entire particle sim sequence in one file instead of hundreds of single files.

Here are a few facts in advance:

  * Cinema lets you open Alembic files with points as either Polygon geometry or Particle Geometry (aka TP)
  * Krakatoa offers offers a PRT load for its own format
  * Krakatoa also offers a TP Source object to use Thinking Particles groups as render sources

First approach:
  
![first approach](/uploads/2015/02/abc_tp_01.png)
  
As straight forward as it can be. Drag in the Alembic file, select Particle Geometry as the Point format and add a Krakatoa TP Source object. You don&#8217;t even have to change any option, it&#8217;ll just work. **However**: the big advantage of using an Alembic Cache is that you can position, scale and rotate it to your hearts content. That is not only very useful for correct positioning, but you can also put multiple instances in your scene at different positions in time and a single sim will give you a bunch of different splashes or whatever. Unfortunately when you import Alembic Points as Particle Geometry, you&#8217;re out of luck here. It&#8217;ll takes its default PSR and stay there, no matter what. Annoying!

The fix:
  
![the fix](/uploads/2015/02/abc_tp_02.png)
  
First of all, import you Alembic cached points as Polygon geometry, not particles. This is important, because now you have the full range of options to transform the cache as described above. Then use a Matrix object like so to generate Thinking Particles out of your cache:
  
![](/uploads/2015/02/abc_tp_03.png)
  
Again I did not configure much here. Obviously you could use multiple caches writing to multiple TP groups and use multiple Krakatoa TP Source objects to render with different settings etc. Make sure to turn the visiblity of the Matrix object **off**! Because otherwise your viewport will be clogged with Matrix cubes. And for the TP generation part the object does not care whether it&#8217;s visible or not.

And that is it. Now you can transform your caches and even animate them and still end up with nice and clean TP that can be read by other plugins like Krakatoa, Turbulence FD, X-Particles etc. pp.
