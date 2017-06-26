---
title: Map templates MARIA GDK version 1.5
keywords: templates, old, 1.5
tags: [navigation]
sidebar: core_sidebar
permalink: core_maps_templates_v1.5.html
summary: Using map templates to create multilayered maps for Maria GDK. 
---
# Map templates

*Applies to Maria GDK 1.5 and older*

The map template xml joins together map data from different sources to create a multilayered, rich map for Maria2012. A template can contain multiple layers referencing mapsignatures from both raster and vector mapservices. Layers can also act as modification layers (for example elevation shading effects) and placeholder layers (for special data like WMS and propagation).

Map templates are provided to the Maria2012 client application via the [Maria2012 Map Catalog service](maria_gdk/programming/functionality/mapcatalog). Templates are searched for in the paths specified in the `<templatesources>`-section of the map catalog service app.config-file. 
[Links](./core_maps_links.html)-files can be used to point to yet other folders containing templates.

Example:

```xml

<compositemaptemplate description="WorldMap">
  <layer mapsignature="Composite vector">
    <maptype value="VectorMap"/>
    <servicetype value="CacheService"/>
    <description>Vector maps without transportation</description>
    <groupfilter>
      <item value="-Roads"/>
      <item value="-Railways"/>
    </groupfilter>
    <params>
      <coverage.show value="false"/>
      <labels.fetchdata value="true"/>
    </params>
  </layer>      
  <layer mapsignature="Oslo">
    <maptype value="RasterMap"/>
    <description>Blue Marble world raster and Oslo airphoto</description>
    <servicetype value="CacheService"/>
    <hostcategory value="PreferLocal"/>
    <params>
      <labels.fetchdata value="false"/>
    </params>
  </layer>
  <layer mapsignature="Composite vector">
    <maptype value="VectorMap"/>
    <servicetype value="CacheService"/>
    <description>Vector maps with transportation</description>
    <groupfilter>
      <item value="Roads"/>
      <item value="Railways"/>
    </groupfilter>
    <params>
      <coverage.show value="false"/>
      <labels.fetchdata value="true"/>
    </params>
  </layer>      
  <layer mapsignature="NormalMapElevations" 
         category="ElevEffects" 
         type="ModLayer"  
         minvisible="500" 
         maxvisible="100K"> 
    <description>Elevation normals</description>
    <servicetype value="CacheService"/> 
  </layer>
  <layer type="placeholder" category="WMSLayers"/>
  <layer type="placeholder" category="CoverageLayers"/>
</compositemaptemplate>
```

## Compositemaptemplate

`<compositemaptemplate>` is the root node of the template-xml.

Compositetemplates consist of one or multiple map layer-elements that together describe how to assemble a map from one or multiple datasorces (services).


 | Attribute   | Description               | Properties | 
 | ---------   | -----------               | ---------- | 
 | description | Text describing template. | O          | 

 | Child element | Description                                                                              | Properties | 
 | ------------- | -----------                                                                              | ---------- | 
 | layer         | Map layer block. Collection of data parameters needed to build a map layer in Maria2012. | R C A      | 

### Layer

 | Attribute    | Description                                                                                                          | Properties | 
 | ---------    | -----------                                                                                                          | ---------- | 
 | mapsignature | Unique key that identifies a map package or m6m multidataset. Key must be a valid entry in mapcollection-xml.        |          | 
 | type         | Layer type (MapLayer, ModLayer, Placeholder, PropagationLayer, WmsLayer). See below for valid layer type values. | O          | 
 | category     | Layer category. Only valid when type attribute is “ModLayer”. See Table 2: layer category attribute values.  | O          | 
 | minvisible   | Minimum visible mapscale. Use of scale factors (k/K/m/M) are allowed.                                                | O          | 
 | maxvisible   | Maximum visible mapscale. Use of scale factors (k/K/m/M) are allowed.                                                | O          | 

