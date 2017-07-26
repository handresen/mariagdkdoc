---
title: Offscreen Maria layer export
keywords: maps, export
tags: [map]
sidebar: core_sidebar
permalink: core_maps_export_offscreen.html
summary: Offscreen bitmap export. 
---

The offscreen export mode requires the user to set up the layers that are to be exported using data classes provided by Maria GDK.
The entry point for this is [OffscreenRasterExportFactory](http://support.teleplanglobe.com/MariaGDKDoc/html/BAA96374.htm) from which you can get the actual [IOffscreenRasterExporter](http://support.teleplanglobe.com/MariaGDKDoc/html/53429A03.htm) implementation that provides the api for adding layers to export.

The export process itself is asynchroneous and reports its progress using events.

When the export has finished you can retrieve the actual exported bitmap using the property [ExportedImage](http://support.teleplanglobe.com/MariaGDKDoc/html/6EAB7FA0.htm)
