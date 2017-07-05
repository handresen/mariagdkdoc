---
title: Track symbol styling
keywords: styling
tags: [track]
sidebar: core_styling_track_sidebar
permalink: core_styling_track_details_symbol.html
summary: Track symbol styling details. 
---

## Track symbol

The track symbol category contains all styling used for standard track symbolization. Style items for specialized track display methods, such as cell display and clusters are not part of this category.

```xml
<stylecategory name="TrackSymbol"/>
  Style items
</stylecategory>
```

### Root items

#### Visible
Controls display of symbol. If set to false, symbol will not be displayed. If true, the track will displayed unless hidden by other settings, for instance the track will be invisible if "Scale" is set to 0 even if "Visible" is set to true.

```xml
<valueitem name="Visible" value="<true|false>"/>
```

#### ColorScheme

Controls color scheme for symbol sets that support this feature. The MilStd2525 symbol library used in the GDK supports color schemes.

```xml
<valueitem name="ColorScheme" value="<Light|Medium|Dark>"/>
```

| Light  | Medium | Dark |
|:---:|:---:|:---:|
| <img class="tableImage" src="images/core/tracks/trackcolorschemelight.png"/> | <img class="tableImage" src="images/core/tracks/trackcolorschememed.png"/> | <img class="tableImage" src="images/core/tracks/trackcolorschemedark.png"/> |


### CoreSymbol

Contains style items for core symbol.

```xml
<compositeitem name="CoreSymbol"/>
  Style items
</compositeitem>
```

#### GreyScale

If true, symbol is displayed in greyscale. Default is false.

```xml
<valueitem name="GreyScale" value="<true|false>"/>
```

#### Opacity

Controls opacity of core symbol. 0.0 is totally transparent (invisible), 1.0 is opaque. Default is 1.0.

```xml
<valueitem name="Opacity" value="[0.0..1.0]"/>
```

 | 0.2                                                                              | 0.8 |                                                                                 
 | ---                                                                              | --- |                                                                                 
 | <img class="tableImage" src="images/core/tracks/trackcoreopacity0_2.png"/>  | <img class="tableImage" src="images/core/tracks/trackcoreopacity0_8.png"/> | 

#### Scale

Scale multiplier, controls size of core symbol. Default is 1.0.

```xml
<valueitem name="Scale" value="[0.0..]"/>
```

 | 0.5                                                                            | 2.0 |                                                                             
 | ---                                                                            | --- |                                                                             
 | <img class="tableImage" src="images/core/tracks/trackcorescale0_5.png"/>  | <img class="tableImage" src="images/core/tracks/trackcorescale2.png"/> | 

#### DynamicScale

If true, symbol scale will be modified by the system so that smaller symbols will be displayed when zoomed out and larger symbols when zoomed in. Other scaling options will be applied in addition to this setting. Default is false.

```xml
<valueitem name="DynamicScale" value="<true|false>"/>
```

#### Symbology

"Symbology" controls type of symbol provider. "SymbolKeyField" controls the track property used as input to the symbol provider. For MilStd2525, the referenced field should contain the entire 15 character MilStd2525 symbol code. SymbologyEncoding determines how the contents of Symbology are handled. Default is MilStd2525.

