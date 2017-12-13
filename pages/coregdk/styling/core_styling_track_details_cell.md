---
title: Track cell styling
keywords: tracks, cells, styling
tags: [track]
sidebar: core_styling_track_sidebar
permalink: core_styling_track_details_cell.html
summary: Track cell styling details. 
---

Cell styling can be used for vsualising large amounts of track data. Cell styling is contained within a separate style category, "TrackCell".

```xml
<stylecategory name="TrackCell">
  <style>
    <compositeitem name="CellFill">
      <valueitem name="FillColor" value="64,255,64,128"/>
    </compositeitem>
  </style>
</stylecategory>
```

Using the styling above yields the following view for AIS-tracks for Norway:
{% include image.html file="./core/tracks/trackcellbasic.png" %}

### Conditional cell styling

Each cell represents one or more tracks. The cells are analyzed on the server, tracks belonging to that cell are not transferred individually to the client. It is possible to request that the server count each unique value for requested fields. For instance, if vessel type is contained in the field "ais.VESSELTYPE", requesting statistics for this field will return a count of each unique vessel type for all cells.
To style based on this value, use the following condition syntax:

```xml
<fieldcondition field="<fieldid>:<value>" op="<op>" value="<value>"/>
```
The following style displays cells containing at least one tanker in red.

```xml
<fieldcondition field="ais.VESSELTYPE:Tanker" op="Gt" value="0"/>
<compositeitem name="CellFill">
  <valueitem name="FillColor" value="255,0,0,128"/>
</compositeitem>
```

{% include image.html file="./core/tracks/trackcellredtankers.png" %}

The field identifier "count" is used to check for track count within a cell:

```xml
<fieldcondition field="count" op="<op>" value="`<value>"/>
```

### Separate alpha

For track cells, it is allowed to specify "aplpha" as a separate value. This allows the base color and transparency to be set independently, for instance to display track density using cell transparency:

```xml
<style>
  <compositeitem name="CellFill">
    <valueitem name="FillColor" value="64,255,64,64"/>
  </compositeitem>
</style>
<style>
  <fieldcondition field="count" op="Gt" value="5"/>
  <compositeitem name="CellFill">
    <valueitem name="Alpha" value="192"/>
  </compositeitem>
</style>
```

{% include image.html file="./core/tracks/trackcellseparatealpha.png" %}

### Display value count

For track cells, track count or value occurence count can be displayed. The sample below displays count of tankers. To display raw track count, use `<valueitem name="ShowCount" value="count"/>`.

```xml
<style>
  <compositeitem name="CellFill">
    <valueitem name="FillColor" value="64,255,64,64"/>
  </compositeitem>
</style>
<style>
  <fieldcondition field="ais.VESSELTYPE:Tanker" op="Gt" value="0"/>
  <compositeitem name="CellFill">
    <valueitem name="FillColor" value="255,0,0,64"/>
    <valueitem name="ShowCount" value="ais.VESSELTYPE:Tanker"/>    
  </compositeitem>
</style>
```

{% include image.html file="./core/tracks/trackcellcount.png" %}
 
### Multiple display values

Multiple lines containing values from more than one field or field occurence count can be added using the *ShowFormattedCount* item. 

 | Element                | Description                                                           | 
 | -------                | -----------                                                           | 
 | Any text not inside {} | Displayed directly in cell                                            | 
 | {count}                | Total count of tracks within cell                                     | 
 | {br}                   | Linebreak                                                             | 
 | {`<fieldname:value>`}    | Count of all tracks with value referenced by fieldname equal to value | 

Example:

```xml
<compositeitem name="CellFill">
  <valueitem name="ShowFormattedCount"
    value="Total:{count}{br}Passenger:{ais.VESSELTYPE:Passenger ship}{br}Tanker:{ais.VESSELTYPE:Tanker}"/>
</compositeitem>
```

{% include image.html file="./core/tracks/cellformattedtext.png" %}


### Font properties

Font for cell texts can be set using *FontName*, *FontSize* and *FontColor*:

```xml
<compositeitem name="CellFill">
  <.../>
  <valueitem name="FontName" value="Arial"/>
  <valueitem name="FontSize" value="10"/>
  <valueitem name="FontColor" value="0,64,0,255"/>
</compositeitem>
```

### Specifying cell request parameters

When performing track requests, the cell request structure must be set up in  [ServiceSideCellRequestSettings](http://support.teleplanglobe.com/mariagdkdoc/html/80177036.htm).

In the sections above, the track field "ais.VESSELTYPE" was used to perform styling. The StatisticsField must be set up in order to extract counts for different vessel types:

```csharp
trackLayerViewModel.ServiceSideCellRequestSettings.StatisticsFields="ais.VESSELTYPE";
```

### Server side filtering

The track layer automatically attempts to apply the client display filter on the server in order to display cells according to the filter. Note that some filter types are client-side only (for instance selection state), these will not be applied when filtering cells.
