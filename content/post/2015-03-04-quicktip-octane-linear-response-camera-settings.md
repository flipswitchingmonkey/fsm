---
title: 'Quicktip Octane: linear response camera settings'
author: michael
layout: post
date: 2015-03-04
url: /2015/03/quicktip-octane-linear-response-camera-settings/
categories:
  - Cinema 4D
tags:
  - Cinema4D
  - Octane
  - QuickTip

---
So, here&#8217;s a quick tip for Octane (Cinema 4D, but I assume it&#8217;s the same in other plugins as well). By default, when you add an Octane camera, under the Camera Imager tab Agfacolor\_Futura\_100CD is selected as the Response type. Even if &#8220;Enable Camera Imager&#8221; isn&#8217;t turned on, this response type is being used and causes your colors to be tinted in a certain way (presumably emulating the Film stock of that response setting). While often looking nice, it will change the way that your textures are rendered. You may have noticed that texture color sometimes looks off. This is probably why. To fix this, simply change the Response to Linear and the Gamma to 2.2. This should show you the colors 1:1 as they were in the texture file.

![](/uploads/2015/03/octane_cameraimager_linear.jpg)