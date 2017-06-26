---
title: Raster map setup in Maria GDK
keywords: raster, setup
tags: [navigation]
sidebar: core_sidebar
permalink: core_raster_map_setup.html
summary: Setting up raster maps in Maria GDK 
---

# Raster map setup

Maria GDK raster map packages are used to setup and describe raster data. Although Maria GDK supports reading a large number of raster formats directly, the data should be converted to GeoPackage for optimal performance. Mappackages can be referenced from Maria GDK templates using the package mapsignature.


## The Raster Map packages XML

[Raster Map Packages XML](./core_raster_mappackages.html) - a detailed description of all the elements, parameters and values that make up the raster map package xml.

## Supported raster formats

[Supported formats](./core_raster_map_formats.html) - The GDAL formats which are included in the Raster Map Service

## Tutorials

*  [Interpolation and resampling methods](./core_raster_resampling_methods.html)

*  [Using GDAL to produce GeoPackages](./core_raster_gdal_geopackage.html)

*  [Using MARIA command line tools to produce GeoPackages](./core_raster_maria_geopackage.html)

*  [Using TileMapBuilder to produce tiled datasets](./core_raster_tilemapbuilder.html)
