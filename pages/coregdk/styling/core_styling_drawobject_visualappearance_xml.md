---
title: Drawobject visual appearance xml
keywords: drawobjects, xml, example, styling
tags: [drawobject]
sidebar: core_styling_sidebar
permalink: core_styling_drawobject_visualappearance_xml.html
summary: Complete style set targeting draw object visual appearance.
---

# DrawObjectStyle.xml

```xml
<?xml version="1.0" encoding="utf-8" ?>
<styleset name="Default">
  <stylecategory name="DrawObjects">
    <style>
      <valueitem name="ColorScheme" value="Medium"/>
      <valueitem name="OverrideColor" value=""/>
      <compositeitem name="Line">
        <valueitem name="Width" value="[LineWidth]" />
        <valueitem name="Color" value="[LineColor]" factor="[AlphaFactor]" />
        <valueitem name="ColorLight" value="[LineColor]" factor="[AlphaFactor]" />
        <valueitem name="ColorMedium" value="[LineColor]" factor="[AlphaFactor]" />
        <valueitem name="ColorDark" value="[LineColor]" factor="[AlphaFactor]" />
        <valueitem name="BeginScaleStep" value="1.0" />
        <valueitem name="EndScaleStep" value="1.0" />
        <valueitem name="DashStyle" value="[LineDashStyle]"/>        
        <valueitem name="ShowLinePointText" value="[ShowLinePointText]" />
      </compositeitem>

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

      <compositeitem name="SmallLabelFont">
        <valueitem name="Font" value="Arial" />
        <valueitem name="Size" value="10.0" />
      </compositeitem>

      <compositeitem name="EchelonFont">
        <valueitem name="Font" value="Arial" />
        <valueitem name="Size" value="24.0" />
      </compositeitem>

      <compositeitem name="LabelFont">
        <valueitem name="Font" value="Arial" />
        <valueitem name="Size" value="10.0" />
        <valueitem name="BackgroundColor" value="[DefaultFontBackgroundColor]" factor="[AlphaFactor]" />
      </compositeitem>

      <compositeitem name="Fill">
        <valueitem name="ForegroundColor" value="[FillForegroundColor]" factor="[AlphaFactor]" />
        <valueitem name="BackgroundColor" value="[FillBackgroundColor]" factor="[AlphaFactor]" />
        <valueitem name="Style" value="[FillStyle]" />
      </compositeitem>

      <compositeitem name="Buffer">
        <valueitem name="Width" value="[BufferLineWidth]" />
        <valueitem name="Color" value="[BufferLineColor]" factor="[AlphaFactor]" />
        <valueitem name="DashStyle" value="[BufferLineDashStyle]" />
      </compositeitem>

      <compositeitem name="Label">
        <valueitem name="Draw" value="[DrawLabel]" />
        <valueitem name="DrawMovementIndicator" value="[DrawMovementIndicator]" />
        <valueitem name="MovementIndicatorColor" value="[MovementIndicatorColor]" factor="[AlphaFactor]"/>
        <valueitem name="DrawOffsetLocationIndicator" value="[DrawOffsetLocationIndicator]" />        
        <valueitem name="OffsetLocationIndicatorColor" value="[OffsetLocationIndicatorColor]" factor="[AlphaFactor]"/>
        <valueitem name="Font" value="[LabelFontName]" />
        <valueitem name="Bold" value="[LabelFontBold]" />
        <valueitem name="Italic" value="[LabelFontItalic]" />
        <valueitem name="Size" value="[LabelFontSize]" />
        <valueitem name="Color" value="[LabelFontForegroundColor]" factor="[AlphaFactor]" />
        
        <!-- Uncomment this section to use generic labeling
        <valueitem name="GenericLabel" value="[GenericLabel]" />
        <valueitem name="Separator" value="-" />
        <compositeitem name="LabelText">
          <valueitem name="Fields" value="field1,field2" />
          <valueitem name="Position" value="Bottom" />
          <valueitem name="XOffset" value="0.0" />
          <valueitem name="YOffset" value="0.0" />
        </compositeitem> 
        -->
      </compositeitem>

      <compositeitem name="Symbol">
        <valueitem name="Size" value="100.0" factor="[ViewSymbolScale]" />
        <valueitem name="DynamicScale" value="true" />
        <valueitem name="Grayscale" value="false" />
        <valueitem name="Alpha" value="[AlphaFactor]" />
        <valueitem name="DropShadow" value="false"/>
        <valueitem name="DropShadowColor" value="255,255,255,255"/>
      </compositeitem>

      <compositeitem name="Unrelated">
        <valueitem name="SimplifiedRenderingCutoffSize" value="[SimplifiedRenderingCutoffSize]" />
        <valueitem name="DrawOutline" value="[DrawOutline]" />
        <valueitem name="Smooth" value="[Smooth]" />
        <valueitem name="DefaultScaleStep" value="0.1" />
      </compositeitem>
    </style>

    <style>
      <mapstatecondition op="Eq" value="Changing" />
      <compositeitem name="Label">
        <valueitem name="Draw" value="false"/>
      </compositeitem>
    </style>
    
    <style>
      <fieldcondition field="StandardIdentity" op="In" value="Hostile,AssumedHostile,Joker,Faker,Suspect" />
      <compositeitem name="Line">
        <valueitem name="Color" value="255,0,0,255" factor="[AlphaFactor]" />
        <valueitem name="ColorLight" value="255,128,128,255" factor="[AlphaFactor]" />
        <valueitem name="ColorMedium" value="255,48,49,255" factor="[AlphaFactor]" />
        <valueitem name="ColorDark" value="200,0,0,255" factor="[AlphaFactor]" />
      </compositeitem>
    </style>

    <style>
      <fieldcondition field="StandardIdentity" op="In" value="Unknown,Pending" />
      <compositeitem name="Line">
        <valueitem name="Color" value="255,255,0,255" factor="[AlphaFactor]" />
        <valueitem name="ColorLight" value="255,255,128,255" factor="[AlphaFactor]" />
        <valueitem name="ColorMedium" value="255,255,0,255" factor="[AlphaFactor]" />
        <valueitem name="ColorDark" value="225,220,0,255" factor="[AlphaFactor]" />
      </compositeitem>
    </style>

    <style>
      <fieldcondition field="StandardIdentity" value="Neutral" />
      <compositeitem name="Line">
        <valueitem name="Color" value="0,128,0,255" factor="[AlphaFactor]" />
        <valueitem name="ColorLight" value="170,255,170,255" factor="[AlphaFactor]" />
        <valueitem name="ColorMedium" value="0,226,0,255" factor="[AlphaFactor]" />
        <valueitem name="ColorDark" value="0,160,0,255" factor="[AlphaFactor]" />
      </compositeitem>
    </style>

    <style>
      <fieldcondition field="StandardIdentity" op="In" value="Friend,AssumedFriend" />
      <compositeitem name="Line">
        <valueitem name="Color" value="0,0,255,255" factor="[AlphaFactor]" />
        <valueitem name="ColorLight" value="128,224,255,255" factor="[AlphaFactor]" />
        <valueitem name="ColorMedium" value="0,168,220,255" factor="[AlphaFactor]" />
        <valueitem name="ColorDark" value="0,107,140,255" factor="[AlphaFactor]" />
      </compositeitem>
    </style>

    <style>
      <fieldcondition field="TacticalStatus" value="PresentPosition" />
      <compositeitem name="Line">
        <valueitem name="DashStyle" value="Solid"/>
      </compositeitem>
    </style>

    <style>
      <fieldcondition field="TacticalStatus" op="In" value="Anticipated,OnOrder,Planned,Suspected" />
      <compositeitem name="Line">
        <valueitem name="DashStyle" value="3,3@0" />
      </compositeitem>
    </style>

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

    <style>
      <compositeitem name="ToolGeoDecoration">
        <valueitem name="Color" value="128,128,128,255"  />
        <valueitem name="BackgroundColor" value="255,255,255,127" />
        <valueitem name="Width" value="2.0" />
        <valueitem name="Font" value="Arial" />
        <valueitem name="Size" value="12.0" />
      </compositeitem>

      <compositeitem name="ScaleTool">
        <valueitem name="Width" value="25.0" />
        <valueitem name="Height" value="8.0" />
      </compositeitem>

      <compositeitem name="Rotate">
        <valueitem name="Color" value="212,255,212,255" />
        <valueitem name="HoverColor" value="241,255,241,255"  />
      </compositeitem>

      <compositeitem name="AddTool">
        <valueitem name="Radius" value="3.0" />
        <valueitem name="NonSelectedColor" value="212,255,212,255"  />
        <valueitem name="NonSelectedHoverColor" value="241,255,241"  />
        <valueitem name="SelectedColor" value="212,255,212,255" />
      </compositeitem>

      <compositeitem name="Tool">
        <valueitem name="LineColor" value="0,0,0,255"  />
        <valueitem name="Width" value="1.0" />
        <valueitem name="HandleRadius" value="4.0"/>
        <valueitem name="HandleDistance" value="25.0" />
      </compositeitem>

      <compositeitem name="PointSquareHandle">
        <valueitem name="NonSelectedColor" value="245,245,245,255" />
        <valueitem name="NonSelectedHoverColor" value="255,255,255,255" />
        <valueitem name="SelectedColor" value="255,255,0,255" />
        <valueitem name="SelectedHoverColor" value="255,255,225,255" />
      </compositeitem>
    </style>
  </stylecategory>
  <stylecategory name="SelectionFan">
    <style>
      <valueitem name="InnerFanBackgroundColor" value="255,255,255,0"/>
      <valueitem name="OuterFanBackgroundColor" value="255,255,255,200"/>
      <valueitem name="FanBackgroundColor" value="255,255,255,0"/>
      <valueitem name="EmptySymbolColor" value="200,200,200,255"/>
      <valueitem name="OuterFanOutlineColor" value="0,0,0,100"/>
      <valueitem name="OuterOutlineThickness" value="1"/>
      <valueitem name="MaxCount" value="10"/>  
      <valueitem name="OutlineColor" value="0,0,0,100"/>
      <valueitem name="OutlineThickness" value="2"/>
 
      <compositeitem name="Selected">    
        <valueitem name="Color" value="255,255,0,255"/>
        <valueitem name="Thickness" value="4.0"/>
      </compositeitem>
 
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
    </style>
  </stylecategory >
  <stylecategory name="Tooltip">
    <style>
      <valueitem name="ShowDelay" value="500"/>
      <valueitem name="HideDelay" value="5000"/>
      <valueitem name="TopBackgroundColor" value="255,255,255,255"/>
      <valueitem name="BottomBackgroundColor" value="228,228,240,255"/>
      <valueitem name="OutlineColor" value="0,0,0"/>
      <valueitem name="OutlineThickness" value="1"/>
      <valueitem name="FontColor" value="0,0,0"/>
      <valueitem name="FontFamilyName" value="Segoe UI"/>
      <valueitem name="FontSize" value="10"/>
      <compositeitem name="TooltipLine1">
        <valueitem name="Label" value="Name:"/>
        <valueitem name="Fields" value="Name"/>
        <valueitem name="FieldSeparator" value="-"/>
      </compositeitem>
    </style>
  </stylecategory >
</styleset>

```

