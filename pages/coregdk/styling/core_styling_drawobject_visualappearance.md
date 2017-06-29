---
title: Drawobject styling visual appearance
keywords: drawobjects
tags: [navigation]
sidebar: core_styling_sidebar
permalink: core_styling_drawobject_visualappearance.html
summary: Set default values for draw object fields based on styles with conditions to target objects with special characteristics.
---

# Styling visual appearance

The style set for the visual appearance has three style categories: DrawObjects, SelectionFan and Tooltip. Each category has a set of predefined styles. The name of the value items and composite value items in the styles have specific meaning and cannot be changed. Changing the value of the value items on the other hand will change the appearance of the draw objects. 

A complete style set for draw objects can be found here: [DrawObjectStyle](core_styling_drawobject_visualappearance_xml.html)

## DrawObjects style category

This style category has several styles for controling the appeatance of the draw objects and the tools for interacting with the draw objects.

### Styling draw object appearance

The main style which controls the appearance of the draw objects has some top level value items and several composite items.

Most of the value items shown bellow use variables to define the actual value to be used for the appearance of the draw object. This means that the value is retrieved from the fields of the draw object when the style is resolved for a draw object.

Some of the value items also have a *factor* attribute which is defined as the variable AlphaFactor. This factor value retrieved from the draw object's fields controls the opactity applied to the colors of the draw object.

The top level value items control the overall coloring of the draw objects:

```xml
<valueitem name="ColorScheme" value="Medium"/>
<valueitem name="OverrideColor" value=""/>
```

 | Name              | Description                                                                                      | 
 | ----              | -----------                                                                                      | 
 | **ColorScheme**   | Controls the colors used for MIL-STD-2525C objects and can be: Dark, Medium or Light             | 
 | **OverrideColor** | Used to override the color scheme for MIL-STD-2525C objects and can be: Black, White or DarkGray | 

The appearance of draw object lines, such as thickness and color, is controlled by the composite item **Line**:

```xml
<compositeitem name="Line">
  <valueitem name="Width" value="[LineWidth]" />
  <valueitem name="Color" value="[LineColor]" factor="[AlphaFactor]" />
  <valueitem name="ColorLight" value="[LineColor]" factor="[AlphaFactor]" />
  <valueitem name="ColorMedium" value="[LineColor]" factor="[AlphaFactor]" />
  <valueitem name="ColorDark" value="[LineColor]" factor="[AlphaFactor]" />
  <valueitem name="BeginScaleStep" value="1.0" />
  <valueitem name="EndScaleStep" value="1.0" />
  <valueitem name="DashStyle" value="[LineDashStyle]"/>
  <valueitem name="ShowLinePointText" value="[ShowLinePointText]"/>
</compositeitem>
```

 | Name                  | Description                                                                                                                                                                                                            | 
 | ----                  | -----------                                                                                                                                                                                                            | 
 | **Width**             | Thickness of draw object lines.                                                                                                                                                                                        | 
 | **Color**             | Color for generic draw object lines.                                                                                                                                                                                   | 
 | **ColorLight**        | Light color for MIL-STD-2525C draw object lines.                                                                                                                                                                       | 
 | **ColorMedium**       | Medium color for MIL-STD-2525C draw object lines.                                                                                                                                                                      | 
 | **ColorDark**         | Dark color for MIL-STD-2525C draw object lines.                                                                                                                                                                        | 
 | **BeginScaleStep**    | The scaling value of the line figure (arrow etc.) at the beginning of the line.                                                                                                                                        | 
 | **EndScaleStep**      | The scaling value of the line figure (arrow etc.) at the end of the line.                                                                                                                                              | 
 | **DashStyle**         | Dash style for draw object lines. Values are as defined by .NET. For example the value "4,6" would give a line where each dash is 4 times the thickness of the line and each gap is 6 times the thickness of the line. | 
 | **ShowLinePointText** | Indicates if the text associated with a line's points (the text must be set on the draw object's point primitives) should be shown. Values can be: true or false.                                                      | 

