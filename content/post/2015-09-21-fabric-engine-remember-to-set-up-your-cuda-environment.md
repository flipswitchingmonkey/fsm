---
title: Fabric Engine – Remember to set up your CUDA environment
author: michael
layout: post
date: 2015-09-21
url: /2015/09/fabric-engine-remember-to-set-up-your-cuda-environment/
categories:
  - Peanuts
tags:
  - CUDA
  - Fabric Engine
image: "fabric-engine-remember-to-set-up-your-cuda-environment.png"
---
If, when you run a GPU canvas scene for the first time, Fabric Engine complains about missing hardware support, check your log files whether maybe `nvvm64_20_0.dll` was not found. It is a library that is part of the CUDA toolkit (Version 6.0 in this case) and, unlike the bin and libnvvp directory, is not added to your system path by the installer. So make sure to also add `c:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v6.0\nvvm\bin\` to it.

As also stated in the manual, of course, which one should have read but one did not: [Fabric Engine Doc](http://docs.fabric-engine.com/FabricEngine/latest/HTML/GPUCompute/index.html)

Also note that if you have more than one CUDA Toolkit version installed, the last one installed will set the `%CUDA_PATH%` environment variable. If this is not ehe same as the nvvm library one, this, again, will cause trouble. So make sure to also set the `%CUDA_PATH%` to `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v6.0`
