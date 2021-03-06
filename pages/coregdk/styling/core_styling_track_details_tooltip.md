---
title: Track tooltip styling
keywords: tracks, tooltips, styling
tags: [track]
sidebar: core_styling_track_sidebar
permalink: core_styling_track_details_tooltip.html
summary: Track tooltip styling details. 
---

The tooltip category contains styling used for a tooltip that can be displayed when the mouse pointer is over a track.

```xml
<stylecategory name="Tooltip"/>
  <style>  
      Style items
  </style>
</stylecategory>
```

The root level value items of the style controls the tooltip's appearance:

```xml
<valueitem name="ShowDelay" value="500"/>
<valueitem name="HideDelay" value="5000"/>
<valueitem name="TopBackgroundColor" value="255,255,255,255"/>
<valueitem name="BottomBackgroundColor" value="228,228,240,255"/>
<valueitem name="OutlineColor" value="0,0,0"/>
<valueitem name="OutlineThickness" value="1"/>
<valueitem name="FontColor" value="0,0,0"/>
<valueitem name="FontFamilyName" value="Segoe UI"/>
<valueitem name="FontSize" value="10"/>  
```

 | Name                      | Description                                         | 
 | ----                      | -----------                                         | 
 | **ShowDelay**             | Delay in milliseconds before the tooltip is shown.  | 
 | **HideDelay**             | Delay in milliseconds before the tooltip is hidden. | 
 | **TopBackgroundColor**    | The top color of the background gradient color.     | 
 | **BottomBackgroundColor** | The bottom color of the background gradient color.  | 
 | **OutlineColor**          | The outline color.                                  | 
 | **OutlineThickness**      | Line thickness of the outline.                      | 
 | **FontColor**             | Font color.                                         | 
 | **FontFamilyName**        | Font name.                                          | 
 | **FontSize**              | Font size.                                          | 

The tooltip can have multiple lines of text where each line is defined by a composite item which starts with the name **TooltipLine**: 

```xml
<compositeitem name="TooltipLine1">
  <valueitem name="Label" value="Name:"/>
  <valueitem name="Fields" value="Name"/>
  <valueitem name="FieldSeparator" value="-"/>
</compositeitem>
```

 | Name               | Description                                                                                        | 
 | ----               | -----------                                                                                        | 
 | **Label**          | An optional label to show for the tooltip line.                                                    | 
 | **Fields**         | A comma separated list of track fields to show as the tooltip line.                                | 
 | **FieldSeparator** | Character used to separate several fields when multiple fields are combined into one tooltip line. | 