Some draw objects show text as part of the draw object. The composite item **DefaultFont** controls the appearance of the font for this text:

```xml
<compositeitem name="DefaultFont">
  <valueitem name="Bold" value="[BoldFont]"/>
  <valueitem name="Italic" value="[ItalicFont]"/>
  <valueitem name="Underline" value="[UnderlineFont]"/>
  <valueitem name="Strikethrough" value="[StrikethroughFont]"/>
  <valueitem name="Font" value="[DefaultFontName]"/>
  <valueitem name="Size" value="[DefaultFontSize]" factor="[ViewFontScale]"/>
  <valueitem name="Color" value="[DefaultFontForegroundColor]" factor="[AlphaFactor]"/>
  <valueitem name="BackgroundColor" value="[DefaultFontBackgroundColor]" factor="[AlphaFactor]"/>
  <valueitem name="MinFontSize" value="[MinFontSize]"/>
  <valueitem name="MinFontMapScale" value="[MinFontMapScale]"/>
  <valueitem name="MaxFontMapScale" value="[MaxFontMapScale]"/>
  <valueitem name="CustomFontScale" value="[CustomFontScale]"/>
</compositeitem>
```

 | Name                | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | 
 | ----                | -----------                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | 
 | **Bold**            | Sets bold font. Values are true or false.                                                                                                                                                                                                                                                                                                                                                                                                                                                     | 
 | **Italic**          | Sets italic font. Values are true or false.                                                                                                                                                                                                                                                                                                                                                                                                                                                   | 
 | **Underline**       | Sets underline font. Values are true or false.                                                                                                                                                                                                                                                                                                                                                                                                                                                | 
 | **Strikethrough**   | Sets strikethrough font. Values are true or false.                                                                                                                                                                                                                                                                                                                                                                                                                                            | 
 | **Font**            | Font name.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | 
 | **Size**            | Font size.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | 
 | **Color**           | Font color.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | 
 | **BackgroundColor** | Color used as a background for the text to increase readability.                                                                                                                                                                                                                                                                                                                                                                                                                              | 
 | **MinFontSize**     | Minimum font size used to scale text when custom font scaling is enabled.                                                                                                                                                                                                                                                                                                                                                                                                                     | 
 | **MinFontMapScale** | Minimum map scale used to scale text when custom font scaling is enabled.                                                                                                                                                                                                                                                                                                                                                                                                                     | 
 | **MaxFontMapScale** | Maximum map scale used to scale text when custom font scaling is enabled.                                                                                                                                                                                                                                                                                                                                                                                                                     | 
 | **CustomFontScale** | Enables custom font scaling and applies to generic objects. Values are true or false. If set to true the text will be scaled between MinFontSize at MinFontMapScale and the actual font size (Size) at MaxFontMapScale, where MinFontSize, MinFontMapScale, and MaxFontMapScale are field values of the generic object. If MinFontMapScale and MaxFontMapScale are 0 (not set), for generic objects other than Generic Text, the text will be scaled as normal untill MinFontSize is reached. | 

Some draw objects use a smaler font than the normal default font. Examples are the range text of Range Rings and ALT text on Alternating Traffic. The composite item **SmallLabelFont** controls the appearance of this font:

```xml
<compositeitem name="SmallLabelFont">
  <valueitem name="Font" value="Arial" />
  <valueitem name="Size" value="10.0" />
</compositeitem>
```

 | Name     | Description | 
 | ----     | ----------- | 
 | **Font** | Font name.  | 
 | **Size** | Font size.  | 

Some draw objects have an echelon indicator representing the command level. A separate font is used for the echelon indicator and the composite item **EchelonFont** controls the appearance of this font:

