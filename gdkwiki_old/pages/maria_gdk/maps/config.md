# Preparing Map Packages


This section is intended for Maria GDK users, primarily working with advanced map setup, as well as for Teleplan Globe internal use. The reader is assumed to be proficient in Maria map setup, xml and general programming.

This document aims to explain the details of setting up Maria GDK map packages for Vector and Raster Maps, and how map packages can be combined using a MariaGDK map template to display a multilayered map.

`<WRAP center round info 80%>`
The information is organized in tables with columns for element/attribute name, description and element properties. Valid properties are:
 | **Property** | **Description**     | 
 | ------------ | ---------------     | 
 | O            | Is optional.        | 
 | R            | Is repeating.       | 
 | C            | Has child elements. | 
 | A            | Has attributes.     | 
`</WRAP>`

## Locating map files

The Vector and Raster map services searches for Maria GDK map-files in the default folder provided at installation, but can also search for data in other locations pointed to by link.xml-files. \\
Read [Configuring Link-xmls](maria_gdk/maps/config/links) for more details.

## Setting up map templates

The map template xml joins together map data from different sources to create a multilayered, rich map for Maria GDK. A template can contain multiple layers referencing mapsignatures from both raster and vector mapservices. Layers can also act as modification layers (for f.ex. elevation shading effects) and placeholder layers (for special data like WMS and propagation). Templates are created and edited using [Maria Map Maker](./config/mariamapmaker).\\

Read [Details Map Templates](maria_gdk/maps/config/Templates) for more information on the xml structure of the templates

## Configuring vector maps

Read [Configuring Vector Maps](./config/vector) for more details.

## Configuring raster maps

Read [Configuring Raster Maps](./config/raster) for more details.

## Adding metadata

Read [Details metadata](./config/metadata) for more information.

## Adding bookmarks

Read [Details bookmarks](./config/bookmarks) for more information.

## Adding feature info lookup tables

Read [Details feature info lookup tables](./config/lookuptables) for more information.
