---
title: Map templates
keywords: templates, dynamic, elevation, composite, overlay
tags: [map, template]
sidebar: core_sidebar
permalink: core_maps_templates.html
summary: Using map templates to create multilayered maps for Maria GDK. 
---

Older Versions: [Maria GDK 1.5 and older](./core_maps_templates_v1.5.html)

The map template xml joins together map data from different sources to create a multilayered, rich map for MariaGDK. A template can contain multiple layers referencing mapsignatures from both raster and vector mapservices. Layers can also act as modification layers (for example elevation shading effects) and placeholder layers (for special data like WMS and propagation).

Map templates are provided to the MariaGDK client application via the MariaGDK Map Catalog service addin. Templates are searched for in the path specified in the "Templatesettings"-section of the TpgServiceHoster settings.json file. 
[Links](./core_maps_links.html)-files can be used to point to yet other folders containing templates.

[Explaining the map template xml](./core_maps_templates_xml.html)

Example:

```xml
<compositemaptemplate id="1ff8e35e-5690-449e-b0e4" name="Test-Norge" version="0.0.127">
  
  <layer type="MapLayer" name="OpenstreetMap">
    <opacity>1</opacity>
    <brightness>0</brightness>
    <gamma>1</gamma>
    <contrast>0</contrast>
    <grayscale>false</grayscale>
    <minscalevisible>244</minscalevisible>
    <maxscalevisible>128000000</maxscalevisible>
    <datasource>
      <mapsignature />
      <maptype>WmsMap</maptype>
      <showlabels>false</showlabels>
      <usecache>true</usecache>
    </datasource>
    <visible>true</visible>
    <property key="serviceType" value="OpenstreetMap" />
    <property key="labels:fetchdata" value="false" />
  </layer>
  
  <layer type="MapLayer" name="norge">
    <opacity>1</opacity>
    <brightness>0</brightness>
    <gamma>1</gamma>
    <contrast>0</contrast>
    <grayscale>false</grayscale>
    <minscalevisible>244</minscalevisible>
    <maxscalevisible>32000000</maxscalevisible>
    <datasource>
      <mapsignature>norgen50-n5000</mapsignature>
      <version min="1.0.0" max="1.99.99" releasetype="Release" versiontype="Newest"/>
      <maptype>VectorMap</maptype>
      <showlabels>true</showlabels>
      <usecache>true</usecache>
    </datasource>
    <visible>true</visible>
    <property key="labels:fetchdata" value="true" />
  </layer>
  
  <layer type="ModLayer" name="Normalmapelevations" category="ElevEffects">
    <opacity>1</opacity>
    <brightness>0</brightness>
    <gamma>1</gamma>
    <contrast>0</contrast>
    <grayscale>false</grayscale>
    <compression>Any</compression>
    <minscalevisible>244</minscalevisible>
    <maxscalevisible>128000000</maxscalevisible>
    <description>Elevation shading layer based on prerendered normal maps</description>
    <datasource>
      <mapsignature>norway_normals20m</mapsignature>
      <maptype>RasterMap</maptype>
      <showlabels>true</showlabels>
      <usecache>true</usecache>
    </datasource>
    <visible>true</visible>
  </layer>
  
  <bookmark name="Oslo" lat="59.87" lon="10.64" scale="15000" mapsignature="norgen50-n5000" default="true" />
 
</compositemaptemplate>
```

## Hill shading layers

Available since Maria GDK 3.1.

In the above example, we had a pregenerated set of normal maps for hillshading. If you have elevation data active in the application you can generate the hill shading automatically. To do this, add a ModLayer as above, but with an "ElevationNormals" maptype data source. The map signature value is not significant. 

**Example:**

```xml

  <layer type="ModLayer" name="Auto shading" category="ElevEffects">
    <opacity>1</opacity>
    <brightness>0</brightness>
    <gamma>1</gamma>
    <contrast>0</contrast>
    <grayscale>false</grayscale>
    <compression>Any</compression>
    <minscalevisible>1</minscalevisible>
    <maxscalevisible>2000000</maxscalevisible>
    <datasource>
      <mapsignature>AutoShading</mapsignature>
      <maptype>ElevationNormals</maptype>
      <showlabels>false</showlabels>
      <usecache>false</usecache>
    </datasource>
    <shadingparameters>
        <ambient>0.6</ambient>
    </shadingparameters>
    <visible>true</visible>
    <property key="labels:fetchdata" value="false" />
  </layer>
```


## Elevation data templates

Templates can also be used to define a set of elevation data layers used for analysis purposes, such as elevation profiles etc.

