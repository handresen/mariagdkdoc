---
title: Producing m6m files using OGR
keywords: maps GDK
tags: [map, vector]
sidebar: core_sidebar
permalink: core_map_preprocess_ogr.html
summary: Using Teleplan Globes OGR plugin to translate map data to m6m-format.
---
OGR is part of the well known Open Source library GDAL. For general documentation on OGR go to [www.gdal.org](http://www.gdal.org).

Translating map data to m6m-format using the OGR set of tools is accomplished using a Teleplan Globe developed plugin. It is used to output spatial data as MARIA m6m-files from all the input formats supported by OGR. Teleplan Globe also provides a small executable for indexing and converting m6m-files to datasets.

## Setup

The OGR plugin consists of a single file, ogr_M6M.dll. To make use of the plugin you have to complete the following install procedure.

 1.  In the folder of all OGR executables create a directory called gdalplugins.
 2.  Copy ogr_M6M.dll to the directory created in step 1.
 3.  Check the installation by running the command under should show the M6M as a supported format.

```bash
ogrinfo.exe --formats
...
-> "M6M" (read/write)
...
```

##  Translation 

To translate data from a OGR supported format to the m6m format we use the ogr2ogr.exe utility of OGR. The result of running ogr2ogr.exe will be a .m6m file for each dataset given to as the source dataset. The complete reference of ogr2ogr.exe can be found at [ogr2ogr reference](http://www.gdal.org/ogr2ogr.html). <br/>
The following example translates a Shape file to the M6M format.

```bash
ogr2ogr.exe -f M6M MyDestinationDataDirectory\ MyShape.shp
```

Note that MyDestinationDataDirectory has to exists before running the command. Also note the backslash after the destination data directory declaration.

MyDestinationDataDirectory should now have one or more .m6m files. These are the input to the next step in the conversion pipeline, [M6M indexing](core_map_preprocess_m6m_indexing.html).

