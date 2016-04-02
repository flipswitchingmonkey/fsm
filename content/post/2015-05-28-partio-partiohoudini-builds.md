---
title: Partio / PartioHoudini builds
author: michael
layout: post
date: 2015-05-28
url: /2015/05/partio-partiohoudini-builds/
categories:
  - Code
  - Houdini
tags:
  - Houdini
  - Partio

---
After hours of going borderline crazy, I&#8217;ve just managed to build the latest version of Partio for Windows (Command Line and Houdini). This was way harder than it should be, thanks to all kinds of weird peculiarities in Visual Studio 2012 and 2013. Jeeez.

This build is based on the current redpawfx partio repository (as of 28/05/15).

It&#8217;s not perfect. The Houdini binary does not output any messages regardless of the verbosity level due to cout/cerr/endl not linking. I&#8217;m sure it&#8217;s something obvious for someone &#8211; for now I can live without it 😉

The Houdini plugin is compiled with VS2012.

The Command Line tools are compiled with VS2013.

There&#8217;s a partio.lib for both VS2012 and VS2013.

If you can use it, enjoy, no guarantees or warranty. It may set your machine on fire.
  
(for example, I had to randomly replace dynamically sized arrays with std::vectors because VS does not support them &#8211; who knows what that will cause&#8230; seems to work for now)

Anyway, here&#8217;s the download:

[partio binary may2015 (rar)](/uploads/2015/05/partio_binary_may2015.rar)

Here&#8217;s a newer Partio4Houdini version for 14.0.444:
  
[HoudiniPartio\_Release\_14.0.444](/uploads/2015/09/HoudiniPartio_Release_14.0.444.zip)

Don&#8217;t forget to have zlib.dll somewhere in your path too: [zlib](/uploads/2015/05/zlib.rar)
