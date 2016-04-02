---
title: 'Cinema 4D: Default Lights'
author: michael
layout: post
date: 2015-02-03
url: /2015/02/cinema-4d-default-lights/
categories:
  - Cinema 4D
tags:
  - Cinema4D
  - Rendering

---
Today I invested some time into understanding how and when the Default Light influences an image. I did this because it bit me in the arse earlier today, when lighting with an HDRI image&#8230; but more about that later.

First of all, the setting can be found under Render Settings > Options > Default Light. It&#8217;s on by default.

Here are various combinations and how and if the Default Light plays into them.

#### Default Light {#defaultlight}

No lights in the scene causes the camera to emit a straight forward light (spot I assume) into the scene when Default Light is turned on. With it turned off, it&#8217;s just a black image.

![Default Light only](/uploads/2015/02/c4d_defaultlightonly.jpg)

#### Area Light + Default Light {#arealightdefaultlight}

First interesting thing: when Cinema finds another regular Light object in the scene, it automatically turns off the default light and uses the scene lights instead, which is what you would expect it to do. Thumbs up for that.

![Area Light + Default Light](/uploads/2015/02/c4d_arealight_defaultlight.jpg)

#### Physical Sky {#physicalsky}

A physical sky object with default settings. Default Light: OFF.

![Physical Sky only](/uploads/2015/02/c4d_physicalsky.jpg)

#### Physical Sky + Default Light {#physicalskydefaultlight}

A physical sky object with default settings. Default Light: ON. No difference. The default light is correctly overridden by the physical sky.

![Physical Sky + Default Light](/uploads/2015/02/c4d_physicalsky_defaultlight.jpg)

#### Sky Object no Material {#skyobjectnomaterial}

A Sky object with default settings and no material attached. Default Light: OFF.

![Sky Object no Material](/uploads/2015/02/c4d_skyplain.jpg)

#### Sky Object no Material + Default Light {#skyobjectnomaterialdefaultlight}

Now a Sky object with default settings and no material attached, but Default Light: ON. Notice something? Unlike the physical sky object, the sky object does **not** disable the default light!

![Sky Object no Material + Default Light](/uploads/2015/02/c4d_skyplain_defaultlight.jpg)

#### Sky Object with HDRI {#skyobjectwithhdri}

Our Sky Object now has an HDRI material attached. Default Light: OFF. All is well.
  
![Sky Object w/ HDRI](/uploads/2015/02/c4d_skyhdri.jpg)

#### Sky Object with HDRI + Default Light {#skyobjectwithhdridefaultlight}

Same, but Default Light: ON. And do you notice something? Suddenly the default light is back on! Causing a specular where you would not expect it and which will cause you to fiddle with your HDRI to no end&#8230;
  
![Sky Object /w HDRI + Default Light](/uploads/2015/02/c4d_skyhdri_defaultlight.jpg)

#### Sky Object with HDRI + Default Light + Regular Light {#skyobjectwithhdridefaultlightregularlight}

&#8230; and then when you managed to get your light to look right somehow (incorporating that unexpected specular somehow), you add another light. And behold: the Default Light disappears again! Causing your image to look different _again_.
  
![](/uploads/2015/02/c4d_skyhdri_defaultlight_light.jpg)

### Conclusion {#conclusion}

I can&#8217;t help but feel like this is a bug, but it is one that&#8217;s been there for a while I think. Until something changes, make absolutely sure to always, **always** turn off Default Light unless you really do not want to use any lights at all. Otherwise you are just opening yourself up for all kinds of headache when the default light starts interfering with your light setup.

**tl;dr:**

  * Light objects in the scene will turn Default Light off
  * Physical Sky objects in the scene will turn Default Light off
  * Sky objects in the scene will leave Default Light on (unless manually turned off in Options)
