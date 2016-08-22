---
title: Middle-Mouse panning in Unreal Editor 4
author: michael
layout: post
date: 2016-04-22
categories:
  - Development
  - Gamedev
tags:
  - Development
  - Unreal
image: "2016-04-22-middle-mouse-panning-in-unreal-editor.jpg"
socialsharing: true
---

Lately I've been playing around with the [Unreal Engine 4 source code](https://github.com/EpicGames/UnrealEngine) and it's quite a 
lot of fun - especially the Nvidia Flex builds, that integrate the [Flex](https://developer.nvidia.com/flex) Physics library. Crazy stuff. 

What bothered me a lot was that in the node editors you were only able to pan using the right mouse button, which was also used for the
context menu. So whenever there was a bit of lag, the context menu would pop up instead of the pan happening. It was also inconsistent
with the rest of the UI, namely the viewports which were being panned using the middle mouse button.

Turns out it was very easy to add the middle mouse button to the node editor. I've forked the 4.11.2 build and added those changes to
[my branch](https://github.com/flipswitchingmonkey/UnrealEngine/tree/flipswitchingmonkey).

Here's the Pull Request:
[https://github.com/EpicGames/UnrealEngine/pull/2311](https://github.com/EpicGames/UnrealEngine/pull/2311)

It really is only a matter of adding the middle mouse button handling to these functions:

    SNodePanel.cpp
    -> SNodePanel::OnMouseButtonDown
    -> SNodePanel::OnMouseMove
    -> SNodePanel::OnMouseButtonUp
    
