---
title: Styling default values for drawobjects
keywords: drawobjects, defaultvalues, styling
tags: [drawobject]
sidebar: core_styling_sidebar
permalink: core_styling_drawobject_defaultvalues.html
summary: Set default values for draw object fields based on styles with conditions to target objects with special characteristics.
---

At creation time, draw object field values, existing or non-existing, can be set by using the same styling mechanism as for controlling their appearance. This means that default values can be set based on styles with conditions, hence targeting draw objects with special characteristics.

This allows both fields defined by the Maria GDK, such as LineWidth, and custom fields defined by the user to be set using the same style. 

A complete style set that targets the draw object fields defined by the Maria GDK can be found here: [DrawObjectDefaultValueStyle](core_styling_drawobject_defaultvalues_xml.html) 

## Style property map

The style for setting field values contains a set of composite items with value items, where the value items contain the actual field values to set. We also need a mechanism for finding which draw object field the value in a value item refers to. This is where the style property map comes in, which maps a value item to an actual draw object field.

The style property map is defiend by the section **stylepropertymap** in the default value style set. This style property map contains a **mapitem** for each draw object field, where the *name* attribute refers to the xml path to the value item and the *target* attribute refers to the draw object field.

```xml
<stylepropertymap>
  <mapitem name="Line.Width" target="LineWidth"/>
</stylepropertymap>

<style>
  <compositeitem name="Line">
    <valueitem name="Width" value="2"/>
  </compositeitem>
</style>
```

In the example above the style contains a composite item *Line* with the value item *Width* which gives the path *Line.Width*. The draw object field that controls the line width is *LineWidth*. Hence we need a style property map item with the name *Line.Width* and target *LineWidth*:

```xml
<mapitem name="Line.Width" target="LineWidth"/>
```

## Styling field values defined by Maria GDK

The complete style set [DrawObjectDefaultValueStyle](./stylingdefaultvalues/DrawObjectDefaultValueStyle.xml) contains all the Maria GDK fields which are defined in the visual appearance style. The structure of the composite items and value items in the default values style is identical to that of the visual appearance style. 

Some of the draw object field values for MIL-STD-2525C draw objects' visual appearance, such as colors and line style, are defined by a standard and cannot be changed.

To use this style for setting default values set the value of the value item to the desired value the field should have at creation time.

The following example will give all draw objects a default 12 points bold Arial font:

```xml
<compositeitem name="DefaultFont">
  <valueitem name="Bold" value="true"/>
  <valueitem name="Font" value="Arial"/>
  <valueitem name="Size" value="12"/>
</compositeitem>
```

## Styling custom field values

Custom field values can be set for all draw objects. To add default custom field values:


*  Add a custom composite item with a value item for each custom field.

*  Add a map item for each custom field to the style property map.

The following example adds two custom fields *CustomField1* and *CustomField2* to all draw objects at creation time:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<styleset name="Default">  <stylepropertymap>
  <mapitem name="Custom.CustomField1" target="CustomField1"/>
  <mapitem name="Custom.CustomField2" target="CustomField2"/>
  </stylepropertymap>
  <stylecategory name="DrawObjects">
    <style>
      <compositeitem name="Custom">
        <valueitem name="CustomField1" value="custom value 1"/>
        <valueitem name="CustomField2" value="custom value 2"/>
      </compositeitem>
    </style>
  </stylecategory>
</styleset>
```

## Conditional field value styling

The styles for setting default field values can also have conditions which targets only the draw objects that satisfy the conditions.

The following example will give all generic rectangle draw objects a default line width of 10 and a default color of green:

```xml
<styleset name="Default">
  <stylepropertymap>
    <mapitem name="Line.Width" target="LineWidth"/>
    <mapitem name="Line.Color" target="LineColor"/>
  </stylepropertymap>
  <stylecategory name="DrawObjects">
    <style>
      <compositecondition>
        <fieldcondition field="Generic" value="true"/>
        <fieldcondition field="GenericType" value="Rectangle"/>
      </compositecondition>
      <compositeitem name="Line">
        <valueitem name="Width" value="10"/>
        <valueitem name="Color" value="0,255,0,255"/>
      </compositeitem>
    </style>
  </stylecategory>
</styleset>
```

Multiple conditional styles can be added in addition to the main value style, or instead of the main value style. This means that it is possible to have a main style that targets all draw objects and have one or more conditional styles that targets specific draw objects in the same style set.
