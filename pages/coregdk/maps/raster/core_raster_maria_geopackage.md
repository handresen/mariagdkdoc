---
title: MARIA GeoPackage
keywords: raster, geopackage
tags: [map, raster]
sidebar: core_sidebar
permalink: core_raster_maria_geopackage.html
summary: Producing GeoPackages using MARIA command line tools 
---

# Using MARIA Command line tools to produce GeoPackages

For general raster image data you can usually use [GDAL](gdal_geopackage) to produce GeoPackages for use in Maria GDK. 

However, there are certain cases where which are not supported by GDAL, such as:


*  Elevation data geopackages

*  Normal map geopackages.

*  Generating mosaics of sets of imagery with different projections. 

The newest version of the Maria Command line tools should always be accessible here:

[`\\tpg-geostore\misc\bin\MariaNativeCmd`](\\tpg-geostore\misc\bin\MariaNativeCmd)

Some useful links:



*  [Global tiling map](http://www.maptiler.org/google-maps-coordinates-tile-bounds-projection/)

*  [Global tile system and resolutions](https://msdn.microsoft.com/en-us/library/bb259689.aspx)


### Resampling of elevation data

The Maria elevation tools support the following resampling methods via the ''--resamp'' command line parameter.


*  ''\-\-resamp nearest'' - Nearest point interpolation

*  ''\-\-resamp average'' - 2x2 Average resampling

*  ''\-\-resamp min'' - minimum value

*  ''\-\-resamp max'' - maximum value


### Tile formats

The geopackage elevation extension supports two tile formats for elevation data, TIFF with floating point samples and PNG with 16-bits unsigned int samples. This can be controlled with the ''--format'' command line option


*  ''\-\-format elev32f'' - Floating point TIFF

*  ''\-\-format elev16'' - 16-bits PNG (default)

### Vertcal resolution

For integer samples, the vertical resolution of the samples can be controlled with the ''\-\-precision'' command line option. This is given with a floating point value indicating the vertical resolution in meters of the samples, for example ''\-\-precision 0.1'' will give a 10cm resolution. This is done by applying a scale and an offset to each sample. You may not get the full resolution in all tiles, it depends on the range of values in the tile. For example, if you specify 0.1m resolution and the tile contains both sea level samples and mountains above 6554m you do not have enough resolution in a 16-bits integer, and the resolution will automatically be adjusted to accomodate this.

## GPKG Elevation tool

This is a conversion tool for elevation data. It will convert the elevation data to GeoPackage without any resampling or reprojection except for in the overviews.

This tool operates on a single input file, so for datsets consisting of multiple files, you need to produce a .vrt file first. 

### Options:

 | ''\-\-elevation''         | Elevation data mode (default)                                                                                                                        |  
 | ''\-\-normals''           | Normal map mode                                                                                                                                      | 
 | ''\-\-tilesize''          | Tile size in pixels (height == width)                                                                                                                | 
 | ''\-\-resamp <method>   | Resampling method: average,nearest,min,max (default is average)                                                                                      | 
 | ''\-\-format <fmt>      | Tile image format: ''any, png, jpeg, elev16 or elev32f.'' (png and jpeg are for normal maps).<br/> Default is elev16 for elevations, and png for normals. | 
 | ''\-\-quality <integer> | JPEG quality (0-100). Default 85                                                                                                                     | 
 | ''\-\-precision''         | Vertical resolution in meters                                                                                                                        | 
 | ''\-\-compress''          | Use LZW compression for TIFF tiles                                                                                                                   | 


### Example 1: Simple conversion of elevation data

This command will convert a floating-point elevation data set to a GeoPackage with LZW compressed tiff tiles.

    gpkg_elev --elevation --tilesize 256 --format elev32f --compress S0_DTM20.tif svalbard_elev.gpkg

If you have a dataset containing several elevation data files, you must build a .vrt and use that as input.

## Maria Tile Builder

This tool is used to produce global geopackages in Google Mercator projection. It will also in time be extended to do general tilings and projections.

This tool operates on Maria GDK rastermappackages.xml as input, i.e the same raster map definition files that are used in the raster service. This enables you to mix and match data from different sources and projections when creating your output mosaic.

The usage of this tool is described by running it with no input parameters. However, not all the listed parameters are currently functional, specifically the tiling and projection parameters are not working.

### Options:

 | ''\-\-elevation''                                | Elevation data mode (default)                                                                                                                        | 
 | -------------------                                | -----------------------------                                                                                                                        | 
 | ''\-\-normals''                                  | Normal map mode                                                                                                                                      | 
 | ''\-\-tilesize''                                 | Tile size in pixels (height == width)                                                                                                                | 
 | ''\-\-resamp <method>                          | Resampling method: average,nearest,min,max (default is average)                                                                                      | 
 | ''\-\-bounds <min_lat,min_lon,max_lat,max_lon> | Bounding box. Only include tiles that intersect this area.                                                                                           | 
 | ''\-\-minlevel <level>                         | Minimum quad tree level. Default is 0.                                                                                                               | 
 | ''\-\-maxlevel <level>                         | Maximum quad tree level. Default is 0                                                                                                                | 
 | ''\-\-format <fmt>                             | Tile image format: ''any, png, jpeg, elev16 or elev32f.'' (png and jpeg are for normal maps). Default is elev16 for elevations, and png for normals. | 
 | ''\-\-quality <integer>                        | JPEG quality (0-100). Default 85                                                                                                                     | 
 | ''\-\-precision''                                | Vertical resolution in meters                                                                                                                        | 
 | ''\-\-compress''                                 | Use LZW compression for TIFF tiles                                                                                                                   | 
 | ''\-\-map <signature>                          | Use a specific map signature from the raster map package.                                                                                            | 
 | ''\-\-nodata <value>                           | Set value to indicate missing data.                                                                                                                  | 

### Example 1: Simple raster image map

Here we want to produce a simple global map. The input data has a resolution of 423m/pixel, so we select level 9 as maximum. This means that we will be oversampling a bit, since level 9 with a tile size of 256x256 pixels corresponds to a resolution of 305m/pixel at the equator. The alternative is to go for level 8 and a bit of undersampling.

    mbt --logfile world_panorama.log --maxlevel 9 --quality 95  world_panorama_v15.rastermappackages.xml world_panorama_15.gpkg


### Example 2: Global normal maps with high resolution insert

In this example we want to produce a global normal map, but with high resolution normals over Norway. We set up the following raster map package with SRTM30 for the global data and 10m DEM data for norway. The global data are in unprojected Lat/lon while the norwegian DEM's are in UTM33.

```xml
<mappackages>
    <mappackage signature="globe_elevation">
        <params>
            <param name="mapContentType" value="Elevation" />
        </params>
      
	<rastermapdataset>
            <dataset dir="srtm30" extension=".dem" recursive="true" />
            <params>
               <param name="resampleMethod" value="Lanczos" />
               <param name="maxResolution" value="5" />
            </params>
        </rastermapdataset>

        <rastermapdataset>
            <dataset dir="DEM\DEM10mTiff" extension=".tif" recursive="true" />
            <params>
                <param name="resampleMethod" value="Bilinear" />
                <param name="minResolution" value="250" />
                <param name="maxResolution" value="5" />
            </params>
        </rastermapdataset>
    </mappackage>
</mappackages>
```

First we produce the base map down to level 8. With a tile size of 128x128 pixels this corresponds to about 1.2km resolution at equator.

    mtb --logfile globe_normals.log --normals --tilesize 128 --maxlevel 8 globe_elevation.rastermappackages.xml globe_normals.gpkg

Next, we run the tool once more with different level settings and a bounding box to create the high resolution insert:

    mtb --logfile globe_normals.log --normals --tilesize 128 --minlevel 8 --maxlevel 14 --bounds "57.5,4.0,72,32.0"  globe_elevation.rastermappackages.xml globe_normals.gpkg
    
This will insert tiles from level 8 to 14 (approx 20m resolution with 128x128 tiles) inside the specified bounding box.

### Example 3: Global elevation with high resolution insert

This dataset will be suited for the 3D globe with a tile size of 33x33. The maxlevel of 10 corresponds to about 1.2km at the equator. The input elevation data is the same as for the normal maps above

    mtb.exe --logfile globe_elevation.log --tilesize 33 --elevation --maxlevel 10  globe_elevation.rastermappackages.xml globe_elevation.gpkg

The high resolution insert goes to level 17 which corresponds to approximately 10m resolution with a 33x33 grid

    mtb --logfile globe_elevation.log --elevation --minlevel 10 --maxlevel 17  --bounds 57.5,4.0,72,32.0  globe_elevation.rastermappackages.xml globe_elevation.gpkg


