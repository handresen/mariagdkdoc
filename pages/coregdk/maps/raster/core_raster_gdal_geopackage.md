---
title: Producing GeoPackages with GDAL
keywords: raster, gdal, geopackage
tags: [map, raster]
sidebar: core_sidebar
permalink: core_raster_gdal_geopackage.html
summary: Using GDAL to produce GeoPackages. 
---

[GeoPackage](http://www.geopackage.org/) is the preferred file format for rasters in Maria GDK. To produce GeoPackages you need GDAL 2.x. The general usage is explained in the [GDAL documentation](http://www.gdal.org/drv_geopackage_raster.html), but we want to expand on a few common use cases.

## Converting a file and building overviews

The simplest case is if you have a single file that you want to convert to GeoPackage. To do this we use [''gdal_translate''](http://www.gdal.org/gdal_translate.html). In this example the input file is missing its projection info, so we specify this with the ''-a_srs'' option.

    gdal_translate -of GPKG -a_srs "+proj=utm +zone=33 +ellps=GRS80 +units=m +no_defs" 33_N2000raster_1.tif 33_n2000raster.gpkg -co QUALITY=85

The QUALITY paramter controls the JPEG compression quality. The default is 75, which will produce quite a bit of artifacts, at least on map data. For aerial photos it may be OK.

The next step is *very important*. For the GeoPackage to have optimal performance, you need to produce overviews, i.e. downscaled versions of each image tile to use when zoomed out on lower detail levels.

To do this, we use the [''gdaladdo''](http://www.gdal.org/gdaladdo.html) command. Overviews should be in power-of-two sizes, i.e 2 4 8 16 etc. The overviews are stored inside the GeoPackage database so you do not specify any output file. 

    gdaladdo -r cubic 33_n2000raster.gpkg 2 4 8 16 32 64

Add as many overviews as you can for the input image, but not smaller than 256 pixels. To calculate the maximum downscale, simply divide the smallest image dimension by 256 and pick the closest smaller power of two. Example: The above image is *20000* pixels in both directions. *20000/256 = 78.125*. The closest lower power of two is *64*.

You should also set the JPEG compression to a suitable number here.


## Merging multiple datafiles into one GeoPackage

If you have multiple files that have the same resolution and projection, it is more efficient to keep them in one GeoPackage, than converting each file separately. To merge them, we first create a "virtual raster" of all the input files, with the GDAL command [''gdalbuildvrt''](http://www.gdal.org/gdalbuildvrt.html).

Example:

    gdalbuildvrt -a_srs "+proj=utm +zone=33 +ellps=GRS80 +units=m +no_defs" -addalpha 33_n1000raster.vrt *.tif

In this case, the source files are missing projection info, so we specify it with the ''-a_srs'' option. We also add the ''-addalpha'' option so that areas not covered by any raster images will be transparent (as opposed to being filled with black).

Next, we can convert this vrt like any other data set to GeoPackage, just as described above:

    gdal_translate -of GPKG 33_n1000raster.vrt 33_n1000raster.gpkg
    
And we can add overviews, also like above. In this case we use the smallest dimension of the .vrt file to calculate maximum downscale on overviews.  
    
    gdaladdo -r cubic 33_n1000raster.gpkg 2 4 8 16 32 64 128
    
    
## Setting a transparent color

Certain data sets may have a border color, typically either completely white or completely black. When converting these data sets to GeoPackage you may want to treat this color as transparent so that it will not appear in the map. This is done by adding an alpha channel and identifying the color in question as nodata.

To do this we use gdalwarp.exe

For example, we have a single TIFF file where we want to remove all pixels with the RGB color "255 255 252".

    gdalwarp -s_srs "+proj=utm +zone=33 +ellps=GRS80 +units=m +no_defs" -of GPKG -co "QUALITY=85" -srcnodata "255 255 252" -dstnodata "255 255 252 0" -dstalpha 33_n2000raster_1.tif 33_n2000raster.gpkg
    
In this example the source file doesn't have any projection info, so we specify it with the ''-s_srs'' option.  

The input can also be a .vrt as produced above with multiple input files. In this case, the ''-addalpha'' option should be omitted when producing the .vrt file.

## Google Maps tiling

Both Maria and other GIS systems are optimized for a Google maps compatible tiling scheme. GDAL has built-in support in gdal_translate for resampling and reprojecting to this tile scheme when the output is GeoPackage. Example:

    gdal_translate -of GPKG world_panorama_v15.vrt world_panorama_v15.gpkg -co TILING_SCHEME=GoogleMapsCompatible -co QUALITY=95
    
This global dataset will be reprojected to EPSG:3857 (Google Mercator) and tiled according to the Google Maps scheme: http://www.maptiler.org/google-maps-coordinates-tile-bounds-projection/




    