```xml
<compositeitem name="EchelonFont">
  <valueitem name="Font" value="Arial" />
  <valueitem name="Size" value="24.0" />
</compositeitem>
```

 | Name     | Description | 
 | ----     | ----------- | 
 | **Font** | Font name.  | 
 | **Size** | Font size.  | 

A few draw objects such as Generic Crosshairs use a separate font for the text associated with the draw object. The composite item **LabelFont** controls the appearance of this font:

```xml
<compositeitem name="LabelFont">
  <valueitem name="Font" value="Arial" />
  <valueitem name="Size" value="10.0" />
  <valueitem name="BackgroundColor" value="[DefaultFontBackgroundColor]" factor="[AlphaFactor]"/>
</compositeitem>
```

 | Name                | Description                                                      | 
 | ----                | -----------                                                      | 
 | **Font**            | Font name.                                                       | 
 | **Size**            | Font size.                                                       | 
 | **BackgroundColor** | Color used as a background for the text to increase readability. | 

Generic draw objects that define an area can be filled with a pattern. The composite item **Fill** controls the appearance of the fill pattern:

```xml
<compositeitem name="Fill">
  <valueitem name="ForegroundColor" value="[FillForegroundColor]" factor="[AlphaFactor]" />
  <valueitem name="BackgroundColor" value="[FillBackgroundColor]" factor="[AlphaFactor]" />
  <valueitem name="Style" value="[FillStyle]"/>
</compositeitem>
```

 | Name                | Description                                                                                                                   | 
 | ----                | -----------                                                                                                                   | 
 | **ForegroundColor** | Color of the fill pattern.                                                                                                    | 
 | **BackgroundColor** | Background color of the fill pattern.                                                                                         | 
 | **Style**           | Fill pattern, where values can be: None, Solid, BackwardDiagonal, ForwardDiagonal, Horizontal, Vertical, Cross, DiagonalCross | 

Generic polyarea and polyline draw objects can have a buffer around the draw object. The composite item **Buffer** controls the appearance of the buffer:

```xml
<compositeitem name="Buffer">
  <valueitem name="Width" value="[BufferLineWidth]" />
  <valueitem name="Color" value="[BufferLineColor]" factor="[AlphaFactor]" />
  <valueitem name="DashStyle" value="[BufferLineDashStyle]"/>
</compositeitem>
```

 | Name          | Description                                                                                                                                                                                                       | 
 | ----          | -----------                                                                                                                                                                                                       | 
 | **Width**     | Thickness of buffer line.                                                                                                                                                                                         | 
 | **Color**     | Color of buffer line.                                                                                                                                                                                             | 
 | **DashStyle** | Dash style for buffer lines. Values are as defined by .NET. For example the value "4,6" would give a line where each dash is 4 times the thickness of the line and each gap is 6 times the thickness of the line. | 

Point symbol draw objects can have labels around the symbol. The layout of the individual labels follows the MIL-STD-2525C standard. The composite item **Label** controls the appearance of the labels:

```xml
<compositeitem name="Label">
  <valueitem name="Draw" value="[DrawLabel]"/>
  <valueitem name="DrawMovementIndicator" value="[DrawMovementIndicator]"/>
  <valueitem name="MovementIndicatorColor" value="[MovementIndicatorColor]" factor="[AlphaFactor]"/>
  <valueitem name="DrawOffsetLocationIndicator" value="[DrawOffsetLocationIndicator]"/>
  <valueitem name="OffsetLocationIndicatorColor" value="[OffsetLocationIndicatorColor]" factor="[AlphaFactor]"/>
  <valueitem name="Font" value="[LabelFontName]"/>
  <valueitem name="Bold" value="[LabelFontBold]"/>
  <valueitem name="Italic" value="[LabelFontItalic]"/>
  <valueitem name="Size" value="[LabelFontSize]"/>
  <valueitem name="Color" value="[LabelFontForegroundColor]" factor="[AlphaFactor]"/>
</compositeitem>
```

 | Name                             | Description                                                              | 
 | ----                             | -----------                                                              | 
 | **Draw**                         | Sets the label visibility. Values are true or false.                     | 
 | **DrawMovementIndicator**        | Sets the movement indicator visibility. Values are true or false.        | 
 | **MovementIndicatorColor**       | Color of the movement indicator.                                         | 
 | **DrawOffsetLocationIndicator**  | Sets the offset location indicator visibility. Values are true or false. | 
 | **OffsetLocationIndicatorColor** | Color of the offset location indicator.                                  | 
 | **Font**                         | Label font name.                                                         | 
 | **Size**                         | Label font size.                                                         | 
 | **Bold**                         | Sets bold label font. Values are true or false.                          | 
 | **Italic**                       | Sets italic label font. Values are true or false.                        | 
 | **Color**                        | Label font color.                                                        | 

