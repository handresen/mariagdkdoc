---
title: Explaining the map template xml
keywords: templates, xml, dynamic, templatefilter, overlay
tags: [map, template]
sidebar: core_sidebar
permalink: core_maps_templates_xml.html
summary: Using map templates to create multilayered maps for Maria GDK. 
---

Map template example:

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

## Compositemaptemplate

`<compositemaptemplate>` is the root node of the template-xml.

Compositetemplates consist of one or multiple map layer-elements that together describe how to assemble a map from one or multiple datasources (services).

 | Attribute | Description | Properties | 
 | --------- | ----------- | ---------- | 
 | name      |             |            | 
 | id        |             |            | 

 | "Compositemaptemplate" child elements | Description                                                                              | Properties | 
 | ------------------------------------- | -----------                                                                              | ---------- | 
 | description                           | Text describing template.                                                                | O          | 
 | layer                                 | Map layer block. Collection of data parameters needed to build a map layer in Maria GDK. | R C A      | 
 | bookmark                              | Add bookmarks that are part of the template. Can be zero or more instances.              |            | 
 | templatefilter                        | (Maria GDK 3.1+) Used for building dynamic templates                                     | O A        | 

### Bookmark

`<bookmark>` is the root node.

 | Attribute    | Description                                                                                                       | 
 | ---------    | -----------                                                                                                       | 
 | name         | A descriptive name of the bookmark                                                                                | 
 | lat          | The latitude of the bookmark.                                                                                     | 
 | lon          | The longitude of the bookmark.                                                                                    | 
 | scale        | The scale of the bookmark.                                                                                        | 
 | mapsignature | The mapsignature this bookmark is valid for.                                                                      | 
 | default      | true/false. Indicates that the bookmark will be used as the default position and scale when opening the template. | 


 | Element     | Description                    | 
 | -------     | -----------                    | 
 | description | A description of the bookmark. | 

Example:

```xml
<bookmark lat="60" lon="10" scale="1000000" mapsignature="bluemarble2012" name="Østlandet">
  <description>Posisjon over østlandet.</description>
</bookmark> 
```

### Layer

 | Attribute    | Description                                                                                                          | Properties | 
 | ---------    | -----------                                                                                                          | ---------- | 
 | mapsignature | Unique key that identifies a map package or m6m multidataset.                                                        |          | 
 | type         | Layer type (MapLayer, ModLayer, Placeholder, PropagationLayer, WmsLayer). See below for valid layer type values. | O          | 
 | category     | Layer category. Only valid when type attribute is “ModLayer”. See Table 2: layer category attribute values.  | O          | 


Valid layer type attribute values:

 | Value            | Description                                                                                                          | 
 | -----            | -----------                                                                                                          | 
 | MapLayer         | Default value. Layer contains vector or raster data.                                                                 | 
 | ModLayer         | Modification layer. Used for f.ex. elevation effects. Indicates that this layer will modify data from another layer. | 
 | Placeholder      | Placeholder layer. Used to signal where data from other sources should be inserted in the template.                  | 
 | PropagationLayer | Special layer used to display propagation data.                                                                      | 
 | WmsLayer         | Special layer used to display WMS data.                                                                              | 
 | ElevationLayer   | Elevation data layer                                                                                                 | 

Valid layer category attribute values:

 | Value       | Description                | 
 | -----       | -----------                | 
 | ElevEffects | Elevation shading effects. | 

 | "Layer" child elements | Description                                                      | Properties | 
 | ---------------------- | -----------                                                      | ---------- | 
 | Opacity                | Starting opacity of the map template layer                       |            | 
 | Brightness             | Starting brightness of the map template layer                    |            | 
 | Gamma                  | Starting gamma of the map template layer                         |            | 
 | Contrast               | Starting contrast of the map template layer                      |            | 
 | Grayscale              | Indicates if this map layer should be displayed in gray scale.   |            | 
 | Description            | Text describing layer.                                           | O          | 
 | Minscalevisible        | Template layer minimum scale value                               |            | 
 | Maxscalevisible        | Template layer maximum scale value                               |            | 
 | Datasource             | Settings related to the source of map data for the current layer |            | 
 | Visible                | Indicates if the layer should be visible or not                  |            | 
 | Property               | Indicates if coverage rectangles should be visible or not        |            | 

#### Opacity

 | Valid values | Description                                          | 
 | ------------ | -----------                                          | 
 | 0.0 - 1.0    | Sets the starting opacity of this map template layer | 

#### Brightness

 | Valid values | Description                                             | 
 | ------------ | -----------                                             | 
 | -1.0 - 1.0   | Sets the starting brightness of this map template layer | 

#### Gamma

 | Valid values | Description                                                                                        | 
 | ------------ | -----------                                                                                        | 
 | 0.5 - 2.2    | Sets the starting gamma of this map template layer. Value should preferably be between 0.5 and 2.2 | 

#### Contrast

 | Valid values | Description                                           | 
 | ------------ | -----------                                           | 
 | -1.0 - 1.0   | Sets the starting contrast of this map template layer | 

#### Grayscale

 | Valid values | Description                                                   | 
 | ------------ | -----------                                                   | 
 | true/false   | Indicates if this map layer should be displayed in gray scale | 

#### Description

 | Valid values | Description               | 
 | ------------ | -----------               | 
 | textstring   | Text describing the layer | 

