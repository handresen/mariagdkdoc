---
title: Metadata
keywords: metadata
tags: [map]
sidebar: core_sidebar
permalink: core_maps_metadata.html
summary: Provide map metadata using xml-files. 
---

Metadata is collected from xml-files. These files should be located together with the map data they relate to (mappackage, multidataset or dataset) and use the extension .metadata.xml (f.x. N5000.m6mmultidataset.metadata.xml).

Example-xml:

```xml
<?xml version="1.0" encoding="utf-8"?>
<metadata>
  <embedded>
    <classification>Unclassified</classification>
    <!-- yyyy-mm-dd iso8601-->
    <expiration>2105-05-05</expiration>
    <level>dataset</level>
    <desc>N5000</desc>
  </embedded>
  <!-- User defined elements -->
  <generic>
    <desc>Vector map data Norway.</desc>
    <scale>N5000</scale>
  </generic></metadata>
```

##  Embedded 

When a tile is requested from the map service, embedded metadata for that tile is included in the generated map reply tile. Embedded metadata should be compact. \\
Embedded metadata are collected from datasets visible in the current view, but due to large boundingboxes, metadata related to data not directly visible may be returned.

IMapLayerViewModel:        

```csharp
/// <summary>        
/// Extract embedded map metadata for current map view        
/// Unique name/value-combinations are stored separately        
/// </summary>        
/// <returns>Merged map metadata for all visible tiles</returns>        
IMapMetadata GetMapMetadata();
```

## Generic

Generic metadata can be requested from the map catalog service when needed. Use generic metadata for more extensive/descriptive  information related to the map. \\
Generic metadata can be requested via mapsignature. If multiple services serve the same mapsignature, we assume that the metadata is the same and metadata from the first service found will be returned.

IMapCatalogService:

```csharp
/// <summary>        
/// Get generic metadata information for requested mapsignature.        
/// </summary>        
/// <param name="mapSignature">Map signature string</param>        
/// <returns>Metadata entries or null if not found.</returns>        
/// If identical mapsignatures are  provided from multiple services,
/// one is chosen at random        
[OperationContract]        
MetadataInfo GetGenericMetadata(string mapSignature);
```

## Metadata in vector maps

Vector maps can provide two kinds of metadata, embedded and generic. \\
Generic metadata are collected from mappackage and multidataset-xml's (not dataset). \\
Embedded metadata are collected from mappackage, multidataset and dataset-xml's. 

## Metadata in raster maps

Raster maps can provide generic and embedded metadata. \\
Generic metadata for raster maps is collected from mappackages.\\
Embedded metadata are collected from mappackage and raw data files.

Note: metadata-files for mappackages referes to single mappackage entries in rastermappackages.xml. 

A rastermappackages-file with two mappackages should have two separate metadatafiles - one for each package.

Example test.rastermappackages.xml:

```xml
<mappackages>		
  <mappackage signature="BlueMarble">
    <rastermapdataset id="Blue Marble">
      <dataset path="Raster\BlueMarble2012\GeoTiff\bm2012.vrt"/>
    </rastermapdataset>
  </mappackage>
  <mappackage signature="OsloRaster">
    <rastermapdataset id="Oslo Raster">
      <dataset path="Raster\Oslo\MrSID\Oslo_2008_2_utm32_1.sid"/>
    </rastermapdataset>	
  </mappackage>
</mappackages>
```

Should have two metadatafiles named:

BlueMarble.rastermappackages.metadata.xml \\
OsloRaster.rastermappackages.metadata.xml

Metadata for raw data files should be contained in a xml with filename `<datafile without extension>.rasterdata.metadata.xml`

