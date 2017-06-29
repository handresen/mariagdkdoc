---
title: Track label decluttering styling
keywords: styling
tags: [navigation]
sidebar: core_styling_sidebar
permalink: core_styling_track_labeldeclutter.html
summary: How to layout track text labels in Maria GDK. 
---

# Track label declutter

## Overview

Track label declutter is used to layout track text labels in order to make the labels readable even in situations when there are a large number of tracks present on the screen.

{% include image.html file="./core/tracks/maria2012_tracklabeldeclutter_html_6960b690.png" caption="No label declutter" %}

{% include image.html file="./core/tracks/maria2012_tracklabeldeclutter_html_7f4afee1.png" caption="Name label is decluttered" %}

Track label decluttering is done by moving labels to alternative positions relative to the track symbol. Label positions that do not overlap existing symbols or other track labels are preferred over positions that do overlap. Positions close to the track symbol are preferred over those that are far away. 

Label declutter/layout is performed at fixed intervals, typically every second. The layout step is performed independently (asynchronously) from the symbol and label rendering in order to avoid reduced rendering performance. As a result, some time may pass between track or map movement and label position updates. 

Note that the labels will change positions if the tracks or the map is not stationary. The declutter algorithm attempts to keep label stability while maintaining agile layout. Layout stability/agility may be tuned further by Teleplan Globe in the future.

## Usage

Track label declutter is controlled using track styling (see separate documentation). It is possible to control label decluttering in a fine grained manner by combining style conditions with declutter settings.

Example style for decluttering label names:

```xml
    <style>
    <!--Test style for label declutter/high priority tracks-->
    <!--Use a simple test condition-->
      <ms2525condition field="symbol.2525code" op="Or">
        <entry field="Identity" value="F"/>
        <entry field="Identity" value="A"/>
      </ms2525condition>
1>>   <valueitem name="RelDispPri" action="add" value="1.0"/>
      <compositeitem name="Label">
        <compositeitem name="LabelText3">
          <valueitem name="Bold" value="true"/>
          <valueitem name="Color" value="128,128,255,255"/>
          <valueitem name="FontSize" value="12.0"/>
          <valueitem name="Fields" value="name,ais.name"/>
          <valueitem name="Position" value="TopLeft"/>
2>>       <valueitem name="AlternativePositions" value="TopLeft,TopRight,DynamicXY"/>
3>>       <valueitem name="AlwaysShow" value="true"/>
4>>       <valueitem name="MaxDistance" value="200"/>
          <valueitem name="XOffset" value="0"/>
          <valueitem name="YOffset" value="-15"/>
5>>       <valueitem name="SymbolMargin" value="2"/>
6>>       <compositeitem name="LabelDecoration">
7>>         <valueitem name="ValidPositions" value="TopLeft,TopRight,DynamicXY"/>
8>>         <valueitem name="MinDistance" value="10"/>
9>>         <valueitem name="LineColor" value="50,50,250,64"/>
10>>        <valueitem name="PixelWidth" value="3.0"/>
          </compositeitem>
        </compositeitem>
      </compositeitem>
    </style>
```

This style block enables label declutter for tracks with id/threat “friendly” or “assumed friendly” (ms2525condition)

### Style elements used in decluttering

The track symbol style has been extended to support track label decluttering. The extensions can be used independently of decluttering.

#### Individual label appearance

Label appearance can now be controlled for each single label item. The same settings can be applied to “LabelText`<n>`” as to the parent “Label” item. This includes “FontSize”, “FontName”, “Color”, “Background”, “Bold” and “Italic”.

The parents “Label” settings are used as default. Zero or more label appearance settings can be applied to specific label items.

#### Action “add”

```xml
1>> <valueitem name="RelDispPri" action="add" value="1.0"/>
```

Action can be “add” or “replace”. “replace” is default behavior. If a valueitem with the same name is already active, its value is replaced. Action “add” performs numerical addition of an existing value and the new value.

“add” was introduced in order to allow boosting of display priorities for certain groups of items, for instance high priority tracks. Track priority is used internally when decluttering or suppressing track labels. As a general rule, labels for tracks with high priorities are more likely to be displayed.

#### AlternativePositions

