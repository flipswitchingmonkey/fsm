---
title: Reflectance Channel tested
author: michael
layout: post
date: 2015-01-29
url: /2015/01/reflectance-channel-tested/
categories:
  - Cinema 4D
tags:
  - Cinema4D
  - Rendering

---
With the release of Cinema 4D R16 came some new shading tech named the Reflectance Channel. It replaces the old reflection channel (but you can still find all of its previous functions as &#8220;Legacy&#8221; shaders) and offers a lot of power by giving you layered, physically correct reflection layers with all the bells and whistles like fresnel and bumps and anything else you want to combine from the amount of already available Cinema 4D shaders. The very first impression was everyone got from playing with it was usually a) pretty! and b) pretty&#8230;slow. Since we&#8217;re just starting with a bunch of new projects and existing projects already use up all the available Octane render time here, we had to make a choice to either rely on the regular old Advanced Render, go with Vray and live with its little &#8220;issues&#8221; let&#8217;s say, or go all in and be a Cinema 4D fanboy and do it all with Physical Renderer and Reflectance Channel. Like I said, the initial impression was that it felt rather slow and thus probably would not be the renderer of choice here. But, investing some more time into it proved more than worth it.

Let&#8217;s take this render for example (thanks to [Raphael Rau](http://www.silverwing-vfx.de/free_stuff.html) for the initial creation of these nice render balls!).
  
![](/uploads/2014/12/reflectance_spheres1280-1.jpg)

This was rendered as a 4k image on a 6-core i7, using C4D R16, Physical Render at medium settings and Reflectance Channels (two or three layers I think). Took about 16 minutes. That&#8217;s not bad at all! And I&#8217;m pretty sure vray would have used just as much. Let&#8217;s not even get started about AR+GI&#8230; (more about that in a moment).

I really fell in love with what you can do to metallic surfaces:
  
![](/uploads/2014/12/reflectance_spheres_crop.jpg)

It&#8217;s just a Beckmann layer with bump map (I generated a dirt map based on a baked texture in [Allegorithmic&#8217;s Bitmap2Material](http://www.allegorithmic.com/products/bitmap2material), another great tool!).

To figure out how all of this affects render times and whether some of the time could be shaved off, I just rendered the material ball with a few simple materials. When rendered with Physical Renderer, it was always at medium settings. When rendered with AR it was with AA set to 2x-4x. GI settings were also medium to good. Also, please keep in mind that all of this was highly non-scientific. I kept working during the longer renders and there are probably a lot of things to tweak for each and every one of them that could have saved time. Still, if nothing else, it&#8217;s a good indicator and what takes long and what does not. Here we go&#8230;

Regular Color channel with Legacy Spec, Physical, no GI: 00:15
  
![](/uploads/2014/12/ColorWithLegacySpec.jpg)

Regular Color channel with Legacy Spec, Physical, QMC+IR: 07:09
  
![](/uploads/2014/12/ColorWithLegacySpec_QMC_IR.jpg)

Regular Color channel with Legacy Spec, Physical, QMC+QMC: 11:09
  
![](/uploads/2014/12/ColorWithLegacySpec_QMC_QMC.jpg)

Regular Color channel with Legacy Spec, AR, QMC+IR: cancelled cause it looked like it would easily take 30+ mins

Ok so these were the old shaders and it was just to set up some initial comparison values.

Now, instead of Color (that was now actually turned off) I used a Lambertian reflectance layer. Reflectance and Spec both set to 50% made it look roughly similar. Physical: 1:20
  
![](/uploads/2014/12/Lambertian.jpg)

AR with subdivision sampling at 6: 8:13
  
![](/uploads/2014/12/LambertianAR.jpg)

Notice something? Yes. AR is f&#8230;ing slow at this. Which is why this was the last AR render I tried. Couldn&#8217;t be bothered any more.

Ok, so before any conclusions, I played with some glossy stuff too.

Spec Blinn Legacy, Reflection Legacy: 00:23
  
![](/uploads/2014/12/FresnelLegacy.jpg)

Spec Blinn Legacy, Reflection Legacy, 20% Roughness: 00:37
  
![](/uploads/2014/12/FresnelLegacyBlur20.jpg)

Spec Blinn Legacy, Reflection Legacy, 20%, QMC+IR: gave up&#8230;

Beckmann 100% Rough, Fresnel + Beckmann 0% Rough: 00:59
  
![](/uploads/2014/12/FresnelRefl.jpg)

Beckmann 100% Rough, Fresnel + Beckmann 20% Rough: 01:30
  
![](/uploads/2014/12/FresnelReflBlur20.jpg)

Lambertian Fresnel + Beckmann 0% Rough: 01:10
  
![](/uploads/2014/12/FresnelReflLambertian.jpg)

Color,Fresnel + Beckmann 50% Mask: 00:23
  
![](/uploads/2014/12/ColorFresnelReflBeckmann.jpg)

Color,Fresnel + Beckmann 50% Mask, Ball and Ground Lambertian: 01:39
  
![](/uploads/2014/12/EnvRefl_ColorFresnelReflBeckmann.jpg)

Color,Fresnel + Beckmann 50% Mask Rough 20%, Ball and Ground Lambertian: 02:02
  
![](/uploads/2014/12/EnvRefl_ColorFresnelReflBeckmannBlur20.jpg)

Beckmann 100% Rough, Fresnel + Beckmann 20% Rough: 03:43
  
![](/uploads/2014/12/EnvRefl_FresnelReflBeckmannBlur20.jpg)

So here&#8217;s the **conclusion**:
  
it all boils down to the last three renders above! What you see there are renders where all materials are using reflectance materials and there is no GI. But there is. In a way. Better, actually. Because GI is basically giving you reflectance. Which reflectance, ehm, gives you. Can best be seen at how the matte surface facing downwards is lit. Totally dark in the earlier renders that didn&#8217;t use GI, now very similar to it. So that&#8217;s **conclusion number 1**: if you are smart in how you place your reflectance materials, you can get GI look at a fraction(!) of the render times. I did not test animation yet, but I assume flicker will not be an issue either.
  
**conclusion number 2**: Use physical renderer with reflectance channel. Easy.
  
**conclusion number 3**: Use the Color channel to set up your diffuse color! Lambertian or a 100% rough Beckmann work just as well and are technically more correct &#8211; but they do take longer to render. And if in the end they look 99% the same, do you really care?

So there we have it. In my opinion the reflectance channel is a massive win for Maxon. It has moved the built-in Cinema 4D renderer from its previous glossy plastic look into Vray territory! I can&#8217;t say much about complex scenes and I&#8217;m pretty sure Vray will still rule there, but I&#8217;m really happy that the Physical Renderer is now a very valid option for many if not most projects.

I&#8217;ll have to spend some more time with it and as is often the case, my opinions may change. But whatever, this is a great time for rendering in Cinema 4D &#8211; AR, Physical, Octane, Vray, Corona, soon Arnold,&#8230; now to build that time machine to find the time to learn them all :)
