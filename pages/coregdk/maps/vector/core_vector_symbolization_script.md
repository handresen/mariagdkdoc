---
title: Vector map setup in Maria GDK
keywords: vector, setup
tags: [map, vector]
sidebar: core_sidebar
permalink: core_vector_symbolization_script.html
summary: Setting up vector maps in Maria GDK 
---

# Symbolization scripting

The ScriptCollection.xml specifies how to build map styles for the feature groups in the corresponding featuregrouping.xml based on the resources available in the symbols.xml. The xml is structured as a collection of multiple style sets describing how to visualize a map feature.

Example:

```xml
<set desc="Extraction Mine" geotype="area">
  <style name="Abandoned/not peatery" 
         group="Extraction Mine\Abandoned/not peatery">
    <areanamed name="AR_Black_Mine_Abandoned"/>
    <labelrule name="EXTRACTION_MINE"/>
  </style>
  <style name="Peatery" 
         group="Extraction Mine\Peatery">
    <areanamed name="AR_Black_Mine_PeatCuttings"/>
    <labelrule name="R000003"/>
  </style>
  <style name="Fully functional, or unknown/not peatery" 
         group="Extraction Mine\Fully functional, or unknown/not peatery">
    <areanamed name="AR_Black_Mine"/>
    <labelrule name="EXTRACTION_MINE"/>
  </style>
</set>
```

## Collection

`<collection>` is the root node of the script-xml.

 | Child element | Description                                           | Properties | 
 | ------------- | -----------                                           | ---------- | 
 | set           | Style set describing ways to visualize a map feature. | O R C A    | 

 | Attribute | Description             | Properties | 
 | --------- | -----------             | ---------- | 
 | name      | Name of the collection. |          | 

### Set

 | Child element | Description                                                     | Properties | 
 | ------------- | -----------                                                     | ---------- | 
 | style         | Style block. Describes how to visualize a specific map feature. | O R C A    | 

 | Attribute | Description                                                    | Properties | 
 | --------- | -----------                                                    | ---------- | 
 | desc      | Style set descriptive name.                                    |          | 
 | geotype   | Geotype. Valid values are “area”, “point”, “line”. | O          | 

#### Style

 | Child element | Description                                 | Properties | 
 | ------------- | -----------                                 | ---------- | 
 | areanamed     | Description of how to style a area feature. | O R A      | 
 | pennamed      | Description of how to style a line feature. | O R A      | 
 | labelrule     | Name of applicable labelrule.               | O A        | 

 | Attribute   | Description                                                                                                                                                                                                                                                                                                                                                                | Properties | 
 | ---------   | -----------                                                                                                                                                                                                                                                                                                                                                                | ---------- | 
 | name        | Name of style description block.                                                                                                                                                                                                                                                                                                                                           |          | 
 | visible     | True/false. If false, suppress feature from map.                                                                                                                                                                                                                                                                                                                           | O          | 
 | group       | Name of featuregroup. To style a sub featuregroup, concat groupnames with “\”.                                                                                                                                                                                                                                                                                         |          | 
 | compositing | "copy" or "blend" (default). "blend" will blend features in the group with underlying features from the same layer and other layers, causing the underlying  layers to still be displayed if current layer is transparent. "copy" will overwrite underlying layers in the same dataset, background layers will however still be displayed if current layer is transparent. | O          | 

##### Areanamed

 | Attribute | Description                                                                                               | Properties | 
 | --------- | -----------                                                                                               | ---------- | 
 | name      | Name of area style description. Should point to area style definition found in corresponding symbols.xml. |          | 

##### Pennamed

 | Attribute | Description                                                                                                                     | Properties | 
 | --------- | -----------                                                                                                                     | ---------- | 
 | name      | Name of line style OR point style description. Should point to line/symbol style definition found in corresponding symbols.xml. |          | 

Example:

```xml
  <set desc="Navn" geotype="point">
    <style name="Stedsnavn" group="Navn\Stedsnavn">
A     <pennamed name="Black_0.5mmSquareFilled"/>
      <labelrule name="Stedsnavn" placements="TL" />
    </style>
  </set>
  <set desc="Hoyde" geotype="line">
    <style name="Hoydekurve" group="Hoyde\Hoydekurve">
B     <pennamed name="Hoydekurve" />
    </style>
  </set>
```

Where A points to a symbol definition and B points to a line definition.

#####  Labelrule

 | Attribute   | Description   | Properties | 
 | ---------   | -----------   | ---------- | 
| name | Name of labelrule. Should point to labelrule definition found in corresponding symbols.xml. |  |
| priority | Priorty. Hints of the importance of this label. Higher number equals less important. | O |
| margin | Placement margin in pixels. | O |
| closestduplicate | Distance in pixels. Label will be suppressed if duplicate labels exists inside the range specified by closestduplicate. | O |
| placementalongline | True/false. If true, label will be placed along line if possible. | O |
| backgroundtag | Name of label background style. This is a fixed set of styles, contact Maria GDK support team for a list of available background style values. | O |
| placements | Valid placements for label. Values are TL (top left), TC (top center), TR (top right), CL (center left), CC (center center), CR (center right), BL (bottom left), BC (bottom center) and BR (bottom right). If not set, all placements are valid. | O |

Example:

```xml
<labelrule name="OSM_Ref" 
           margin="0" 
           closestduplicate="90" 
           priority="10" 
           backgroundtag="majorroad"/>
```

{% include image.html file="core/vector/maria2012_vectormaps_html_1190902.png" %}