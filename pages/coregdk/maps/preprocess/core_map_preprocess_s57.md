---
title: S-57 Hydrographic Data Translations
keywords: maps GDK
tags: [navigation]
sidebar: core_sidebar
permalink: core_map_preprocess_s57.html
summary: Using FME to translate S-57 data to m6m files
---

# S-57 Hydrographic Data Translations


## Step 1

### ENC
Start FME workbench and open the supplied "S-57.fmw" workspace. Included in the translation from S-57 ENC data to M6M are two FME transformers - a selection filter and a coordinate extractor. Their task is to extract depth values from geometries referenced by S-57 SOUNDG records and put these values in a seperate attribute field, ZDEPTH, added for the purpose.   

Select 'Edit Parameters...' from the Tools menu.

{% include image.html file="core/preprocess/translation_parameters.png" %}

Use the navigation buttons or edit the content of 'Source S-57 (ENC) Hydrographic Data File(s):' - field to point to the location(s) of your ENC source data.

FME supports updates, files with extension .001, .002  ... containing successive corrections to a .000 file. For this to work, it is necessary that the updates are located in the same directory as the corresponding .000 file.

Use the navigation button or edit the content of the 'Fanout Directory: - field to point to a desired location for the output from this stage of the process. Note: Since we are using 'Fanout' with respect to dataset names (S-57 cell names) and thus explicitly stating a top level target folder, the 'Destination m6mformat Directory:' - parameter is immaterial.

Click 'OK' and run the translation process.

### AML

For AML data, the workspace "AML.fmw" is used instead of "S-57.fmw". Otherwise all instructions in this guide can be applied.

## Step 2

Drawing of ENC charts is complex and needs specifically designed renderers. To assist drawing and minimize subsequent cross object lookups the output from the previous stage must pass through an additional filter. This is done with a separate command line utility "S57filter.exe". Do as follows:

 1.  Open a command line tool and navigate to the folder where “S57filter.exe” is located.
 2.  Type: ''S57filter.exe `<source folder>` `<target folder>` [-r]''

The `<source folder>` is the output folder from the previous stage. Use the '-r' option to traverse subdirectories.

## Step 3

Create vectortiles. Use the script buildvectortiles_ENC.ps1 on each of the datasets. Use the following zoomlevels:

 | Dataset  | Band | Zoomlevel | 
 | -------  | ---- | --------- | 
 | Overview | 1    | 3-14      | 
 | General  | 2    | 10-13     | 
 | Coastal  | 3    | 10-13     | 
 | Approach | 4    | 13-17     | 
 | Harbor   | 5    | 14-17     | 
 | Berthing | 6    | 14-17     | 


