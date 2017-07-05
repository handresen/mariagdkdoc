---
title: Maps in Maria GDK
keywords: maps GDK
tags: [map]
sidebar: core_sidebar
permalink: core_maps_prepare.html
summary: Using maps in Maria GDK 
---


This section is intended for Maria GDK users, primarily working with advanced map setup, as well as for Teleplan Globe internal use. The reader is assumed to be proficient in Maria map setup, xml and general programming.

This document aims to explain the details of setting up Maria GDK map packages for Vector and Raster Maps, and how map packages can be combined using a MariaGDK map template to display a multilayered map.

{% include callout.html content="
The information is organized in tables with columns for element/attribute name, description and element properties." type="info" %}

Valid properties are:

 | **Property** | **Description**     | 
 | ------------ | ---------------     | 
 | O            | Is optional.        | 
 | R            | Is repeating.       | 
 | C            | Has child elements. | 
 | A            | Has attributes.     |


## Locating map files

The Vector and Raster map services searches for Maria GDK map-files in the default folder provided at installation, but can also search for data in other locations pointed to by link.xml-files. \\
Read [Configuring Link-xmls](core_maps_links.html) for more details.

## Setting up map templates

The map template xml joins together map data from different sources to create a multilayered, rich map for Maria GDK. A template can contain multiple layers referencing mapsignatures from both raster and vector mapservices. Layers can also act as modification layers (for f.ex. elevation shading effects) and placeholder layers (for special data like WMS and propagation). Templates are created and edited using [Maria Map Maker](mmm_landing_page.html).


Read [Details Map Templates](core_maps_templates.html) for more information on the xml structure of the templates

## Configuring vector maps

Read [Configuring Vector Maps](core_vector_map_setup.html) for more details.

## Configuring raster maps

Read [Configuring Raster Maps](core_raster_map_setup.html) for more details.

## Adding metadata

Read [Details metadata](core_maps_metadata.html) for more information.

## Adding bookmarks

Read [Details bookmarks](core_maps_bookmarks.html) for more information.

## Adding feature info lookup tables

Read [Details feature info lookup tables](core_maps_lookuptables.html) for more information.






{% include links.html %}