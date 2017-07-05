---
title: Preprocess Vector Data
keywords: maps GDK
tags: [map]
sidebar: core_sidebar
permalink: core_map_preprocess_vector.html
summary: Preprocessing vector data for use in Maria GDK 
---

The Vector Map Service is contained in the Maria GDK and dedicated to produce maps from vector data. The Vector Map Service reads a proprietary Teleplan Globe map format (.m6mdata, .m6mindex), as well as vector tiles (.vtiles.sqlite). Both of these formats are created from sets of m6m files. An m6m file is equivalent to a single layer in a spatial dataset.

The m6m files may be produced using [Geospatial Data Abstraction Library / OGR](http://www.gdal.org/) or [FME Desktop](http://www.safe.com/fme/fme-desktop/). Both require a writer / driver developed and distributed by Teleplan Globe to be able to produce m6m files.

### Basic guides

Basic instructions on how to translate vector files to a format that can be read by Maria GDK


*  [Producing m6m files using FME](core_map_preprocess_fme.html)

*  [Producing m6m files using OGR](./core_map_preprocess_ogr.html)

*  [Building m6m datasets from m6m files](./core_map_preprocess_m6m_indexing.html)

*  [Building vector tilesets from m6m files](./core_map_preprocess_vectortiles_indexing.html)


### Advanced guides

Instructions on larger operations related to map conversion and the creation of map packages for MariaGDK

*  [S-57 Hydrographic Data Translation](./core_map_preprocess_s57.html)

*  [N50 Data Translation](./core_map_preprocess_n50.html)

*  [OpenStreetMap data](./convertmap/osm)

