---
title: Vector map area feature symbolization.
keywords: vector, symbolization, area
tags: [navigation]
sidebar: core_sidebar
permalink: core_vector_symbolization_areas.html
summary: Setting up area feature symbolization for vector maps in Maria GDK 
---

### Areas

The `<areas>` section of the file (located on the level below the root node) contains all area symbol definitions.

 | Child element | Description                       | Properties | 
 | ------------- | -----------                       | ---------- | 
 | config        | Configuration settings for areas. |          | 
 | area          | Area element definition.          | R C        | 

{% include image.html file="./core/vector/maria2012_vectormaps_html_m3eb83d76.png" caption="Figur 3: Example of complex area style" %}

#### Config

 | Child element | Description                                                                  | Properties | 
 | ------------- | -----------                                                                  | ---------- | 
 | defaultroot   | Default root path to location of symbol/pattern files used in symbolization. | O A        | 

##### Defaultroot

 | Attribute | Description                                                                               | Properties | 
 | --------- | -----------                                                                               | ---------- | 
 | path      | Relative path to where the pointsymbol-, linepattern- and areapattern-files, are located. |          | 

#### Area

Areas can be symbolized with outlines, fill patterns/textures and point symbols attached. Textures/brushbitmaps must be available as .png files.

Example:

```xml
<area>
  <sgname>AR_Black_Mine_Abandoned</sgname>
  <outline linename="Black_0.2mm-2mmLenDash-0.5mmGapLine"/>
  <symbol name="Black_PickAxeShovel180" placement="center"/>
</area>
```

 | Child element | Description                                                                                                                                                | Properties | 
 | ------------- | -----------                                                                                                                                                | ---------- | 
 | sgname        | The name of the symbol graphic. This name is used when referencing the point definition from the script files or from other parts of the symbols.xml file. | O          | 
 | outline       | Definition of the area outline.                                                                                                                            | O A        | 
 | brush         | Definition of the area brush.                                                                                                                              | O A        | 
 | symbol        | Defines a symbol to be drawn on the approximate top or center of an area.                                                                                  | O R A      | 
 | brushbitmap   | Defines a fill pattern bitmap for the brush.                                                                                                               | O A        | 

##### Outline

 | Attribute | Description                                     | Properties | 
 | --------- | -----------                                     | ---------- | 
 | linename  | References a line definition to use as outline. |          | 

##### Brush

 | Attribute | Description                                                                                                                    | Properties | 
 | --------- | -----------                                                                                                                    | ---------- | 
 | style     | Brush style. Valid values are [solid, hatched].                                                                                |          | 
 | hatch     | Hatch pattern. Mandatory if style is hatched. Valid values are [bdiagonal, cross, diagcross, fdiagonal, horizontal, vertical]. | O          | 
 | colorname | Brush color (defined in color table).                                                                                          | O          | 

##### Symbol

 | Attribute | Description                                                                                                                                         | Properties | 
 | --------- | -----------                                                                                                                                         | ---------- | 
 | name      | Name of the symbol graphic to be used in combination with the pen style. Should correspond to an existing sgname already defined in a `<point>` node. | O          | 
 | placement | Placement of the symbol on the line. Valid values are [top, center].                                                                                | O          | 
 | rotoffset | Rotation offset.                                                                                                                                    | O          | 
 | xoffs     | Offset in the X direction.                                                                                                                          | O          | 
 | yoffs     | Offset in the Y direction.                                                                                                                          | O          | 

##### Brushbitmap

 | Attribute | Description                                                                                                                                               | Properties | 
 | --------- | -----------                                                                                                                                               | ---------- | 
 | name      | Bitmapbrush filename (only basename, no extension). Should be located at the path pointed to by the `<defaultroot>` node in the config section of the file. |          | 