#### Minscalevisible

 | Valid values | Description                                                                                                                               | 
 | ------------ | -----------                                                                                                                               | 
 | 0 - ∞      | Template layer minimum scale value. The layer will not be visible at scales lower than this.  Use of scale factors (k/K/m/M) are allowed. | 

#### Maxscalevisible

 | Valid values | Description                                                                                                                                | 
 | ------------ | -----------                                                                                                                                | 
 | 0 - ∞      | Template layer maximum scale value. The layer will not be visible at scales higher than this.  Use of scale factors (k/K/m/M) are allowed. | 

#### Visible

 | Valid values | Description                                     | 
 | ------------ | -----------                                     | 
 | true/false   | Indicates if the layer should be visible or not | 

#### Property

 | Value | Description                                                                                 | 
 | ----- | -----------                                                                                 | 
 | key   | "coverage:show" Indicates if rectangles showing the coverage of the map data is to be drawn | 
 | value | true/false                                                                                  | 

Example:

```xml
<property key="coverage:show" value="false" />
```


#### Datasource

 | "Datasource" child elements | Description                                                   | Properties | 
 | --------------------------- | -----------                                                   | ---------- | 
 | mapsignature                | Unique key that identifies a map package or m6m multidataset. | O A        | 
 | maptype                     | Map content type.                                             | O A        | 
 | version                     | Version specification                                         | O A        | 
 | hostcategory                | Map service host category.                                    | O A        | 
 | showlabels                  | Indicates if labels should be drawn                           |            | 
 | layergroupfilter            | Group filter. Used to exclude or include parts of layerdata.  | O C        | 
 | usecache                    | Decides if the system should prefer the cache.                | O C        | 
 | templatefilter              | (Maria GDK 3.1+) Used for building dynamic templates          | O A        | 

##### Mapsignature

 | Valid values | Description                                                                                                                                                                                               | 
 | ------------ | -----------                                                                                                                                                                                               | 
 | textstring   | For vector maps, this is the filename of the map package (excluding ".m6mmappackage.xml"). For raster maps, the signature is an attribute of the <mappackage> element in the .rastermappackages.xml file. | 

##### Maptype

 | Valid values     | Note                                                                                                                  | 
 | ------------     | ----                                                                                                                  | 
 | Undefined        | This is the default maptype                                                                                           | 
 | VectorMap        |                                                                                                                       | 
 | RasterMap        |                                                                                                                       | 
 | CompositeMap     |                                                                                                                       | 
 | Propagation      |                                                                                                                       | 
 | Elevation        |                                                                                                                       | 
 | ElevationNormals | Generate normal maps from an elevation data set. If you have pregenerated normal maps, use regular RasterMap instead. | 
 | Bathymetry       |                                                                                                                       | 
 | WmsMap           |                                                                                                                       | 


##### Version

Version specification is used to specify specific versions or ranges of versions for map sources (map packages).

 | Attribute   | Valid values                                  | Comment                                                                                                                         | 
 | ---------   | ------------                                  | -------                                                                                                                         | 
 | min         | Three part numeric, semantic version, "1.0.3" | Lowest version accepted                                                                                                         | 
 | max         | Three part numeric, semantic version, "1.0.7" | Highest version accepted                                                                                                        | 
 | releasetype | Release, Draft, AnyReleaseState (default)     | If set to "Release" or "Draft", only versions of that type will be accepted                                                     | 
 | versiontype | AnyVersion (default), Exact, Newest           | "AnyVersion" matches any in range. Exact only matches "min", "Newest" matches only newest version if mixed versions are present | 



##### Showlabels

 | Valid values | Description                          | 
 | ------------ | -----------                          | 
 | true/false   | Indicates if labels should be drawn. | 

##### Usecache

 | Valid values | Description                                 | 
 | ------------ | -----------                                 | 
 | true/false   | Indicates if cached map data should be used | 

##### Layergroupfilter

Use group filters to control which vector map themes should be displayed in map layer. “-“ in front of feature group name excludes group from map. Subgroups must include the parent group(s), separated with backslash "\"

Example (show only motorways and railways):

```xml
  <layergroupfilter>Roads\Motorways</layergroupfilter>
  <layergroupfilter>Railways</layergroupfilter>
```

### Templatefilter (only Maria GDK 3.1+)

`<templatefilter>` is the root node. 

 | Attribute | Description                                                                                        | 
 | --------- | -----------                                                                                        | 
 | refs      | List of overlay-tags to include in template. Only used in regular templates (not overlays)         | 
 | tags      | List over tags valid for this overlay. Only used in overlays.                                      | 
 | pri       | Priority number. Lower number equals higher priority. Default value is 100. Only used in overlays. | 

template.xml:

```xml
<compositemaptemplate name="BasemapXYZ" id="5497D2A9-25C6-429B-AA9A-0ADAB98721C0">
  <templatefilter refs="groundraster, norwayoverlay, seaoverlay"/>
</compositemaptemplate>
```

or:

```xml
<compositemaptemplate name="TestRef" id="5497D2A9-25C6-429B-AA9A-0ADAB98721C0">
  <layer type="MapLayer" name="Groundrasters"> 
    <.../>
    <datasource>
      <templatefilter refs="groundraster"/>
    </datasource>
  </layer>
</compositemaptemplate>
```


overlay.xml:

```xml
<compositemaptemplate name="test" id="0CB9BB2B-3C4E-4922-889B-7442E683C2AB">
  <templatefilter tags="groundraster" pri="500"/>
  <layer type="OverlayLayer" name="OpenstreetMap">
    <.../>
  </layer>
</compositemaptemplate>
```