```xml
2>> `<valueitem name="AlternativePositions" value="TopLeft,TopRight,DynamicXY"/>
```

To enable label declutter, specify alternative label positions using this valueitem.

 | Value                                                   | Description                                                                                                                                                                                                                          | 
 | -----                                                   | -----------                                                                                                                                                                                                                          | 
 | Top, Bottom                                             | Directly above or below symbol. “SymbolMargin” shifts the label up or down respectively with indicated number of pixels                                                                                                          | 
 | TopLeft, TopRight, Left, Right, BottomLeft, BottomRight | Left or right sides of the symbol. “Top`<l/r side>`” and “Bottom`<l/r side>`” aligns label with top of symbol. “SymbolMargin” shifts label to the left for left sided placements and to the right for right sided placements | 
 | DynamicXY                                               | Exact placement is controlled by the declutter/layout system.                                                                                                                                                                        | 

Note that “DynamicXY” is not required in order to facilitate simple label declutter. If more than one alternative is indicated, the layout system will attempt to place the label at the position that produces the least overlaps.

If both concrete placements and “DynamicXY” are specified, the layout system will tend to prefer concrete placements over “DynamicXY”.

#### AlwaysShow

```xml
3>> <valueitem name="AlwaysShow" value="true"/>
```

Forces label rendering even if the label overlaps existing labels. If not set (default), labels are drawn sorted by track display priority. Any overlap with existing labels causes the new label to be suppressed.

#### MaxDistance

```xml
4>> <valueitem name="MaxDistance" value="200"/>
```

Max distance for “DynamicXY” label placement. By increasing this value, label placements further away from the symbol are allowed.

#### SymbolMargin

```xml
5>> <valueitem name="SymbolMargin" value="2"/>
```

Adds extra spacing between symbol and label. Automatically determines the direction of spacing based on label placement.

#### LabelDecorations

```xml
6>> <compositeitem name="LabelDecoration">
7>>   <valueitem name="ValidPositions" value="TopLeft,TopRight,DynamicXY"/>
8>>   <valueitem name="MinDistance" value="10"/>
9>>   <valueitem name="LineColor" value="50,50,250,64"/>
10>>  <valueitem name="PixelWidth" value="3.0"/>
    </compositeitem>
```

Composite style item for specifying decorations associated with a label. Currently a simple line between label and the symbol is supported. This can be changed in the future.

Labels placed at one of the “ValidPositions” used within “LabelDecoration” can be drawn with a decoration. In addition, distance between label center and track symbol center must be at least “MinDistance”, or the decoration will be suppressed.

“LineColor” and “PixelWidth” control decoration appearance.

### Internal system setup

If track rendering setup is controlled by the system integrator, a “TrackLabelSetupRenderer” and “TrackLabelDecorationRenderer” must be added to the “TrackRenderManager” (see code below). Maria NG tester already implements this setup. Also note that “TrackLabelSetupRenderer” must be added before other track label renderers.

```csharp
namespace MariaNGTester.Objects
{
  public class TrackRendererWithClusteringFactory: ITrackRendererFactory
  {

    private readonly IGeoUnitsSetting_geoUnitsSetting;

    public TrackRendererWithClusteringFactory(IGeoUnitsSettinggeoUnitsSetting)
    {
      _geoUnitsSetting = geoUnitsSetting;
    }

    public ITrackRenderManagerNew()
    {
      ITrackRenderManager_trackRenderManager = newTrackRenderManager();
      _trackRenderManager.Renderers.Add(newTrackGridRenderer());
      _trackRenderManager.Renderers.Add(newTrackHistoryRenderer());
      _trackRenderManager.Renderers.Add(newTrackCourseSpeedVectorRenderer());
      _trackRenderManager.Renderers.Add(newTrackSimpleCellRenderer());
>>>   _trackRenderManager.Renderers.Add(newTrackLabelSetupRenderer%%(_%%geoUnitsSetting));
>>>   _trackRenderManager.Renderers.Add(newTrackLabelDecorationRenderer());
      _trackRenderManager.Renderers.Add(newTrackSymbolRenderer());
      _trackRenderManager.Renderers.Add(newTrackLabelRenderer());
      return_trackRenderManager;
    }
  }
}
```

