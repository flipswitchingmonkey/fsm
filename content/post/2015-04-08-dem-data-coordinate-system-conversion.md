---
title: DEM data coordinate system conversion
author: michael
layout: post
date: 2015-04-08
url: /2015/04/dem-data-coordinate-system-conversion/
categories:
  - Peanuts
tags:
  - DEM

---
When dealing with DEM data, you can run into a whole bunch of coordinate systems other than the commen longitude/latitude. For example, DEM data in Germany is often provided based on the [Gauß-Krüger](http://en.wikipedia.org/wiki/Gauss%E2%80%93Kr%C3%BCger_coordinate_system) and [UTM](http://en.wikipedia.org/wiki/Universal_Transverse_Mercator_coordinate_system) system instead. Never heard of it? Well, it&#8217;s nice in so far as it gives you a rectangular grid. It&#8217;s not nice as it is really not straight forward to convert between the two&#8230; luckily there are a number of conversion tools available online:

[Orchids](http://www.orchids.de/haynold/tkq/KoordinatenErmittler.php?) LL to various using GMap

[Delattinia](http://www.delattinia.de/GMaps/maps-koordinaten.html) LL to GK using GMap

There are a number more (just google &#8216;lon lat utm convert&#8217; or something), but it is important to know which reference system you are working in (ETRS89 vs WGS84 for example) and also which UTM grid you are in. And so on. Which is why I really like the two links above, because you just get your position from the map directly and can be pretty sure you&#8217;re not too far off then&#8230;
