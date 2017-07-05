---
title: Vector map legend
keywords: vector, legend
tags: [map, vector]
sidebar: core_sidebar
permalink: core_maps_vectorlegend.html
summary: Displaying legend for vector maps.
---

The vector map service can provide bitmaps representing all visible map themes in current view (S52 and themes with only label representations are ignored).

{% include callout.html content="When running with multiple mapservices serving multiple instances of the same mappackage, make sure that all mappackages reference an identical set of setup files (featuregrouping.xml, mariner_settings.xml etc). If not - legend bitmaps might differ from what is visible in map view." type="info" %}

### Collecting legend data information

When the map client request a tile, information about which themes are visible are collected from the map service and embedded in the generated map reply tile. Legend data is then collected from the tiles and merged in the serviceclient. They are then made available via the IMapLayerViewModel as a list of legenddata.

IMapLayerViewModel:

```csharp
/// <summary>
/// Collect legend data information from all vector map layers in view
/// </summary>
/// <returns>List of legend data information in current view</returns>       
List<IMapLegendData> GetMapLegendData();
```

GetMapLegendData returns a list of legenddata information that can be used to extract bitmaps from the vector map service.

```csharp
/// <summary>
/// Returns merged legend data for all tiles in view
/// </summary>
public interface IMapLegendData
{
    /// <summary>
    /// Merged legend data information collected from tiles.
    ///
    /// Top level in dictionary is mapsignature, then datasetid, geometry type and 
    /// ending with a list of featuregroups.
    /// </summary>
    Dictionary<string, 
       Dictionary<string, 
          Dictionary<GeometryType, HashSet<string>>>> Legends { get; } 
}
```

### Requesting legend bitmaps

Use the information collected from GetMapLegendData to build a RasterResourceRequest. 

```csharp
/// <summary>
/// Fetch legend resource bitmap for single theme
/// </summary>
/// <returns>Bitmapsource representing single legend theme</returns>
BitmapSource GetMapLegendResources(RasterResourceRequest request);
```

The RasterResourceRequest should supply a resourceid-string with syntax: <br/> *[mapsignature]*#*[dataset]*#*[featuregroup]*.

Example:

{% include callout.html content="
*resourceid:* Norge2013#Norge N250#Arealdekke\Skog <br/>
*geotype:* Polygon <br/>
*rastersize:* Medium <br/><br/>

Will return a medium sized bitmap representing a skog-polygon as defined by the \"Norge N250\" dataset referred to by the \"Norge2013\" mappackage." type="info" %}

```csharp
enum GeometryType
{
	UnknownType=0; 
	Point=1;
	LineString=2; 
	Polygon=3;
}

enum RasterSize
{
	Small=0;
	Medium=1;
	Large=2;
}

message RasterResourceRequest
{
     required string resourceid=1;
     optional GeometryType geotype=2 [default=LineString];
     optional RasterSize rastersize=3 [default=Small];
}
```

