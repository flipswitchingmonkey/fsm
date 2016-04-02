---
title: Enable a specific CUDA device – in Houdini
author: michael
layout: post
date: 2015-06-24
url: /2015/06/enable-a-specific-cuda-device-in-houdini/
categories:
  - Houdini
tags:
  - CUDA
  - Houdini

---
As a follow up to my previous article on how to enable a specific CUDA device using environment variables, 
it turns out that this works for HIP file environment variables as well.

By adding the `CUDA\_VISIBLE\_DEVICES` variable to your scene file, you can define which device will be 
used for the OpenCL acceleration. You can confirm the setting by opening the About Houdini dialog and checking 
the Show Details box. Halfway down those details, there should be a OpenCL Device line with the correct GPU selected.

Unfortunately it does not seem to work if I put the same setting into the 123.cmd file, to make the change global 
for Houdini. I&#8217;m still looking into why that is.