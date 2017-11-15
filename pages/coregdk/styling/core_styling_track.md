---
title: Track Styling
keywords: tracks, styling
tags: [track]
sidebar: core_styling_sidebar
permalink: core_styling_track.html
summary: Maria GDK features powerful mechanisms for controlling track appearance and styling. Several aspects of track visualisation can be controlled using styling. Conditional styling allows styling based on track attributes, map attributes and external settings. 
---

## Introduction

Styling elements include:


*  Symbol size and symbol scaling control 

*  Type of symbology 

*  Label contents, fonts, size and placement 

*  [History](./core_styling_track_history.html) limits and appearance 

*  Speed vector appearance 

*  Clustering groups, fill color and outline 

*  Visualisation of multi select 

Style element details can be found [here](./core_styling_track_details.html).

## Background

Maria GDK track styling is based on the styling core used in MARIA OMM (object management module). The style definitions have been simplified based on experience with OMM styling.

## Styling overview

Styles are defined using XML-data. At the time of writing, this is the preferred method for style definitions. Other ways of defining styles may be added in the future.

Using the NGTester app, it is very easy to experiment with different styling definitions. Select the “Track style editor” tab, modify some style value, click “Apply” and observe the result.

{% include image.html file="core/maria_2102_track_styling_html_3df3b6fa.png" caption="MilStd2525" %}

{% include image.html file="core/maria_2102_track_styling_html_m4dbc1ada.png" caption="NTDS" %}

Very simple track styling can be defined as follows:

```xml
<styleset name="Default">
  <stylecategory name="TrackSymbol">
    <style>
      <valueitem name="Visible"value="true"/>
      <compositeitem name="CoreSymbol">
        <valueitem name="Opacity"value="1.0"/>
        <valueitem name="Scale"value="1.5"/>
        <valueitem name="DynamicScale"value="false"/>
        <valueitem name="SymbolKeyField"value="symbol.2525code"/>
        <valueitem name="Symbology"value="MilStd2525"/>
      </compositeitem>
    </style>
  </stylecategory>
</styleset>
```

By using this style definition, all tracks are rendered using Mil standard 2525 symbology (`<valueitem name="Symbology" value="MilStd2525"/>`) and symbols are fixed size at all map scales (`<valueitem name="DynamicScale" value="false"/>`). In order to change symbology to NTDS, simply change the Symbology item to `<valueitem name="Symbology" value="NTDS"/>`

When drawing the actual track, the values in “valueitem”-elements are used as input. All styling ultimately boils down to generating the appropriate valueitems for a given track. In the example above, this is very straightforward; the same valueitems are used for every track. Real life styling definitions will typically include conditional styling, allowing tracks to be displayed with individual styling or to change visualisation based on system settings or map properties.

## Styling and client trackstores

The top-level item of the style definition is the “styleset”. A styleset contains a collection of style categories covering different aspects of track visualisation. See [categories](core_styling_track_details.html) for details.

The style set and categories are represented by interfaces as follows:

{% include image.html file="./core/maria_2102_track_styling_html_m573ba0f5.png" %}

Stylesets are used by a client side track store within the Maria GDK track core to perform track styling. The track stores are accessible through *ITrackLayerViewModel.ClientTrackStores*.

{% include image.html file="./core/maria_2102_track_styling_html_m557004a4.png" %}

The styleset can be set either directly through *IClientTrackStoreManager.TrackStores[id].Styleset* or by calling *ITrackLayerViewModel.SetTracksStyle(id, xmlRep)*. The NGTester application uses the last method to set track styling.

Note that the tracks contained in theclient trackstores are continuously updated by the Maria GDK track core. This is done by fetching tracks from a track service and applying the service data packages to the client track store.

### Style evaluation and display tracks

Understanding how the styles are evaluated is important in order create complex styles.

Each client track contains a collection of resolved style items. The final style item values are based on:


*  The styleset contained in the parent *IClientTrackStore* 

*  Track data 

*  External input represented by *IItemContext*. External input includes object states such as selection, user input and named variables 

The resolved styles are contained in *IClientTrack.ActiveStyles*. Using the style example below, *ActiveStyles* contains one entry, “TrackSymbol”.

Each style category in the styleset contains one or more styles. When these are resolved, only concrete style items will remain. For a given track, each *style* entry in the definition is either included or not included based on the style condition.


#### Track specific conditions

 | Name            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | 
 | ----            | -----------                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | 
 | SpeedCondition  | Used to test for track speed. Contains a speed value that can be defined using a string with unit postfix (eg 10kts). “op” contains a field operator.                                                                                                                                                                                                                                                                                                                                           | 
 | ms2525Condition | Used to test directly against Milstd2525 symbol string. Can contain multiple subfields. Example: `<ms2525condition field="symbol.2525code" op="Or">` `<entry field="Identity" value="H"/>` `<entry field="Identity" value="U"/>` `<entry field="Identity" value="P"/>` `</ms2525condition>` Ms2525condition “field” attribute refers to the track field containing 2525-string. “op” is the same as for “compositecondition”. Entry “field” attribute refers to a section of the 2525-string. | 
 | AgeCondition    | Used to test for track age. “field” attribute contains the age field name, “value” contains the age value to compare with in seconds and “op” contains the operator.                                                                                                                                                                                                                                                                                                               | 

**Mil standard 2525 "field" identifiers**

 | Name           | Character positions | 
 | ----           | ------------------- | 
 | CodingScheme   | 1                   | 
 | Identity       | 2                   | 
 | BattleDim      | 3                   | 
 | Status         | 4                   | 
 | FuncId         | 5-10                | 
 | SymMod SizeMod | 11-12               | 
 | CountryCode    | 13-14               | 
 | OrderOfBattle  | 15                  | 


{% include links.html %}
