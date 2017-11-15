---
title: Lookuptables for feature info
keywords: lookuptables, featurequeries
tags: [map]
sidebar: core_sidebar
permalink: core_maps_lookuptables.html
summary: Use lookup tables to map feature attribute codes to readable text when performing feature queries. 
---

{% include callout.html content="Available since Maria GDK 3.1." type="info" %}

Lookup tables are used for mapping feature attribute codes to readable text when performing feature queries on vector maps. These lookup tables are defined in xml-files and should be located together with the map data they relate to (multidatasets). The files use the extension .lookup.xml (f.ex. N500.m6mmultidataset.lookup.xml).

Example:

```xml
<lookup>
  <attribute code="FFP" text="Farming Pattern">
    <value text="Linear" code="1"/>
    <value text="Regular" code="2"/>
    <value text="Terraced" code="3"/>
    <value text="Intermingled Woods" code="4"/>
    <value text="Intermingled Trees" code="5"/>
    <value text="Treeless" code="6"/>
    <value text="Trellised" code="7"/>
    <value text="Irregular" code="8"/>
  </attribute>
  <attribute code="FUN" text="Condition of Facility">
    <value text="Under Construction" code="1"/>
    <value text="Abandoned" code="2"/>
    <value text="Destroyed" code="3"/>
    <value text="Dismantled" code="4"/>
    <value text="Proposed" code="5"/>
    <value text="Fully Functional" code="6"/>
    <value text="Ruined" code="7"/>
    <value text="Partially Functional" code="9"/>
    <value text="Partially Destroyed" code="11"/>
    <value text="Damaged" code="13"/>
    <value text="Disused" code="16"/>
    <value text="Planned" code="17"/>
    <value text="Salvaged" code="18"/>
  </attribute>
  <attribute code="DMT" text="Canopy Cover (percentage)"/>
</lookup>
```

Querying a feature with attribute "FFP, 3" while using this example lookup-table will return the result "Farming Pattern, Terraced". 

### Lookup table xml

`<lookup>` is the root of the lookup-xml.

 | Child element | Description       | Properties | 
 | ------------- | -----------       | ---------- | 
 | attribute     | Feature attribute | O R C A    | 

#### Attribute

 | Child element | Description     | Properties | 
 | ------------- | -----------     | ---------- | 
 | value         | Attribute value | R O A      | 


 | Attribute | Description                           | Properties | 
 | --------- | -----------                           | ---------- | 
 | code      | Attribute code                        |            | 
 | text      | Textual translation of attribute code |            | 

#### Value

 | Attribute | Description                            | Properties | 
 | --------- | -----------                            | ---------- | 
 | code      | Attribute value                        |            | 
 | text      | Textual translation of attribute value |            | 

{% include callout.html content="Note that the original feature attribute values are available clientside." type="info" %}

