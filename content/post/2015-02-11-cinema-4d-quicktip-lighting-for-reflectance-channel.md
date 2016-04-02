---
title: 'Cinema 4D Quicktip: Lighting for Reflectance Channel'
author: michael
layout: post
date: 2015-02-11
url: /2015/02/cinema-4d-quicktip-lighting-for-reflectance-channel/
categories:
  - Cinema 4D
tags:
  - Cinema4D
  - QuickTip
  - Rendering

---
As you can see I find out new stuff about the reflectance channel every day&#8230; again about the lighting this time. 

An interesting little find: if your material is only using reflectance, and its Roughness is set to 0% (fully reflective with no blur), a regular light will NOT show up at all! And it won&#8217;t light the scene either! Turn Roughness to 1% and voila, light&#8217;s back. But you may not want even 1%. Then&#8230;

here&#8217;s a workaround: make your light an Area light. Doesn&#8217;t matter whether Diffuse/Specular/GI is turned on or off, makes no difference! Then go to Details tab and turn on Show in Reflection. And now, there&#8217;s your light again.

I&#8217;m still trying to work out the details and whether this is as it should be or whether it is a bug.