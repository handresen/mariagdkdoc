---
title: Maria GDK core documentation
keywords: GDK overview
tags: [map, track, drawobject, service]
sidebar: core_sidebar
permalink: core_landing_page.html
summary: Maria GDK documentation is primarily intended for development teams using the Maria GDK platform and for members of the Maria GDK development team. 
---

## Services
Maria Geo Developement Kit consists of a set of services and client side assemblies that can be used to develop a customized GIS application. The following resources cover the configuration and installation of the services and their relationships.

- Installing services (Maria GDK 1.5 and older)
- Installing services (Maria GDK 2.0 and newer)
- Configuring service addins

## Maps
[Preparing and configuring map packages for Maria GDK.](./core_maps_prepare.html)

### Vector Maps
The Vector Map Service is contained in the Maria GDK and dedicated to serve maps from vector data. The Vector Map Service reads the open standard format [Vector Tiles](https://github.com/mapbox/vector-tile-spec). These files are produced using [Geospatial Data Abstraction Library / OGR](http://www.gdal.org/) or [FME Desktop](http://www.safe.com/fme/fme-desktop/), and a proprietary tool. [Maria Map Maker](./mmm_landing_page.html) contains all the neccessary components for producing and styling vector maps. 


### Raster Maps
The Raster Map Service is contained in the Maria GDK and dedicated to serve maps from raster data. The Raster Map Service contains dedicated readers for :

*  Geopackage
*  MBTiles
*  ECW
*  QM2

In addition, the Raster Map Service uses [Geospatial Data Abstraction Library](http://www.gdal.org/) to read a wide variety of other raster [formats](./core_raster_map_formats.html). The Raster Service is controlled by a set of configuration files that defines the composition of each dataset. The Raster service produces data for visual maps, elevation analysis, and effect layers.

## Tracks

## Draw objects

## Developing applications with Maria GDK
These resources are intended for Maria Geo Developement Kit users. The reader is assumed to be proficient in .NET programming with C# and WPF.

### Overview

### Getting started

{% include links.html %}