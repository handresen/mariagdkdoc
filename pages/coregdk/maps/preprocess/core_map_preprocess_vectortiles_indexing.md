---
title: Vector tile indexing
keywords: maps GDK
tags: [navigation]
sidebar: core_sidebar
permalink: core_map_preprocess_vectortiles_indexing.html
summary: Using Teleplan Globes command line tool to convert m6m files to vector tiles.
---

# Vector tile indexing 

The FME and OGR translation processes described in this wiki produce m6m files. These must be indexed and converted to datasets or vector tiles before they can be used in MARIA. Conversion to vector tiles is done with the command line tool "ToVectorTileset.exe", which is distributed by Teleplan Globe. 

Use as follows: Open a command-prompt and navigate to the folder where "ToVectorTileset.exe" is located. Type: 

```bash
tovectortileset `<source folder>` `<target filename>` `<-level n>` [-recursive]
	[-zip][-attribs "a1,a2,..,an"]
	[-lborder <value>][-aborder <value>]
	[-pborder <value>] [-namedborder<i> <layer name> <value>]
```
Note that the target folder must exist prior to execution. Use ''-recursive'' if you want to go through subfolders as well.

"-zip" compresses database blob data using zlib compression

"-attribs" is used if only specific feature attributes should be included. Only including the required attributes can substantially reduce target filesize.

Border values in tile coords, 4096 is a full tile. l(ine)\|a(rea)\|p(oint) borders define defaults for feature type, named layer borders override these.

Names can contain wildcards '?' and '*'

Example:

```bash
tovectortileset c:\sourcemaps\lvl10 c:\targetmaps\mymap.vtiles.sqlite -level 10
-attribs "name,type,geotype,date" 
-pborder 512 -namedborder1 byg* 0 -nameborder2 *light* 4096
```

### Level

The "level"-parameter is a number that describes the tile level and roughly corresponds to a map scale where level 0 covers the whole earth at 1:256M and each level means scale/2, see table. 

 | Level | Scale        | Comment                                | 
 | ----- | -----        | -------                                | 
 | 0     | 1:256M       | Whole earth between 85N and 85S        | 
 | 1     | 1:128M       | 256M/2`<sup>`level`</sup>`                 | 
 | 2-18  | 1:64M-1:1000 | Level 18 roughly corresponds to 1:1000 | 

The level parameter should be selected so that it matches typical display scales for the map data. If the same map data is to be used on several levels, it is possible to run multiple conversions with different level values in order to produce tilings for different levels.

Note that the vector tile files are sqlite databases and can be analyzed using apps such as SqliteBrowser or the command line tool sqlite3.exe that is located in the same folder as ToVectorTileset.exe.

To determine the average block of celldata, use this SQL-statement:

```sql
select avg(length(tile_data)) from tiles;
```

As a rule of thumb, average tile size should range between 1000 bytes and 50000 bytes and max tile size should be less than 1MB. In the end, the tile level must be tuned based on experience and intended map data scales along with map previews. Increasing the level will tend to increase total data size while decreasing size per tile.

### Merging

```bash
ToVectorTileset.exe -merge `<target filename>` `<source filename>`
```
Merge tiles from database given by source filename into database given by target filename. Overlapped tiles are 
supported.
Blob-compression is used only if target database uses blob-compression.

### Make boundaries

Generate boundary:

```bash
ToVectorTileset.exe -makebounds `<db filename>`
```

### Compacting

To compress geodata using zlib, call

```bash
ToVectorTileset.exe -compact `<db filename>`
```
Depending on data, this should compress the database to approximately 0.5-0.7 times the original size. Note that vector service shipped with MARIA GDK 2.1 or newer is required. Note that vacuuming and unused tile data removal is also performed as part of the compression.

{{tag>[m6m] [m6mdataset] [indexing] [convert]}}


