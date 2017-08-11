---
title: Dynamic template example
keywords: templates, dynamic, overlay, templatefilter
tags: [map, template]
sidebar: core_sidebar
permalink: core_maps_templates_dynamic.html
summary: Demonstrating dynamic templates 
---

## Setting up the example

Say you have four overlays defined: [OL:Plain](#olplain), [OL:Special](#olspecial), [OL:AlsoSpecial](#olalsospecial) and [OL:Peaceful](#peaceful). Overlay-xml-syntax is the same as for a "normal" template, but these overlays include a `<templatefilter>` used for tagging. [OL:Plain](#olplain) is tagged with "wms", [OL:Special](#olspecial) is tagged with "specialground", [OL:AlsoSpecial](#olalsospecial) is tagged with "specialground" and "beware" and [OL:Peaceful](#peaceful) is tagged with "nothingtoseehere".

### OL:Plain

```xml
<compositemaptemplate id="x" name="OL:Plain" version="0.0.127">
<templatefilter tags="wms" pri="500"/> <!-- tagged -->
  <layer type="MapLayer" name="PlainOpenstreet">
    <opacity>1</opacity>
    <brightness>0</brightness>
    <gamma>1</gamma>
    <contrast>0</contrast>
    <grayscale>false</grayscale>
    <minscalevisible>244</minscalevisible>
    <maxscalevisible>128000000</maxscalevisible>
    <datasource>
      <mapsignature>openstreet</mapsignature>
      <maptype>WmsMap</maptype>
      <showlabels>false</showlabels>
      <usecache>true</usecache>
    </datasource>
    <visible>true</visible>
    <property key="serviceType" value="OpenstreetMap" />
    <property key="labels:fetchdata" value="false" />
  </layer>
</compositemaptemplate>
```

### OL:Special

```xml
<compositemaptemplate id="y" name="OL:Special" version="0.0.127">
  <templatefilter tags="specialground" pri="250"/> <!-- tagged -->
  <layer type="MapLayer" name="SpecialInfo">
    <opacity>1</opacity>
    <brightness>0</brightness>
    <gamma>1</gamma>
    <contrast>0</contrast>
    <grayscale>false</grayscale>
    <minscalevisible>1000</minscalevisible>
    <maxscalevisible>100000</maxscalevisible>
    <datasource>
      <mapsignature>scary</mapsignature>
      <maptype>VectorMap</maptype>
      <showlabels>true</showlabels>
      <usecache>true</usecache>
    </datasource>
    <visible>true</visible>
    <property key="labels:fetchdata" value="true" />
  </layer>
</compositemaptemplate>
```

### OL:AlsoSpecial

```xml
<compositemaptemplate id="z" name="OL:AlsoSpecial" version="0.0.127">
  <templatefilter tags="specialground, beware" pri="150"/> <!-- tagged -->
  <layer type="MapLayer" name="VerySpecialInfo">
    <opacity>1</opacity>
    <brightness>0</brightness>
    <gamma>1</gamma>
    <contrast>0</contrast>
    <grayscale>false</grayscale>
    <minscalevisible>200</minscalevisible>
    <maxscalevisible>2000000</maxscalevisible>
    <datasource>
      <mapsignature>danger</mapsignature>
      <maptype>VectorMap</maptype>
      <showlabels>true</showlabels>
      <usecache>true</usecache>
    </datasource>
    <visible>true</visible>
    <property key="labels:fetchdata" value="true" />
  </layer>
</compositemaptemplate>
```

### OL:Peaceful

```xml
<compositemaptemplate id="xyz" name="OL:Peaceful" version="0.0.127">
  <templatefilter tags="nothingtoseehere" pri="10"/> <!-- tagged -->
  <layer type="MapLayer" name="Peaceful">
    <opacity>1</opacity>
    <brightness>0</brightness>
    <gamma>1</gamma>
    <contrast>0</contrast>
    <grayscale>false</grayscale>
    <minscalevisible>1</minscalevisible>
    <maxscalevisible>4M</maxscalevisible>
    <datasource>
      <mapsignature>bliss</mapsignature>
      <maptype>VectorMap</maptype>
      <showlabels>false</showlabels>
      <usecache>true</usecache>
    </datasource>
    <visible>true</visible>
  </layer>
</compositemaptemplate>
```

You should also have a basemaptemplate. This is the template that Maria GDK will relate to.


## Basemap example 1 (referencing complete layers):
The following template contains only a templatefilter referencing tags "wms" and "specialground". 

```xml
<compositemaptemplate id="xyz" name="BM:Complex" version="0.0.127">
  <templatefilter refs="wms, specialground"/>
</compositemaptemplate>
```

Maria GDK will analyze all available overlays and include only layers from those tagged with either "wms" or "specialground" - in this case [OL:Plain](#olplain), [OL:Special](#olspecial) and [OL:AlsoSpecial](#olalsospecial). The result template will contain three separate layers: `PlainOpenstreet`, `SpecialInfo` and `VerySpecialInfo`. The templatefilter-priority argument will decide how the layers are ordered. (Overlays with multiple layers will keep its internal ordering.)

## Basemap example 2 (referencing only datasources):
The following template contains filters inside the datasource-blocks. This makes it possible to include multiple datasources in one layer and control them as one. Only datasource-information will be extracted from the overlays. All other layer-parameters (as opacity, max/minscale etc) will be discarded and replaced by the parameters in the referencing basemaplayer. 

```xml
<compositemaptemplate id="xyz" name="BM:Complex" version="0.0.127">
  <layer type="MapLayer" name="SingleTag">
    <opacity>1</opacity>
    <brightness>0</brightness>
    <gamma>1</gamma>
    <contrast>0</contrast>
    <grayscale>false</grayscale>
    <minscalevisible>5000</minscalevisible>
    <maxscalevisible>8000000</maxscalevisible>
    <datasource>
      <templatefilter refs="wms"/>
    </datasource>
    <visible>true</visible>
    <property key="serviceType" value="OpenstreetMap" />
    <property key="labels:fetchdata" value="false" />
  </layer>
  
  <layer type="MapLayer" name="MultiTag">
    <opacity>1</opacity>
    <brightness>0</brightness>
    <gamma>1</gamma>
    <contrast>0</contrast>
    <grayscale>false</grayscale>
    <minscalevisible>244</minscalevisible>
    <maxscalevisible>32000000</maxscalevisible>
    <datasource>
      <templatefilter refs="specialground"/>
    </datasource>
    <visible>true</visible>
    <property key="labels:fetchdata" value="true" />
  </layer>
   
</compositemaptemplate>
```

After analyzing the available overlays, only those tagged "wms" or "specialground" will be included ([OL:Plain](#olplain), [OL:Special](#olspecial) and [OL:AlsoSpecial](#olalsospecial)). Datasource-information from each overlay will be added to its referencing basemap-layer. This results in a template with two layers: <br/>
- `SingleTag`: one datasource ("openstreet") included from the [OL:Plain](#olplain)-overlay. <br/>
- `MultiTag`: two datasources ("scary") and ("danger"), included from the [OL:Special](#olspecial) and [OL:AlsoSpecial](#olalsospecial)-overlays. <br/> <br/>
This enables the two datasources included in `MultiTag` to be handled as one (dynamic) layer. Hiding `MultiTag`-layer will hide data from both "scary"- and "danger"-mapsignatures.



## Complex example
It is also possible to combine the different templatefilters. Say you have a basemap-template like this:

```xml
<compositemaptemplate id="xyz" name="BM:Complex" version="0.0.127">
  <layer type="MapLayer" name="Peaceful">
    <opacity>1</opacity>
    <brightness>0</brightness>
    <gamma>1</gamma>
    <contrast>0</contrast>
    <grayscale>false</grayscale>
    <minscalevisible>1</minscalevisible>
    <maxscalevisible>4M</maxscalevisible>
    <datasource>
      <mapsignature>bliss</mapsignature>
      <maptype>VectorMap</maptype>
      <showlabels>false</showlabels>
      <usecache>true</usecache>
    </datasource>
    <visible>true</visible>
  </layer>

  <templatefilter refs="wms"/>
  
  <layer type="MapLayer" name="MultiTag">
    <opacity>1</opacity>
    <brightness>0</brightness>
    <gamma>1</gamma>
    <contrast>0</contrast>
    <grayscale>false</grayscale>
    <minscalevisible>244</minscalevisible>
    <maxscalevisible>32000000</maxscalevisible>
    <datasource>
      <templatefilter refs="specialground"/>
    </datasource>
    <visible>true</visible>
    <property key="labels:fetchdata" value="true" />
  </layer>
</compositemaptemplate>
```

When the template is resolved - this will appear as a template with 3 maplayers: `PlainOpenstreet`, `MultiTag` and `Peaceful`, but including 4 different datasources (plainopenstreet, scary, danger and bliss).

## IMapLayerViewModel

```csharp
        /// <summary>
        /// Get all layernames and corresponding layerids in active template.
        /// The layerid returned for dynamic layers is a group id.
        /// Use that id as input to GetReferencedMapLayers to fetch referenced overlays.
        /// </summary>
        /// <returns>Returns a dictionary with layernames and corresponding layerids (guid-string)</returns>
        Dictionary<string, string> GetMapLayers();
```

```csharp
        /// <summary>
        /// Get all referenced overlays for a dynamic templatelayer.
        /// </summary>
        /// <param name="id">Layer id</param>
        /// <returns>Returns a list of all referenced overlays</returns>
        IEnumerable<IRasterLayerData> GetReferencedMapLayers(string id);
```
