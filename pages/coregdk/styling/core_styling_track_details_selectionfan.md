---
title: Track selection fan styling
keywords: styling
tags: [track]
sidebar: core_styling_track_sidebar
permalink: core_styling_track_details_selectionfan.html
summary: Track selection fan styling details. 
---

# Selection fan

The selection fan category contains styling used for a selection fan that can be displayed when multiple tracks can be selected at the same point by clicking on the map.

```xml
<stylecategory name="SelectionFan"/>
  <style>  
      Style items
  </style>
</stylecategory>
```

The value items at the root level of the style control the appearance of the actual fan control used to display the tracks:

```xml
<valueitem name="InnerFanBackgroundColor" value="255,255,255,0"/>
<valueitem name="OuterFanBackgroundColor" value="255,255,255,200"/>
<valueitem name="FanBackgroundColor" value="255,255,255,0"/>
<valueitem name="EmptySymbolColor" value="200,200,200,255"/>
<valueitem name="OuterFanOutlineColor" value="0,0,0,100"/>
<valueitem name="OuterOutlineThickness" value="1"/>
<valueitem name="MaxCount" value="10"/>  
<valueitem name="OutlineColor" value="0,0,0,100"/>
<valueitem name="OutlineThickness" value="2"/>
```

 | Name                        | Description                                                                                                                                                                          | 
 | ----                        | -----------                                                                                                                                                                          | 
 | **InnerFanBackgroundColor** | The inner color of the background gradient color.                                                                                                                                    | 
 | **OuterFanBackgroundColor** | The outer color of the background gradient color.                                                                                                                                    | 
 | **FanBackgroundColor**      | The center color of the selection fan.                                                                                                                                               | 
 | **EmptySymbolColor**        | Color used for a track if no preview for it exists.                                                                                                                                  | 
 | **OuterFanOutlineColor**    | The outer outline color.                                                                                                                                                             | 
 | **OuterOutlineThickness**   | Line thickness of outer outline.                                                                                                                                                     | 
 | **OutlineColor**            | The inner outline color.                                                                                                                                                             | 
 | **OutlineThickness**        | Line thickness of inner outline.                                                                                                                                                     | 
 | **MaxCount**                | Maximum number of tracks that the fan can show. If more than maximum number of tracks can be selected, the fan will not be shown. This is to prevent the fan from being overcrowded. | 

The composite item **Selected** controls the appearance of the selection rectangle for selected tracks in the fan:

```xml
<compositeitem name="Selected">    
  <valueitem name="Color" value="255,255,0,255"/>
  <valueitem name="Thickness" value="4.0"/>
</compositeitem>  
```

 | Name          | Description                       | 
 | ----          | -----------                       | 
 | **Color**     | Color of selection rectangle.     | 
 | **Thickness** | Thickness of selection rectangle. | 

The composite item **Label** controls the appearance of the label that can be shown for the tracks in the fan:

```xml
<compositeitem name="Label">    
  <valueitem name="FontSize" value="10"/>
  <valueitem name="FontName" value="Verdana"/>
  <valueitem name="Bold" value="false"/>
  <valueitem name="Italic" value="false"/>
  <valueitem name="Color" value="50,50,50,155"/>
  <valueitem name="Background" value="255,255,255,127"/>        
  <valueitem name="Show" value="false"/>
  <valueitem name="Fields" value="Name"/>
  <valueitem name="FieldSeparator" value="-"/>
</compositeitem>  
```

 | Name          | Description                       | 
 | ----          | -----------                       | 
| **FontSize** | Font size. |
| **FontName** | Font name. |
| **Bold** | Sets bold label font. Values are true or false. |
| **Italic** | Sets italic label font. Values are true or false. |
| **Color** | Font color. |
| **Background** | Color used as a background for the text to increase readability. |
| **Show** | Sets the label visibility. Values are true or false. |
| **Fields** | A comma separated list of track fields to show as the label. |
| **FieldSeparator** | Character used to separate several fields when multiple fields are combined into one label. |
