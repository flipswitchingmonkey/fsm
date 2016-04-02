---
title: Turbulence FD / GTX Titan benchmark results
author: michael
layout: post
date: 2013-03-30
url: /2013/03/turbulence-fd-gtx-titan-benchmark-results/
categories:
  - Peanuts
tags:
  - Benchmark
  - Cinema4D
  - GPGPU
  - GTX Titan

---
I bought a GTX Titan for use with Octane, and it scales up nicely, basically 1:1 with the number of CUDA cores. 
And since Turbulence FD is GPU calculated too, I was expecting to see some very nice results there too. 
However, I was already wary, since the TFD dev mentioned somewhere that the memory speed is what counts, 
not the number of CUDA cores. See for yourself just how true that is:

Using the Flamethrower example file that comes with TFD, Editor-Update off, on C4D R14 with TFT 1.0 rev 1120:

i3930 @4.2GHz: 2:44 min
  
GTX 680: 0:35 min
  
GTX Titan: 0:33 min

So it looks like at least for TFD, upgrading to a Titan really makes no sense, unless you are constantly running out of 
memory, in which case the 6GB may be the solution over the 4GB offered by the GTX 680.