It is also possible to override the MIL-STD-2525C layout of the labels and specify the layout in the composite item **Label** instead:  

```xml
<compositeitem name="Label">
 
  <!-- The value items for MIL-STD-2525C labels must also 
       be included in addition to the items bellow -->
  
  <valueitem name="GenericLabel" value="[GenericLabel]" />
  <valueitem name="Separator" value="-" />
  <compositeitem name="LabelText1">
    <valueitem name="Fields" value="field1" />
    <valueitem name="Position" value="TopLeft" />
    <valueitem name="XOffset" value="0.0" />
    <valueitem name="YOffset" value="0.0" />
  </compositeitem>
  <compositeitem name="LabelText2">
    <valueitem name="Fields" value="field2,field3" />
    <valueitem name="Position" value="Bottom" />
    <valueitem name="XOffset" value="0.0" />
    <valueitem name="YOffset" value="0.0" />
  </compositeitem> 
</compositeitem>
```

 | Name             | Description                                                                                                | 
 | ----             | -----------                                                                                                | 
 | **GenericLabel** | Sets genric labeling mode. Values are true or false. A value of true will enable generic labeling.         | 
 | **Separator**    | Character used to separate several fields when multiple fields are combined into one label.                | 
 | **LabelText**    | Defines a label item. Label composite items must start with the name *LabelText*. \\ \\ <WRAP leftalign>
 | **Fields**       | A comma separated list of draw object fields to show as the label.                                         | 
 | **Position**     | Position of the label. Values are: Left, Right, Top, Bottom, TopLeft, TopRight, BottomLeft, BottomRight    | 
 | **XOffset**      | Horizontal offset of the label from its position in pixels.                                                | 
 | **YOffset**      | Vertical offset of the label from its position in pixels.                                                  | 
</WRAP> |

The appearance of point symbol draw objects is controlled by the composite item **Symbol**:

