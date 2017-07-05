---
title: Vector map featuregroups
keywords: vector, setup, featuregroups
tags: [map, vector]
sidebar: core_sidebar
permalink: core_vector_featuregroups.html
summary: Using featuregroups in vector maps. 
---

Older Versions: [1.4](./core_vector_featuregroups_v1.4.html)

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

 | Attribute                    | Description                                                                                                                                                                                         | Properties | 
 | ---------                    | -----------                                                                                                                                                                                         | ---------- | 
 | name                         | Name of feature group.                                                                                                                                                                              |            | 
 | sourcelayer                  | Name of sourcelayer in dataset.                                                                                                                                                                     |            | 
 | geotype                      | Geotype of featuregroup. Valid values are “area”, “point”, “line”.                                                                                                                      | O          | 
 | [compound](#compound-groups) | All matching subgroups will be used when symbolizing a compound featuregroup. Only used for point features. Valid values are "true" and "false".                                                    | O          | 
 | base                         | Indicates which subgroup is the base symbol of a compound featuregroup. Valid values are "true" and "false".                                                                                        | O          | 
 | top                          | Indicates that the symbolization of this subgroup should be placed above the compound base symbol. The offset should be dynamic, based on the base symbol size. Valid values are "true" and "false" | O          | 
 | [orient](#orientation)       | Name of attribute containing orientation of feature. In degrees.                                                                                                                                    | O          | 

#### Condition

See paragraph on [conditions](./core_vector_symbolization.html#condition).

#### Compound groups

If you need to visualize complex point symbols without wanting to predefine all combinations, you can use compound featuregroups for a more dynamic symbolization configuration. This enables you to f.x. define a configuration for a buoy-feature with six symbols, where each symbol varies based on the features attributes.

When a featuregroup is given the attribute <... compound="true">, all subgroups will be matched against a features attributes and combined to draw the final point symbolization.
Use *base* to indicate which subgroup should be used as the "base" symbol. If more than one symbol is marked as base, behaviour is undefined.
Use *top* to hint that a subgroups symbolization should be drawn above the base symbol. The offset is dynamic, based on the height of the base symbol. 

Example xml:

```xml
  <group name="buoy" compound="true">
    <condition oper="equal" field="F_CODE" value="BC020" />
    <group name="nav system type">
      <group name="radar station">
        <condition oper="equal" field="NST" value="45" />
      </group>
      <group name="non-pillar; radio navigation system">
        <condition oper="and">
          <condition oper="in" field="NST" value="1,4,5,17,41,51,53" />
          <condition oper="noteq" field="SSC" value="10" />
        </condition>
      </group>                                                
    </group>
    <group name="buoy type category" base="true">
      <group name="articulated light (base)">
        <condition oper="equal" field="BTC" value="35" />
      </group>
      <group name="default; type unknown">
        <condition oper="and">
          <condition oper="equal" field="BTC" value="0" />
          <condition oper="in" field="SSC" value="0,85,999" />
        </condition>
      </group>
    </group>
    <group name="topmark" top="true">
      <group name="one cone up">
        <condition oper="in" field="TMC" value="16,21" />
      </group>
      <group name="one cone down">
        <condition oper="equal" field="TMC" value="22" />
      </group>
    </group>
  </group>
```

#### Orientation

If a point symbol should be rotated according to a feature attribute value, you can specify which attribute using <group orient="[attribute]">. Orientation is given in degrees and will apply to all point symbols resolved by the featuregroup.

Example:

```xml
  <group name="ROUTEP" geotype="point" sourcelayer="routep">
    <group name="route (maritime) - direction unknown">
      <condition oper="equal" field="DOF" value="0" />
    </group>
    <group name="route (maritime) - direction known" orient="DOF">
      <condition oper="noteq" field="DOF" value="0" />
    </group>
  </group>
```

