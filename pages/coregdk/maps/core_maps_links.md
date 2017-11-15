---
title: Using links-files
keywords: links, setup, vector, raster
tags: [map]
sidebar: core_sidebar
permalink: core_maps_links.html
summary: Use links.xml-files to point to secondary file locations. 
---

The Vector and Raster Map services searches for Maria GDK map files in the default folder provided at installation. Map packages can also be located in secondary locations using links.xml-files pointing to the data.

{% include callout.html content="Avoid circular references in link.xmls." type="warning" %}

File containing links should be named *.links.xml.

Example maps.links.xml:

```xml
<links>
  <link path="H:\Maps\" recursiondepth="3"/>
  <link path="C:\servicetest\kart\"/>
</links>
```

## Links

 
 | Child element | Description                                           | Properties | 
 | ------------- | -----------                                           | ---------- | 
 | link          | Link to location containing mappackages and datasets. | O R A      | 

## Link

 | Attribute | Description                                           | Properties | 
 | ------------- | -----------                                           | ---------- | 
 | path          | Path to location. |       |
 | recursiondepth | Number of levels to recurse down the directory tree. Default value is 0. | O | 
