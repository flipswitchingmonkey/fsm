---
title: Houdini QuickTip – OBJ Export and UVs
author: michael
layout: post
date: 2013-04-04
url: /2013/04/houdini-quicktip-obj-export-and-uvs/
categories:
  - Peanuts
tags:
  - Houdini
  - QuickTip

---
You must map your UVs onto the Vertices in Houdini, not onto the Points, if you plan to export your object
later on as a Wavefront OBJ. If UVs are mapped to the Points, an exported OBJ file will not contain any UV data at all.