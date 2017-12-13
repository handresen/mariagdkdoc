---
title: TileMapBuilder command line tool
keywords: raster, mbtiles
tags: [map, raster]
sidebar: core_sidebar
permalink: core_raster_tilemapbuilder.html
summary: Producing MBTiles database from a Maria map template. 
---

The TileMapBuilder command line tool can be used to produce an MBTiles database from a Maria map template. 

### Usage

    TileMapBuilder [options]  <template> <mbtiles>

### Options

 | `--bb <minLat> <minLon> <maxLat> <maxLon>` | Bounding box in geographical coordinates.                                | 
 | `--append`                                 | Append to an existing MBTiles database.                                  | 
 | `--minlevel <level>`                       | Minimum quad tree level. Default is 0.                                   | 
 | `--maxlevel <level>`                       | Maximum quad tree level. Default is 0                                    | 
 | `--buffer <pixels>`                        | Buffer to include around each tile. Use this on vector maps with labels. | 
 | `--catservice <URL>`                       | Catalog service URL. Default is http://localhost:9008/catalog            | 
 | `--tplservice <URL>`                       | Template service URL. Default is http://localhost:9008/maptemplates      | 
 | `--format <format>`                        | Tile image format: 'png' or 'jpg'. Default is 'png'                      | 
 | `--quality <integer>`                      | JPEG quality (0-100). Default 85                                         | 
 | `--name <name>`                            | MBTiles dataset name. Default is the same as template name.              | 
 | `--desc <description>`                     | MBTiles dataset description                                              | 
 | `--version <version string>`               | MBTiles dataset version. Default "1.0"                                   | 

### Examples

Building a global dataset with high resolution insert over Oslo

    TileMapBuilder --maxlevel 8 "World Panorama v1.5" panorama_oslo.mbtiles
    TileMapBuilder --append --buffer 100 --minlevel 9 --maxlevel 15  --bb 59.85 10.5 60.0 11.0 Norge-vtiles panorama_oslo.mbtiles

