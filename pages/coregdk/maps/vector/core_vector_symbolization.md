---
title: Vector map feature symbolization.
keywords: vector, setup
tags: [map, vector]
sidebar: core_sidebar
permalink: core_vector_symbolization.html
summary: Setting up feature symbolization for vector maps in Maria GDK 
---

# Feature symbolization

Symbols.xml is divided in seven parts contained inside the root element `<symbolizations>`.

Overall structure of the symbols.xml file:

```xml
<symbolization>
  <config\>
  <constants\>
  <colors\>
  <points\>
  <lines\>
  <areas\>
  <labels\>
</symbolization>
```

## Symbolization

`<symbolization>` is the root node of the symbols.xml.

 | Child element | Description                                              | Properties | 
 | ------------- | -----------                                              | ---------- | 
 | config        | Configuration settings used when building the map style. | O C        | 
 | constants     | Constant definitions.                                    | O C A      | 
 | colors        | Color definitions.                                       | O C A      | 
 | points        | Point symbol definitions.                                | O C        | 
 | lines         | Line symbol definitions.                                 | O C        | 
 | areas         | Area symbol definitions.                                 | O C        | 
 | labels        | Font characteristics and label rule definitions.         | O C        | 

###  Config

 | Child element  | Description                                              | Properties | 
 | -------------  | -----------                                              | ---------- | 
 | yaxisdirection | Defines the direction of the yaxis for symbols/patterns. | O A        | 

#### Yaxisdirection

 | Attribute | Description                                                                                                                                            | Properties | 
 | --------- | -----------                                                                                                                                            | ---------- | 
 | value     | “Up” (origo in bottom left corner, y axis pointing up) or “Down” (origon in top left corner, y axis pointing down). Default value is “up”. |          | 

###  Constants

 | Child element | Description                              | Properties | 
 | ------------- | -----------                              | ---------- | 
 | constant      | Constant value needed for symbolization. | O R A      | 

#### Constant

 | Attribute | Description                          | Properties | 
 | --------- | -----------                          | ---------- | 
 | name      | String that identifies the constant. |          | 
 | value     | The value of the constant.           |          | 

Example:

```xml
<constants>
  <constant name="contour_interval" value="20"/>
</constants>
```

### Colors

The `<colors>`node contain the color table information. This color table is referenced from the point, line, area and label sections of the symbol.xml file.

 | Child element | Description             | Properties | 
 | ------------- | -----------             | ---------- | 
 | color         | Color value definition. | O R A      | 

#### Color

 | Attribute | Description                                                         | Properties | 
 | --------- | -----------                                                         | ---------- | 
 | Name      | String that identifies the color.                                   |          | 
 | Rgba      | value defining the red, green, blue and alpha values for the color. |          | 

Example:

```xml
<colors>
  <color name="black" rgba="0,0,0,255"/>
  <color name="black12" rgba="224,224,224,255"/>
  <color name="cyan" rgba="0,159,218,255"/>
  <color name="cyan4" rgba="242,252,252,255"/>
  <color name="white" rgba="255,255,255,255"/>
</colors>
```

### Points

[Details Point Symbolization](./core_vector_symbolization_points.html)

###  Lines

[Details Line Symbolization](./core_vector_symbolization_lines.html)

### Areas

[Details Area Symbolization](./core_vector_symbolization_areas.html)

### Labels

[Details Label Symbolization](./core_vector_symbolization_labels.html)


