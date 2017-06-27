---
title: Producing m6m files using FME
keywords: maps GDK
tags: [navigation]
sidebar: core_sidebar
permalink: core_map_preprocess_fme.html
summary: Using Teleplan Globes FME plugin to translate map data to m6m-format
---

# FME

Translating map data to m6m-format using FME is accomplished using a Teleplan Globe developed plugin. It is used to output spatial data as MARIA m6m-files from all the input formats supported by FME. M6MFormat is a writer-only plugin, and it is available for both 32 and 64 bit versions of FME. Teleplan Globe also provides a small executable for indexing and converting m6m-files to datasets.


## Setup

The plugin consists of three files, which have to be placed in the correct subdirectories of the FME installation:

*  m6mformat.dll → [..]\FME\Plugins\
*  m6mformat.db  → [..]\FME\formatsinfo\
*  m6mformat.fmf → [..]\FME\metafile\

## Translation

Start FME workbench and choose "Blank Workspace" to manually add readers and writers from the toolbar. Add a reader, specify the format and location of your input data, and select "Single Merged Feature Type".  

{% include image.html file="./core/preprocess/fmereader.png" %}

Add a writer, select the "Teleplan Globe MARIA m6m" format, and specify a location to create the dataset. The coordinate system must be set to EPSG:4326 (WGS84 lat/lon), and "Dynamic Schema" selected.

{% include image.html file="./core/preprocess/fmewriter.png" %}

Finally, connect the reader and writer, and run the translation. 

## Indexing

The resulting m6m files must be indexed before they can be used in MARIA. For info on how to index m6m files manually using command line, read [M6M indexing.](./core_map_preprocess_m6m_indexing.html) 
Indexing can also be done automatically in FME using a shutdown Python script. The script requires that the location of "tom6mdataset.exe" is included in the Windows system environment variables path. Also, if dataset fanout is used in the workspace, the fanout folder must be linked to the 'DestDataset_M6MFORMAT' parameter.  In the navigator window in FME, click Workspace Parameters → Advanced → Shutdown Python Script, and enter the following Python code:

```python
import fme
import os

outpath = fme.macroValues['DestDataset_M6MFORMAT']

outstring = 'tom6mdataset %s %s\ -recursive' % (outpath,outpath)
print outstring
os.system(outstring)
```

The resulting m6m dataset will be created in the location set in the writer, or in the folder one level up if dataset fanout is used.





{{tag>[Map] [FME] [convert] [m6m]}}










