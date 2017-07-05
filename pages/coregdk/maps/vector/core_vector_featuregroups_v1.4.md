---
title: Vector map featuregroups v1.4
keywords: vector, setup, featuregroups
tags: [map, vector]
sidebar: core_sidebar
permalink: core_vector_featuregroups_v1.4.html
summary: Using featuregroups in vector maps (version 1.4). 
---

Featuregroups are used for grouping map themes into logical groups that are easy to control and style. Featuregroups is the only file that references the dataset and requires that you know what features are available.

Example:

```xml
<group name="Extraction Mine" sourcelayer="aaa010" geotype="area">
  <group name="Abandoned/not peatery">
    <condition oper="and">
      <condition field="FUN" value="2" oper="equal"/>
      <condition field="MIN" value="8" oper="noteq"/>
    </condition>
  </group>
  <group name="Peatery">
    <condition field="MIN" value="8" oper="equal"/>
  </group>
  <group name="Fully functional, or unknown/not peatery">
    <condition oper="and">
      <condition field="FUN" value="0,6" oper="in"/>
      <condition field="MIN" value="8" oper="noteq"/>
    </condition>
  </group>
</group>
```

##  Groups

`<groups>` is the root node of the featuregrouping-xml.

 | Child element | Description                                      | Properties | 
 | ------------- | -----------                                      | ---------- | 
 | group         | Describes a logical collection of data elements. | O R C A    | 

### Group

 | Child element | Description                 | Properties | 
 | ------------- | -----------                 | ---------- | 
 | condition     | Featuregroup rule condition | O R C A    | 
 | group         | Nested group element.       | O R C A    | 

 | Attribute   | Description                                                                    | Properties | 
 | ---------   | -----------                                                                    | ---------- | 
 | name        | Name of feature group.                                                         |            | 
 | sourcelayer | Name of sourcelayer in dataset.                                                |            | 
 | geotype     | Geotype of featuregroup. Valid values are “area”, “point”, “line”. | O          | 

#### Condition

See paragraph on [conditions](./core_vector_symbolization.html#Condition).

