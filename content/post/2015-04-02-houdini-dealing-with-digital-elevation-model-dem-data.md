---
title: 'Houdini: Dealing with Digital Elevation Model (DEM) data'
author: michael
layout: post
date: 2015-04-02
url: /2015/04/houdini-dealing-with-digital-elevation-model-dem-data/
categories:
  - Houdini
tags:
  - DEM
  - Houdini
  - Python

---
This article is going to be a work in progress, since the subject is something I&#8217;m currently working on in a project and things change daily.

DEM data is basically a bunch of number giving you the elevation of a specific spot on earth. That spot is usually encoded in longitude and latitude or some other horizontal and vertical value. So, yes, basically XYZ data. Which makes it pretty easy to work with.

I&#8217;ve worked with two file formats so far, but there are a number more out there (though the principle stays the same, some come with extra header or meta data that needs to be evaluated before the data becomes useful).

### SRTM data (.hgt) {#srtmdatahgt}

SRTM stands for the Shuttle Radar Topography Mission by NASA, which kindly offers its data to the world. There are multiple versions in different resolution and states of data quality, but they all use the hgt file format. It&#8217;s just a list of values that stand for height measurements, and you get the location of those points from its filename (lon/lat). All the files are 1201&#215;1201 measurements, so you end up with a one degree longitute and latitude grid.

Btw., the amount of data can add up quite a bit &#8211; the full set of points for Germany from the SRTM data came to roughly 125 million points.

I&#8217;m using numpy to read the data and write them into an array. You should too. Numpy is great.

Here&#8217;s the basic idea. Put it into a Python node and be amazed.

    import numpy as np
    import os
    import math
    
    node = hou.pwd()
    geo = node.geometry()
    hgt_file = "c:/data/N00E000.hgt"
    skip_amount = 10
    
    lon = 10
    lat = 47
    
    basedir, fname = os.path.split(hgt_file)
    
    lon_EW = 'E' if lon > 0 else 'W'
    lat_NS = 'N' if lat > 0 else 'S'
    
    # this builds the correct filename based and the lon and lat variables
    base_fname = lat_NS + str(int(math.floor(lat))).zfill(2) + lon_EW + str(int(math.floor(lon))).zfill(3)
    fname = base_fname + ".hgt"
    hgt_file = basedir + os.sep + fname
    
    if os.path.exists(hgt_file):
        siz = os.path.getsize(hgt_file)
        # could hardcode dim to 1201, but future 1" measurement files will have higher dimensions
        dim = int(math.sqrt(siz/2))
        assert dim*dim*2 == siz, 'Invalid file size'
    
        # >i2 means 16bit integer, big-endian (which is how the hgt data is stored)
        elev = np.fromfile(hgt_file, np.dtype('>i2'), dim*dim).reshape((dim, dim))
    
        # define iterator outside so wwe can access index inside loop
        it = np.nditer(elev, flags=['f_index'])
        geo.addAttrib(hou.attribType.Global, "hgt", base_fname, create_local_variable=True)
        geo.setGlobalAttribValue("hgt", base_fname)
    
        # create a nice dialogue to let us interrupt the operation if it takes too long
        with hou.InterruptableOperation(
            "Creating points from HGT file", open_interrupt_dialog=True) as operation:
    
            while not it.finished:
                # skip_amount to reduce the data
                # -32768 is the measurement in the hgt files if data is missing or faulty
                if it.index % skip_amount == 0 and it[0] > -32768:
                    point = geo.createPoint()
                    col = math.floor(it.index % dim)
                    row = math.floor(it.index / dim)
                    height = it[0]
                    point.setPosition((row, height, col))
                percent = float(it.index) / float(dim*dim)
                operation.updateProgress(percent)
                it.iternext()
    

### XYZ ASCII data (.xyz) {#xyzasciidataxyz}

XYZ is a simple format. One data entry per line, three space separated values for x, y and z.

Keeping things simpler, without the fancy automated name generation etc., here&#8217;s how to read the data:

    import numpy as np
    import os
    
    node = hou.pwd()
    geo = node.geometry()
    
    xyz_file = hou.parm('/obj/geo1/python1/xyz_file').evalAsString()
    x_scale, y_scale, z_scale = hou.parmTuple('/obj/geo1/python1/xyz_scale').evalAsFloats()
    
    xyz = None
    if os.path.exists(xyz_file):
        xyz_array = np.genfromtxt(xyz_file, dtype=[('x', np.float), ('y', np.float), ('z', np.float)], delimiter=' ')
    
        for xyz in np.nditer(xyz_array):
            point = geo.createPoint()
            point.setPosition((xyz['x'] * x_scale, xyz['y'] * y_scale, xyz['z'] * z_scale))
    

### Creating a surface {#creatingasurface}

Houdini offers a lot of ways to surface point clouds. For example, how about creating a VDB from the points and then convert it to a PointSoup? That&#8217;s one way. Not super fast though.

But, remember that all the points are in an even grid. What else is in an even grid? A Grid object. So, here&#8217;s the, I think, fastest way to surface your map:

  * create a Grid, sized to the bounding box of the point cloud and makes sure the grid and point cloud are at roughly one level
  * write all the P.z values of the points to an attribute (e.g. &#8220;height&#8221;)
  * use Attribute Transfer to transfer &#8220;height&#8221; from the point cloud to the Grid, make sure to use a narrow kernel radius
  * use Attribute Wrangle to set the Grid&#8217;s @P.z to its height
  * if your pointcloud has unregular edges, you could use Blast to remove everything from the Grid where height was e.g. 0.0

This method has another great advantage: playing with the Grid resolution lets you scale between high- and low-poly versions of your map.

![](/uploads/2015/04/germany_DEM.jpg)
