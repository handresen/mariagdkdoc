---
title: N50 data translation
keywords: maps GDK
tags: [map, vector]
sidebar: core_sidebar
permalink: core_map_preprocess_n50.html
summary: Using FME to translate the N50 vector dataset from Kartverket.
---

{% include callout.html content="Under construction." type="warning" %}

This example will demonstrate how to use FME to translate the N50 vector dataset from Kartverket. With the amount of data in this set, the most sensible structure to produce is one m6m dataset for each county in Norway. 
In addition to the files specified [here](./core_map_preprocess_fme.html), the requirement for this operation is the plugin GeoSOSI Professional for FME, which is developed by Geodata.

## Reader

Begin by adding a reader to your workspace. Choose the SOSI format, and use the advanced browser to add the folder containing the N50 dataset. Tick "Subfolders". Select the coordinate system option "Read from source", and the workflow option "Single Merged Feature Type". Double-click the reader, go to Format Attributes, and tick fme_basename. This will pass the SOSI filename to the next module. Because the attributes in the SOSI-files does not necessarily adhere to the SOSI standard, it might be necessary to lower the Attribute validation level (under reader parameters in the navigator pane) to Basic.

## Transformer

To produce an m6m dataset for each county, it is necessary to first produce an m6m file for each feature type, in one folder for each county. If the N50 dataset is structured with one SOSI file for each municipality, we need to extract the county number from the filename. The SOSI filenames contain the county number in the first two characters. Add the SubstringExtractor transformer to your workspace and connect it to the reader. Set Source Attribute to fme_basename, start and end index to 0 and 2, and name the result attribute something descriptive, like "countynumber".

## Writer

Add a writer to your workspace, and choose the M6M format. Specify a location for the output, choose coordinate system EPSG:4326, and select Workflow Option "Dynamic Schema". Connect the writer to the transformer. To get a folder of m6m files for each county, the writer needs to fan out on the attribute you created in the previous step. In the navigator pane, go to the advanced parameters for the writer and set Fanout Dataset: Yes. Attribute to fanout on should be the attribute you created with the transformer.

The workspace should look similar to this before you run it:
{% include image.html file="core/preprocess/n50_translate.png" %}

## Indexing

Indexing. Same procedure as with [M6M indexing](./core_map_preprocess_m6m_indexing.html) from any vector source.
