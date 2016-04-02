---
title: 'Houdini: remember to lock your door – and your nodes'
author: michael
layout: post
date: 2015-04-02
url: /2015/04/houdini-remember-to-lock-your-door-and-your-nodes/
categories:
  - Houdini
tags:
  - Houdini
  - QuickTip

---
![](/uploads/2015/04/houdini_lock_nodes_pls_kthx.png)

Something that&#8217;s often forgotten (and then sometimes remember when waiting for the same, unchanged node to cook for the gazillions time) is the fact that you can lock nodes in Houdini. You can think of the lock as taking a snapshot of your node-graph up to the point of that node and then working off of that snapshot.

Disadvantages:

  * node is locked. So no changes.
  * HIP file grows bigger, because node&#8217;s contents are cached to the scene file

Advantages:

  * contents are cached in the file &#8211; which size-wise can be a problem, but it also means no reliance on external files. File nodes can be cached and that caches the contents of the actual file read in.
  * if memory is no issue, the scene grows potentially a lot faster, since none of the previous nodes need to be re-cooked