Valid layer type attribute values:

 | Value            | Description                                                                                                          | 
 | -----            | -----------                                                                                                          | 
 | MapLayer         | Default value. Layer contains vector or raster data.                                                                 | 
 | ModLayer         | Modification layer. Used for f.ex. elevation effects. Indicates that this layer will modify data from another layer. | 
 | Placeholder      | Placeholder layer. Used to signal where data from other sources should be inserted in the template.                  | 
 | PropagationLayer | Special layer used to display propagation data.                                                                      | 
 | WmsLayer         | Special layer used to display WMS data.                                                                              | 

Valid layer category attribute values:

 | Value       | Description                | 
 | -----       | -----------                | 
 | ElevEffects | Elevation shading effects. | 

 | Child element | Description                                                  | Properties | 
 | ------------- | -----------                                                  | ---------- | 
 | maptype       | Map content type.                                            | O A        | 
 | hostcategory  | Map service host category.                                   | O A        | 
 | servicetype   | Service type.                                                | O R A      | 
 | description   | Text describing layer.                                       | O          | 
 | groupfilter   | Group filter. Used to exclude or include parts of layerdata. | O C        | 
 | params        | Layer parameters.                                            | O C        | 

#### Maptype

Specify maptype in layer.

 | Attribute | Description                                                                                                                                                                                                                              | Properties | 
 | --------- | -----------                                                                                                                                                                                                                              | ---------- | 
 | value     | Valid values are “Undefined”, “VectorMap”, “RasterMap”, “RasterPhoto”, “CompositeMap”, “Propagation”, “Elevation”, “ElevationNormals”, “Bathymetry”, “WmsMap”. Default value is “Undefined”. |          | 

#### Hostcategory

Not implemented.

Specify if data should be retrieved from local or remote services.

 | Attribute | Description                                                                                                                         | Properties | 
 | --------- | -----------                                                                                                                         | ---------- | 
 | value     | Valid values are “DontCare”, “Local”, “Remote”, “PreferLocal”, “PreferRemote”. Default value is “DontCare”. |          | 

#### Servicetype

Specify preferred servicetypes. Multiple servicetypes can be listed, the first service that can provide the wanted data will be used.

 | Attribute | Description                                                                                                                                                                   | Properties | 
 | --------- | -----------                                                                                                                                                                   | ---------- | 
 | value     | Valid values are “Undefined”, “VectorService”, “RasterService”, “CacheService”, “WmsService”, “PropagationService”. Default value is “Undefined”. |          | 

#### Groupfilter

Use group filters to control which feature groups should be displayed in map layer. To filter subgroups, the parent group(s) must be included, separated with a backslash (see example). “-“ in front of feature group name excludes the group from the map. 

Example (show only highways and railways):

```xml
<groupfilter>
  <item value="Roads\Highways"/>
  <item value="Railways"/>
</groupfilter>
```

 | Child element | Description                                     | Properties | 
 | ------------- | -----------                                     | ---------- | 
 | item          | Feature group to include or exclude from layer. | O R A      | 

##### Item

 | Attribute | Description                                                                                                                                                                    | Properties | 
 | --------- | -----------                                                                                                                                                                    | ---------- | 
 | value     | Name of feature group to exclude or include. Feature group must be a valid entry in the file featuregrouping.xml belonging to the mapsignature specified in the current layer. | O          | 

#### Params

Example:

```xml
<params>
  <coverage.show value="false"/>
  <labels.fetchdata value="true"/>
</params>
```

 | Child element | Description                                                                                                                                                                                                                                                                                                            | Properties | 
 | ------------- | -----------                                                                                                                                                                                                                                                                                                            | ---------- | 
 | <parameter>   | Layer parameters supported by Maria2012. <br/> Supported parameters: <br/> “coverage.show” -- draw data coverage rectangles (true/false), <br/> "coverage.showlabels -- show dataset name on coverage rectangles (only valid when coverage.show is "true") <br/> “labels.fetchdata” -- fetch labeldata (true/false), | O R A      | 

 | Attribute | Description      | Properties | 
 | --------- | -----------      | ---------- | 
 | value     | Parameter value. | O          | 

