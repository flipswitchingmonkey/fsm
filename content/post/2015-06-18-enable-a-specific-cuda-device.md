---
title: Enable a specific CUDA device
author: michael
layout: post
date: 2015-06-18
url: /2015/06/enable-a-specific-cuda-device/
categories:
  - Code
tags:
  - CUDA
  - Fabric Engine

---
I&#8217;ve been playing with CUDA and the [Fabric Engine](http://fabricengine.com/) 2.0 beta and wanted to test the
performance on a multi-GPU system and how each GPU did on itself. For this, I had to somehow force a
specific card to be visible to CUDA only. And it&#8217;s actually rather straight forward to do so,
using the `CUDA_VISIBLE_DEVICES` environment variable.

First of all, find out which device has which number. The best way is to use the deviceQuery tool that 
comes with the CUDA Toolkit. You can find it here, if it has been compiled:

    C:\ProgramData\NVIDIA Corporation\CUDA Samples\v7.0\bin\win64\Release\deviceQuery.exe
    

Otherwise there are Solution files for VS2011/12/13 at:

    c:\ProgramData\NVIDIA Corporation\CUDA Samples\v7.0\1_Utilities\deviceQuery\
    

It&#8217;ll output some very detailed information on your devices like this:

    C:\ProgramData\NVIDIA Corporation\CUDA Samples\v7.0\bin\win64\Release\deviceQuery.exe Starting...                                                               
    
     CUDA Device Query (Runtime API) version (CUDART static linking)                                                                                                
    
    Detected 2 CUDA Capable device(s)                                                                                                                               
    
    Device 0: "GeForce GTX 780 Ti"                                                                                                                                  
      CUDA Driver Version / Runtime Version          7.5 / 7.0                                                                                                      
      CUDA Capability Major/Minor version number:    3.5                                                                                                            
    ...
        lots of information
    ...
    
    deviceQuery, CUDA Driver = CUDART, CUDA Driver Version = 7.5, CUDA Runtime Version = 7.0, NumDevs = 2, Device0 = GeForce GTX 780 Ti, Device1 = GeForce GTX TITAN
    Result = PASS                                                                                                                                                   
    

The last line gives us the DeviceID and that is all you need. Just set 

    set CUDA_VISIBLE_DEVICES=1
    

to limit CUDA to the Titan, or

    set CUDA_VISIBLE_DEVICES=0,1
    

to use both. It&#8217;s a comma separated list.
