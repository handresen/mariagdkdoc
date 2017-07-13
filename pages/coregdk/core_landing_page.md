---
title: Maria GDK core documentation
keywords: GDK overview
tags: [map, track, drawobject, service]
sidebar: core_sidebar
permalink: core_landing_page.html
summary: Maria GDK documentation is primarily intended for development teams using the Maria GDK platform and for members of the Maria GDK development team. 
---
Maria Geo Development Kit (MARIA GDK) is a toolkit for development of high performance GIS applications. 

## Maps
[Preprocessing](./core_maps_preprocess_vector.html) vector data for use in MARIA GDK.

[Preparing and configuring](./core_maps_prepare.html) map packages for Maria GDK. Adding [metadata](./core_maps_metadata.html), [bookmarks](./core_maps_bookmarks.html) and [lookup tables](./core_maps_lookuptables.html) for feature info queries.

### Vector Map setup
The Vector Map Service is contained in the Maria GDK and dedicated to serve maps from vector data. The Vector Map Service reads the open standard format [Vector Tiles](https://github.com/mapbox/vector-tile-spec). These files are produced using [Geospatial Data Abstraction Library / OGR](http://www.gdal.org/) or [FME Desktop](http://www.safe.com/fme/fme-desktop/), and a proprietary tool. [Maria Map Maker](./mmm_landing_page.html) contains all the neccessary components for producing and styling vector maps. 

[Setting up vector maps](./core_vector_map_setup.html).

### Raster Maps setup
The Raster Map Service is contained in the Maria GDK and dedicated to serve maps from raster data. The Raster Map Service contains dedicated readers for Geopackage, MBTiles, ECW and QM2.

In addition, the Raster Map Service uses [Geospatial Data Abstraction Library](http://www.gdal.org/) to read a wide variety of other raster [formats](./core_raster_map_formats.html). The Raster Service is controlled by a set of configuration files that defines the composition of each dataset. The Raster service produces data for visual maps, elevation analysis, and effect layers.

[Setting up raster maps](./core_raster_map_setup.html).

### WMS

## On top of the map

### High performance tracking and geofencing
MARIA GDK has [tracking](./core_tracks.html) functionality highly optimized for receiving, processing and visualizing large amounts of real-time tracking data including aggregation of history and statistics along with rule based [geofencing](./core_geofencing.html).

### Draw object functionality
MARIA GDK has build-in support for Mil Std 2525C, and flexible functionality for definition and visualization of custom [draw objects](./core_drawobjects.html).

### Styling tracks and drawobjects
MARIA GDK features powerful mechanisms for controlling the appearance/[styling](./core_styling.html) of [tracks](./core_styling_track.html) and [draw objects](./core_styling_drawobjects.html).

### Geolocation
MARIA GDK [Geolocation](./core_geolocation.html) service allows fast, faceted, freetext searches for full or partial placenames.

### Point symbols

### Calculations
MARIA GDK has support for fast terrain analysis functions, elevation profiles, radio propagation and radio coverage [calculations](./core_calculations.html) and visualization.

### Vector map legend
Displaying [feature legends](./core_maps_vectorlegend.html) for vector maps.

### Map layer export
Performing bitmap [export](./core_maps_export.html) of a MARIA layer stack.

## Technologies

## Tools, libraries and utilities

## Installation and setup
Resources on service [installation](./core_setup_service_installation.md), [configuration](./core_setup_service_configuration.html) and [discovery](./core_setup_service_discovery.html).

## Maria GDK Releases
This is an overview of the available releases of the Maria 2012 GDK

 | Version | Released    | Changes   | Notes | Approved By | 
 | ------- | --------    | -------  | ----- | -----------  | 
 |1.0.0| 27 May 2012 |[Changes](http://issues.teleplanglobe.com/releaseNotes/M?q=version%3A+1.0.0&title=Change+log+Maria+GDK+Release&token=1BCFDEB82AE6833DA890D9438CCDF843&showDescription=false&showComments=false) | Incomplete list. | Pål Ulvenes | 
 | 1.1.0   | 16 Jan 2014 | [Changes](http://issues.teleplanglobe.com/releaseNotes/M?q=version%3A+1.1.0&title=Change+log+Maria+GDK+Release&token=1BCFDEB82AE6833DA890D9438CCDF843&showDescription=false&showComments=false)| | Pål Ulvenes         | 
 | 1.2.0   | 17 Jun 2014 | [Changes](http://issues.teleplanglobe.com/releaseNotes/M?q=version%3A+1.2.0&title=Change+log+Maria+GDK+Release&token=1BCFDEB82AE6833DA890D9438CCDF843&showDescription=false&showComments=false)|  | Thomas Martin Dørum | 
 | 1.3.0   | 17 Oct 2014 | [Changes](http://issues.teleplanglobe.com/releaseNotes/M?q=version%3A+1.3.0&title=Change+log+Maria+GDK+Release&token=1BCFDEB82AE6833DA890D9438CCDF843&showDescription=false&showComments=false)|  | Pål Ulvenes         | 
 | 1.4.0   | 26 Feb 2015 | [Changes](http://issues.teleplanglobe.com/releaseNotes/M?q=version%3A+1.4.0&title=Change+log+Maria+GDK+Release&token=1BCFDEB82AE6833DA890D9438CCDF843&showDescription=false&showComments=false) | Propagation service is not updated,<br/>version 1.3 is included in this release.| Pål Ulvenes|
 | 1.4.1   | 14 Apr 2015 | [ Changes](http://issues.teleplanglobe.com:8080/releaseNotes/M?q=Fix+versions%3A+1.4.1&title=Release+Note+Maria+GDK+version+1.4.1&token=wo63rm3u40ne24n7986unn4e&showDescription=false&showComments=false)  | Important bug fixes for version 1.4.0 | Pål Ulvenes | 
 | 1.5.0   | 10 Jul 2015 | [ Changes](http://issues.teleplanglobe.com:8080/releaseNotes/M?q=Fix+versions%3A+1.5.0&title=Release+Note+Maria+GDK+version+1.5.0&token=wo63rm3u40ne24n7986unn4e&showDescription=false&showComments=false)  | Propagation service is not updated,<br/>version 1.3 is included in this release. | Pål Ulvenes | 
 | 2.0.0   | 29 Jan 2016 | [ Changes](http://issues.teleplanglobe.com:8080/releaseNotes/M?q=Fix+versions%3A+2.0.0&title=Release+Note+Maria+GDK+version+2.0.0&token=1evk53dsqyomgsl3ok94fxw11&showDescription=false&showComments=false) | | Thomas Martin Dørum | 
 | 2.0.1   | 01 Apr 2016 | [ Changes](http://issues.teleplanglobe.com:8080/releaseNotes/M?q=Fix+versions%3A+2.0.1&title=Release+Note+Maria+GDK+version+2.0.1&token=1evk53dsqyomgsl3ok94fxw11&showDescription=false&showComments=false) | MariaToolbox is not updated,<br/>version 1.0.0 is included in this release. | Thomas Martin Dørum | 
 | 2.1.1   | 14 Sep 2016 | [ Changes](http://issues.teleplanglobe.com:8080/releaseNotes/M?q=Fix+versions%3A+2.1.1&title=Release+Note+Maria+GDK+version+2.1.1&token=1evk53dsqyomgsl3ok94fxw11&showDescription=false&showComments=false) |  | Thomas Martin Dørum | 
 | 3.1.0   | 01 Jun 2017 | [ Changes](http://issues.teleplanglobe.com:8080/releaseNotes/M?q=Fix+versions%3A+3.1.0&title=Release+Note+Maria+GDK+version+3.1.0&token=1evk53dsqyomgsl3ok94fxw11&showDescription=false&showComments=false) | | Thomas Martin Dørum | 




Nightly builds are available for early adaption and testing purposes.


{% include links.html %}