These elevation layers are processed in the order they appear in the template file, in such a manner that the first layer that returns a valid result is used. This means that you typically want to place datasets with higher resolution and smaller area first in the file. If your query falls within the high resolution subdataset, you will get result from these data, and if not, it will fall through to the next layer which may for example be a global base elevation grid.

These templates are typically set programmatically through the IElevationData interface.

In the following example, we have a 10m resolution elevation data set that covers Norway first in the template, 
and a global SRTM dataset as fallback.

**Example:**

```xml

<?xml version="1.0" encoding="utf-8" standalone="yes"?>
<compositemaptemplate xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" id="FBE924BD-3D90-4E0C-AA62-C6346DAB6829" name="World Panorama v1.5" version="0.0.1">
  <layer type="ElevationLayer" name="Norge 10m">
    <compression>Any</compression>
    <datasource>
      <mapsignature>elevation10m</mapsignature>
      <maptype>ElevationData</maptype>
      <usecache>false</usecache>
    </datasource>
    <visible>true</visible>
  </layer>
  <layer type="ElevationLayer" name="SRTM30">
    <compression>Any</compression>
    <datasource>
      <mapsignature>globe_elevation</mapsignature>
      <maptype>ElevationData</maptype>
      <usecache>false</usecache>
    </datasource>
    <visible>true</visible>
  </layer>
</compositemaptemplate>


```

## Dynamic layers in templates

Available since Maria GDK 3.1.+.

It is possible to dynamically build up templates using overlay-templates and corresponding tags. This is done by creating a (basemap) template containing references to (overlay) templates. (Overlay-templates use the same syntax as regular templates, but are NOT regarded as "normal" templates by Maria GDK). The purpose is to ease the process of replacing and adding maps to a basemap without continously having to update the template.xml.

First you need to add [templatefilters](./core_maps_templates_xml.html#templatefilter-only-maria-gdk-31) (with tags for referencing) in overlay-templates.

```xml
<compositemaptemplate name="testOverlay" id="0CB9BB2B-3C4E-4922-889B-7442E683C2AB">
  <templatefilter tags="groundraster" pri="500"/> <!-- tag this overlay -->
  <layer type="OverlayLayer" name="OpenstreetMap">
    <opacity>1</opacity>
    <brightness>0</brightness>
    <gamma>1</gamma>
    <contrast>0</contrast>
    <grayscale>false</grayscale>
    <compression>Any</compression>
    <minscalevisible>100</minscalevisible>
    <maxscalevisible>128000000</maxscalevisible>
    <datasource>
      <mapsignature>wms</mapsignature>
      <maptype>WmsMap</maptype>
      <hostcategory>DontCare</hostcategory>
      <showlabels>false</showlabels>
      <usecache>false</usecache>
    </datasource>
    <visible>true</visible>
    <property key="serviceType" value="OpenstreetMap" />
    <property key="osMapStyle" value="Standard" />
  </layer>
  </compositemaptemplate>
```

Then add templatefilters in basemap-templates to reference maplayers or datasources in overlay-templates.

Either <a name="en">(exI)</a>:

```xml
<compositemaptemplate name="testTemplateRef" id="5497D2A9-25C6-429B-AA9A-0ADAB98721C0">
  <!-- reference all layers in overlays tagged with groundraster and seaoverlay-->
  <templatefilter refs="groundraster, seaoverlay"/> 
</compositemaptemplate>
```
or <a name="to">(exII)</a> (only available since Maria GDK 3.1.1(?)):

```xml
<compositemaptemplate name="TestDatasourceRef" id="5497D2A9-25C6-429B-AA9A-0ADAB98721C0">
  <layer type="MapLayer" name="Groundrasters"> 
    <.../>
    <datasource>
      <!-- reference all overlays tagged with groundraster but include only datasource information -->
      <templatefilter refs="groundraster"/> 
    </datasource>
    <visible>true</visible> 
  </layer>
</compositemaptemplate>
```

[Completely expanded xml-example for dynamic templates.](./core_maps_templates_dynamic.html)

When Maria GDK is loading the basemaptemplate - all available overlay-templates with a referenced tag will be included. Templatefilters located outside the layer-block(s) [(exI)](#en) will include all maplayers from the referenced overlays. Templatefilters located inside the datasource-block [(exII)](#to) will include only the datasouce-blocks (from the referenced overlays).

Overlay-templates will also provide a prioritynumber used for sorting the overlay-templates when including them in the basemap. Lower number equals higher priority. Default value is 100.

**Note:** map layers specified directly in the basemap-template will always be placed first in the completed maptemplate and thus drawn first (lowest priority layers).

**Also note:** The ordering of maplayers internally in a overlay-template will not be disturbed when included into a basemap via tag references.