```xml
<valueitem name="SymbolKeyField" value="<Track field containing symbol code>"/>
<valueitem name="Symbology" value="MilStd2525|NTDS|BitmapFile|MilSymCompact"/>
<valueitem name="SymbologyEncoding" value="MilStd2525|MilSymCompact"/>
```

 | Value         | Comment | Sample                                                                                    |   
 | -----         | ------- | ------                                                                                    |   
 | MilStd2525    | Uses 15-character symbol string specified in [Mil-Std-2525C](http://www.dtic.mil/doctrine//doctrine/other/ms_2525c.pdf) | <img class="tableImage" src="images/core/tracks/trackcoresymbology2525.png"/> | 
 | NTDS          | Four fist characters of 2525 code, uses battle dimension and standard identity to determine symbol | <img class="tableImage" src="images/core/tracks/trackcoresymbologyntds.png"/> | 
 | BitmapFile    | URI referencing png or jpg file. Note that this imaged should be scaled appropriately for display and must be reachable from the client. |  <img class="tableImage" src="images/core/tracks/trackcoresymbologybitmapfile.png"/> | 
 | MilSymCompact | Three character code, first denotes threat/identity, the second and third indicate heading or time old (X in symbol). MilStd2525 codes can be used and will be automatically mapped. Heading is indicated using triangle indicator within the symbol. The core symbol itself should not be rotated. |  <img class="tableImage" src="images/core/tracks/milsymcompact.png"/> | 




#### Drop shadow

Displays drop shadow beneath the core symbol. Default is off.

```xml
<valueitem name="DropShadow" value="<true|false>"/>
<valueitem name="DropShadowColor" value="<color>"/>
```

{% include callout.html content="Possible bug, unable to display drop shadow." type="warning" %}

#### Rotate

Rotates track symbol using the track course. Default is false.

```xml
<valueitem name="Rotate" value="<true|false>"/>
```

 | true                                                                               | false |                                                                                    
 | ----                                                                               | ----- |                                                                                    
 | <img class="tableImage" src="images/core/tracks/trackcoresymbolrotate.png"/>  | <img class="tableImage" src="images/core/tracks/trackcoresymbology2525.png"/> | 

### Speed vector

Contains style items for speed vector/movement indicator. The speed vector points along the specified course of the track, and is possibly scaled based on speed of track and map scale. Note that the core track symbol is drawn on top of the speed vector, the vector may not be visible if the symbol is opaque and the length is small.

```xml
<compositeitem name="SpeedVector">
  Speed vector style items
</compositeitem>
```

#### Basic appearance

Controls color and thickness of the speed vector. 

```xml
  <valueitem name="Color" value="<color>"/>
  <valueitem name="Thickness" value="<pixels>"/>
```

#### Speed vector length

Speed vector length is set using a combination of length type and length value.

```xml
  <valueitem name="LenType" value="<Meters|Seconds|Pixels>"/>
  <valueitem name="Len" value="<Length in unit set in LenType>"/>
```

 | LenType | Description                                                                                                                                                                           | 
 | ------- | -----------                                                                                                                                                                           | 
 | Meters  | Length is measured as fixed meters in the real world. Zooming in will make the vector longer                                                                                          | 
 | Seconds | Length is measured as seconds ahead in time at the specified speed of the track. The length of the vector in real world meters (speed * time). Zooming in will make the vector longer | 
 | Pixels  | Fixed pixel length. Zooming in will not change the length of the vector                                                                                                               | 

#### Movement indicator

Movement indicator is typically drawn for land units and consists of an offset line and a fixed length arrow pointing in the direction that the unit is moving (course). Length settings are ignored for tracks with movement indicator set to true. If course is not defined for a track, no indicator will be drawn. Default is false.

```xml
  <valueitem name="MovementIndicator" value="<true|false>"/>
```

 | Settings                                                                                                            | Visual                                                                                |        
 | --------                                                                                                            | ------                                                                                |        
 | Color=Green, Thickness=6, Len=180, LenType=Seconds | <img class="tableImage" src="images/core/tracks/trackcorespeed180s_6p.png"/>  | 
 | Color=Green, Thickness=6, Len=80, LenType=Pixels   | <img class="tableImage" src="images/core/tracks/trackcorespeed80p_6p.png"/>    | 
 | Color=Green, Thickness=2, Len=80, LenType=Pixels,<br/>MovementIndicator=true. <br/> Note that "Len" and "LenType" are ignored| <img class="tableImage" src="images/core/tracks/trackcorespeed180s_2p_mi.png"/> | 

### Labels

Track labels are used to display textual information around the track symbol. As for all other styling items, labels can be condtionally suppressed. This is often useful in order to avoid overcluttering the display.

```xml
<style>
  <mapscalecondition value="1:1M" op="Gt"/>
  <compositeitem name="Label" state="suppress"/>
</style>
```

Note that the item containing "suppress" must appear after any active label style in order to suppress labels.
#### Basic appearance

Basic label appearance is controlled by using standard style items. Colors include alpha, "r[0-255],g[0-255],b[0-255],a[0-255]", font names reference installed system fonts such as "Verdana" and "Arial". Font size can be specified using decimals, ie "12.5". Note that the style items located directly under <compositeitem name="Label">` provides a default only and can be overridden in each specific label text.

```xml
      <compositeitem name="Label">
        <valueitem name="Color" value="<color>"/>
        <valueitem name="Background" value="<color>"/>
        <valueitem name="FontName" value="<font name>"/>
        <valueitem name="FontSize" value="<point size>"/>
        <valueitem name="Bold" value="<true|false>"/>
        <valueitem name="Italic" value="<true|false>"/>
        <compositeitem name="LabelText1">
        	...
        </compositeitem>
        <compositeitem name="LabelText2">
        	...
        </compositeitem>
        ...
      </compositeitem>
```


#### Individial label texts

Any number of label texts can be added using "`<compositeitem name="LabelText<id>`">". However, using more than eight labels will not be practical due to layout issues. Simple label setup example:

```xml
  <compositeitem name="LabelText1">
    <valueitem name="Fields" value="ais.name"/>
    <valueitem name="Position" value="TopLeft"/>
  </compositeitem>
```

<img class="tableImage" src="images/core/tracks/trackcorelabelsimple.png"/> 

##### Label items details

```xml
<compositeitem name="LabelText<id>">
  <valueitem name="Fields" value="<comma separated track field-id list>"/>
  <valueitem name="FieldSeparator" value="<textual field separator>"/>
  <valueitem name="Position" value="<placement id>"/>
  <valueitem name="AlternativePositions" value="<comma separated placement ids>"/>
  <valueitem name="AlwaysShow" value="<true|false>"/>
  <valueitem name="XOffset" value="<pixels offset, positive x to the right>"/>
  <valueitem name="YOffset" value="<pixels offset, positive y down>"/>
  <valueitem name="SymbolMargin" value="<pixels to move text away from symbol>"/>
  <compositeitem name="LabelDecoration">
    <valueitem name="ValidPositions" value="<comma separated placement ids>"/>
    <valueitem name="MinDistance" value="<pixels>"/>
    <valueitem name="LineColor" value="<color>"/>
    <valueitem name="PixelWidth" value="<pixels>"/>
  </compositeitem>
</compositeitem>
```

 | Item name              | Description                                                                                                                                                                                                                    | Symbol                                                                                   |   
 | ---------              | -----------                                                                                                                                                                                                                    | ------                                                                                   |   
 | Fields, FieldSeparator | One or more track field ids. If no value for the field exists, no text will be displayed. Field separator will only be displayed if it is required to separate two fields. Fields="ais.name,ais.VESSELTYPE" FieldSeparator="/" |  <img class="tableImage" src="images/core/tracks/trackcorelabelwithseparator.png"/> | 
 | Position               | Label placement relative to symbol. See [track label declutter](maria_gdk/programming/functionality/styling/track/tracklabledeclutter)                                                                                         |                                                                                          |   
 | AlternativePositions   | Documented in [track label declutter](maria_gdk/programming/functionality/styling/track/tracklabledeclutter)                                                                                                                   |                                                                                          |   
 | AlwaysShow             | If "true", the label will be displayed even if it is overlaps other labels                                                                                                                                                     |                                                                                          |   
 | XOffset, YOffset       | Pixel offsets for label placement. Consider using "SymbolMargin" instead as this will automatically correct sign for left/right and top/bottom placement                                                                       |                                                                                          |   
 | SymbolMargin           | Pixels to move text away from symbol.<br/>`<valueitem name="SymbolMargin" value="10"/>` | <img class="tableImage" src="images/core/tracks/trackcorelabelmargin.png"/> | 
 | LabelDecoration        | Documented in [track label declutter](maria_gdk/programming/functionality/styling/track/tracklabledeclutter)                                                                                                                   |                                                                                          |   


In addition to the style items above, MaxDistance and AlwaysShow are described in detail in the documentation for [track label declutter](maria_gdk/programming/functionality/styling/track/tracklabledeclutter)

### Marking

Styled marking can be used to highlight track symbols. Marking is nearly always used in combination with a condition, for instance selection state. The marks consist of one or more layers of rectangles surrounding the track symbol. Type/color/thickness can be added directly under "Mark" element or be placed in layers. Unwrapped items are drawn at the bottom, then each wrapped item are drawn ordered alphabetically by name. Last layer is drawn on top.The basic structure is as follows:

```xml
<compositeitem name="Mark">
  <!-- Root layer settings -->  
  <compositeitem name="Layer1">      
    <!-- Layer1 settings -->  
  </compositeitem>
  <compositeitem name="Layer2">
    <!-- Layer2 settings -->  
  </compositeitem>
</compositeitem>
```

{% include callout.html content="Note that track selection marking should use this mechanism. The \"Selected\" style item is now obsolete and may be removed at a later time. To mark selected tracks, include the following condition: 
<br/><br/> `<statecondition key=\"Selected\" scope=\"PerItem\" state=\"Active\"/>`" type="info" %}

##### Mark elements

 | Item name  | Description                                                                                   | 
 | ---------  | -----------                                                                                   | 
 | Type       | Currently only "Rect" is supported. If not set, rectangle is assumed                          | 
 | Color      | Color with alpha                                                                              | 
 | Thickness  | Thickness in pixels                                                                           | 
 | SizeOffset | The shape drawn is inflated with this value, or deflated if negative. Size is given in pixels | 

Example:

```xml
<compositeitem name="Mark">
    <valueitem name="Color" value="0,0,0,192"/>
    <valueitem name="Thickness" value="4.0"/>
    <valueitem name="SizeOffset" value="2.0"/>
    <compositeitem name="Layer2">
      <valueitem name="Color" value="255,0,0,192"/>
      <valueitem name="Thickness" value="2.0"/>
      <valueitem name="SizeOffset" value="5.0"/>
    </compositeitem>
</compositeitem>
```

<img class="tableImage" src="images/core/tracks/trackmark.png"/> 


### History

Track history is used to display previously reported positions in the map. See [track history](maria_gdk/programming/functionality/styling/track/historystyling) for details.

```xml
<stylecategory name="TrackSymbol">
  <style>
    ...
    <compositeitem name="History">
      ...
    </compositeitem>
  </style>
</stylecategory>
```