```xml
<compositeitem name="Symbol">
  <valueitem name="Size" value="100.0" factor="[ViewSymbolScale]"/>
  <valueitem name="DynamicScale" value="true"/>
  <valueitem name="Grayscale" value="false"/>
  <valueitem name="Alpha" value="[AlphaFactor]"/>
  <valueitem name="DropShadow" value="false"/>
  <valueitem name="DropShadowColor" value="255,255,255,255"/>
</compositeitem>
```

 | Name                | Description                                                                                      | 
 | ----                | -----------                                                                                      | 
 | **Size**            | Symbol size in percentage. (The symbol size will be scaled by the view's symbol scale as well)   | 
 | **DynamicScale**    | Sets if the symbol is scaled automatically according to the map scale. Values are true or false. | 
 | **Grayscale**       | Sets if the symbol is shown as gray scale. Values are true or false.                             | 
 | **Alpha**           | The symbol opacity. Values are between 0 and 1.                                                  | 
 | **DropShadow**      | Sets if the symbol should have a drop shadow. Values are true or false.                          | 
 | **DropShadowColor** | The color for the drop shadow.                                                                   | 

The composite item **Unrelated** contains styling elements that by definition are unrelated to a specific styling area:

```xml
<compositeitem name="Unrelated">
  <valueitem name="SimplifiedRenderingCutoffSize" value="[SimplifiedRenderingCutoffSize]" />
  <valueitem name="DrawOutline" value="[DrawOutline]" />
  <valueitem name="Smooth" value="[Smooth]" />
  <valueitem name="DefaultScaleStep" value="0.1"/>
</compositeitem>
```

 | Name                              | Description                                                                                                                                                        | 
 | ----                              | -----------                                                                                                                                                        | 
 | **SimplifiedRenderingCutoffSize** | The minimum size (width or height) in pixels of the draw object's bounding box before simplified rendering is enabled.                                             | 
 | **DrawOutline**                   | Sets if the outline for draw objects are shown. Currently only applies to filled FanAreas. Values are true or false.                                               | 
 | **Smooth**                        | Sets if lines are rendered as smooth, meaning the line segments will be curved between the actual points to give the line a smooth look. Values are true or false. | 
 | **DefaultScaleStep**              | Sets the scale step used by the tool for scaling a draw object's text.                                                                                             | 

### Highlight shape of selected objects

The appearance of selected objects can be controlled using style. 

Example to change width of line objects when selected:

```xml
   <style>
      <statecondition key="Selected" scope="PerItem" state="Active"/>
      <compositeitem name="Line">
        <valueitem name="Width" value="[LineWidth]" factor="2.75" />
      </compositeitem>
   </style>
```

### Mark draw objects

Styled marking can be used to highlight generic polylines, polyareas, ellipses and crosshairs. Marking is nearly always used in combination with a condition. The marks consist of one or more layers surrounding the geometry of the draw object. The basic structure is as follows:

```xml
    <style>
      <compositeitem name="Mark">
        <compositeitem name="Layer1">
          <valueitem name="DashStyle" value="Solid"/>
          <valueitem name="Color" value="255,192,0,255"/>
          <valueitem name="Width" value="2.0"/>
          <valueitem name="Offset" value="5"/>
          <valueitem name="Draw" value="true"/>
        </compositeitem>
        <compositeitem name="Layer2">
          <valueitem name="DashStyle" value="Solid"/>
          <valueitem name="Color" value="112,48,160,255"/>
          <valueitem name="Width" value="2.0"/>
          <valueitem name="Offset" value="5"/>
          <valueitem name="Draw" value="true"/>
        </compositeitem>
        <compositeitem name="Layer3">
          <valueitem name="DashStyle" value="Solid"/>
          <valueitem name="Color" value="0,176,80,255"/>
          <valueitem name="Width" value="2.0"/>
          <valueitem name="Offset" value="5"/>
          <valueitem name="Draw" value="true"/>
        </compositeitem>
      </compositeitem>
    </style>
```

 | Name          | Description                                                                                                                                                                                                         | 
 | ----          | -----------                                                                                                                                                                                                         | 
 | **DashStyle** | Dash style for mark layer. Values are as defined by .NET. For example the value “4,6” would give a line where each dash is 4 times the thickness of the line and each gap is 6 times the thickness of the line. | 
 | **Color**     | Color for mark layer.                                                                                                                                                                                               | 
 | **Width**     | Thickness of mark layer.                                                                                                                                                                                            | 
 | **Offset**    | The distance in pixels from the preceding mark layer or draw object geometry for the first mark layer.                                                                                                              | 
 | **Draw**      | Sets the mark layer visibility. Values are true or false.                                                                                                                                                           | 

### Conditional styles for draw object appearance

Draw objects that have a standard identity, the threat posed by the draw object, have colors according to the standard identity.

Individual styles represent the four basic standard identity categories: unknown, friend, neutral, and hostile. The standard identity styles modify the color value items of the composite item **Line** from the original style. A field condition, that uses the *StandardIdentity* value in the draw object's fields, is used to identify the different standard identity categories.

The style for the *hostile* standard identity category: 

```xml
<style>
  <fieldcondition field="StandardIdentity" op="In" value="Hostile,AssumedHostile,Joker,Faker,Suspect" /> 
  <compositeitem name="Line">
    <valueitem name="Color" value="255,0,0,255" factor="[AlphaFactor]" />
    <valueitem name="ColorLight" value="255,128,128,255" factor="[AlphaFactor]" />
    <valueitem name="ColorMedium" value="255,48,49,255" factor="[AlphaFactor]" /> 
    <valueitem name="ColorDark" value="200,0,0,255" factor="[AlphaFactor]"/>
  </compositeitem>
</style>
```

The style for the *unknown* standard identity category:

```xml
<style>
  <fieldcondition field="StandardIdentity" op="In" value="Unknown,Pending" />
  <compositeitem name="Line">
    <valueitem name="Color" value="255,255,0,255" factor="[AlphaFactor]" />
    <valueitem name="ColorLight" value="255,255,128,255" factor="[AlphaFactor]" />
    <valueitem name="ColorMedium" value="255,255,0,255" factor="[AlphaFactor]" />
    <valueitem name="ColorDark" value="225,220,0,255" factor="[AlphaFactor]" />
  </compositeitem>
</style>
```

The style for the *neutral* standard identity category:

```xml
<style>statndard identityfield="StandardIdentity" value="Neutral" />
  <compositeitem name="Line">
    <valueitem name="Color" value="0,128,0,255" factor="[AlphaFactor]" />
    <valueitem name="ColorLight" value="170,255,170,255" factor="[AlphaFactor]" />
    <valueitem name="ColorMedium" value="0,226,0,255" factor="[AlphaFactor]" />
    <valueitem name="ColorDark" value="0,160,0,255" factor="[AlphaFactor]" />
  </compositeitem>
</style>
```

The style for the *friend* standard identity category:

```xml
<style>
  <fieldcondition field="StandardIdentity" op="In" value="Friend,AssumedFriend" />
  <compositeitem name="Line">
    <valueitem name="Color" value="0,0,255,255" factor="[AlphaFactor]" />
    <valueitem name="ColorLight" value="128,224,255,255" factor="[AlphaFactor]" />
    <valueitem name="ColorMedium" value="0,168,220,255" factor="[AlphaFactor]" />
    <valueitem name="ColorDark" value="0,107,140,255" factor="[AlphaFactor]" />
  </compositeitem>
</style>
```

Draw objects that have a status, which refers to wether the draw object exists at its location or will in the future exist at that location, have a line style according to the status.

Individual styles represent the two status categories: present, and planned. The status styles modify the dash style value item of the composite item **Line** from the original style. A field condition, that uses the *TacticalStatus* value in the draw object's fields, is used to identify the different status categories.

The style for the *present* status:

```xml
<style>
  <fieldcondition field="TacticalStatus" value="PresentPosition" />
  <compositeitem name="Line">
    <valueitem name="DashStyle" value="Solid"/>
  </compositeitem>
</style>
```

The style for the *planned*, *anticipated*, *suspected*, and *on order* status:

```xml
<style>
  <fieldcondition field="TacticalStatus" op="In" value="Anticipated,OnOrder,Planned,Suspected" />
  <compositeitem name="Line">
    <valueitem name="DashStyle" value="3,3@0" />
  </compositeitem>
</style>
```

When draw objects get disabled, they get a gray appearance. A separate style is used for the disabled state and modify the line and fill colors, and sets a gray scale mode for symbols. A field condition, that uses the runtime field *Disabled*, is used to identify the disabled state:

```xml
<style>
  <fieldcondition field="runtime.Disabled" value="true"/>
  <compositeitem name="Line">
    <valueitem name="Color" value="127,127,127,127" />
    <valueitem name="ColorLight"  value="127,127,127,127" />
    <valueitem name="ColorMedium"  value="127,127,127,127" />
    <valueitem name="ColorDark"  value="127,127,127,127" />
  </compositeitem>

  <compositeitem name="Symbol">
    <valueitem name="Grayscale" value="true" />
  </compositeitem>
  
  <compositeitem name="Fill">
    <valueitem name="ForegroundColor" value="127,127,127,127" />
    <valueitem name="BackgroundColor" value="127,127,127,127" />
  </compositeitem>      
</style>
```

There is also a style that hides the labels of a point symbol draw object during map pan operations. It is recommended to have this style because it gives the system better performance during a map pan operation:

```xml
<style>
  <mapstatecondition op="Eq" value="Changing" />
  <compositeitem name="Label">
    <valueitem name="Draw" value="false"/>
  </compositeitem>
</style>
```

### Styling draw object tool appearance

Maria GDK contains a set of tools for editing draw objects visually on the map. These tools range from object scaling and moving to adding and deleting the points of the draw object, and more. All of these tools use a style to control their appearance.

The composite item **Tool** controls the general appearance of all the tools:

```xml
<compositeitem name="Tool">
  <valueitem name="LineColor" value="0,0,0,255"  />
  <valueitem name="Width" value="1.0" />
  <valueitem name="HandleRadius" value="4.0"/>
  <valueitem name="HandleDistance" value="25.0" />
</compositeitem>   
```

 | Name               | Description                                                | 
 | ----               | -----------                                                | 
 | **LineColor**      | Color of the tool lines.                                   | 
 | **Width**          | Thickness of the tool lines.                               | 
 | **HandleRadius**   | Radius of the tool handles used to interact with the tool. | 
 | **HandleDistance** | Lenght of handle for rotating a draw object.               | 

The composite item **ToolGeoDecoration** controls the decorations that the tools display, such as text and dipsplacement:

```xml
<compositeitem name="ToolGeoDecoration">
  <valueitem name="Color" value="128,128,128,255"  />
  <valueitem name="BackgroundColor" value="255,255,255,127" />
  <valueitem name="Width" value="2.0" />
  <valueitem name="Font" value="Arial" />
  <valueitem name="Size" value="12.0" />
</compositeitem>
```

 | Name                | Description                                                      | 
 | ----                | -----------                                                      | 
 | **Color**           | Line and text color for geo decorations.                         | 
 | **BackgroundColor** | Color used as a background for the text to increase readability. | 
 | **Width**           | Thickness of geo decoration lines.                               | 
 | **Font**            | Font name for geo decoration text.                               | 
 | **Size**            | Font size for geo decoration text.                               | 

The composite item **PointSquareHandle** controls the colors of the tool handles:

```xml
<compositeitem name="PointSquareHandle">
  <valueitem name="NonSelectedColor" value="245,245,245,255" />
  <valueitem name="NonSelectedHoverColor" value="255,255,255,255" />
  <valueitem name="SelectedColor" value="255,255,0,255" />
  <valueitem name="SelectedHoverColor" value="255,255,225,255"/>
</compositeitem>
```

 | Name                      | Description                                                                         | 
 | ----                      | -----------                                                                         | 
 | **NonSelectedColor**      | Color of the unselected tool handle.                                                | 
 | **NonSelectedHoverColor** | Color of the unselected tool handle when the mouse pointer is over the tool handle. | 
 | **SelectedColor**         | Color of the selected tool handle.                                                  | 
 | **SelectedHoverColor**    | Color of the selected tool handle when the mouse pointer is over the tool handle.   | 

The composite item **ScaleTool** controls the text scaling tool: 

```xml
<compositeitem name="ScaleTool">
  <valueitem name="Width" value="25.0" />
  <valueitem name="Height" value="8.0"/>
</compositeitem>  
```

 | Name       | Description                     | 
 | ----       | -----------                     | 
 | **Width**  | Width of the text scale arrow.  | 
 | **Height** | Height of the text scale arrow. | 

The composite item **Rotate** controls the rotation tool handle colors:

```xml
<compositeitem name="Rotate">
  <valueitem name="Color" value="212,255,212,255" />
  <valueitem name="HoverColor" value="241,255,241,255"/>
</compositeitem>
```

 | Name           | Description                                                                       | 
 | ----           | -----------                                                                       | 
 | **Color**      | Color of the rotation tool handle.                                                | 
 | **HoverColor** | Color of the rotation tool handle when the mouse pointer is over the tool handle. | 

The composite item **AddTool** controls the tool handles for the add points tool:

```xml
<compositeitem name="AddTool">
  <valueitem name="Radius" value="3.0" />
  <valueitem name="NonSelectedColor" value="212,255,212,255"  />
  <valueitem name="NonSelectedHoverColor" value="241,255,241"  />
  <valueitem name="SelectedColor" value="212,255,212,255"/>
</compositeitem>
```

 | Name                      | Description                                                                         | 
 | ----                      | -----------                                                                         | 
 | **Radius**                | Radius of the tool handles used to interact with the tool.                          | 
 | **NonSelectedColor**      | Color of the unselected tool handle.                                                | 
 | **NonSelectedHoverColor** | Color of the unselected tool handle when the mouse pointer is over the tool handle. | 
 | **SelectedColor**         | Color of the selected tool handle.                                                  | 

## SelectionFan style category

When multiple draw objects can be selected at the same point by clicking on the map, a selection fan is displayed with all the draw objects that can be selected. The appearance of this selection fan is controlled by a style.

The value items at the root level of the style control the appearance of the actual fan control used to display the draw objects:

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

 | Name                        | Description                                                                                                                                                                            | 
 | ----                        | -----------                                                                                                                                                                            | 
 | **InnerFanBackgroundColor** | The inner color of the background gradient color.                                                                                                                                      | 
 | **OuterFanBackgroundColor** | The outer color of the background gradient color.                                                                                                                                      | 
 | **FanBackgroundColor**      | The center color of the selection fan.                                                                                                                                                 | 
 | **EmptySymbolColor**        | Color used for a draw object if no preview for it exists.                                                                                                                              | 
 | **OuterFanOutlineColor**    | The outer outline color.                                                                                                                                                               | 
 | **OuterOutlineThickness**   | Line thickness of outer outline.                                                                                                                                                       | 
 | **OutlineColor**            | The inner outline color.                                                                                                                                                               | 
 | **OutlineThickness**        | Line thickness of inner outline.                                                                                                                                                       | 
 | **MaxCount**                | Maximum number of objects that the fan can show. If more than maximum number of objects can be selected, the fan will not be shown. This is to prevent the fan from being overcrowded. | 

The composite item **Selected** controls the appearance of the selection rectangle for selected draw objects in the fan:

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

The composite item **Label** controls the appearance of the label that can be shown for the draw objects in the fan:

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

 | Name               | Description                                                                                 | 
 | ----               | -----------                                                                                 | 
 | **FontSize**       | Font size.                                                                                  | 
 | **FontName**       | Font name.                                                                                  | 
 | **Bold**           | Sets bold label font. Values are true or false.                                             | 
 | **Italic**         | Sets italic label font. Values are true or false.                                           | 
 | **Color**          | Font color.                                                                                 | 
 | **Background**     | Color used as a background for the text to increase readability.                            | 
 | **Show**           | Sets the label visibility. Values are true or false.                                        | 
 | **Fields**         | A comma separated list of draw object fields to show as the label.                          | 
 | **FieldSeparator** | Character used to separate several fields when multiple fields are combined into one label. | 

## Tooltip style category

When the mouse pointer is over a draw object, a tooltip for the draw object will be shown. The appearance of this tooltip is controlled by a style.

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
 | **Fields**         | A comma separated list of draw object fields to show as the tooltip line.                          | 
 | **FieldSeparator** | Character used to separate several fields when multiple fields are combined into one tooltip line. | 

