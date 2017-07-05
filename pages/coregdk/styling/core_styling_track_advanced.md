---
title: Track styling advanced topics
keywords: tracks, styling
tags: [track]
sidebar: core_styling_sidebar
permalink: core_styling_track_advanced.html
summary: Advanced track styling topics. 
---

#### Custom symbolizer parameters

Styling values assigned to tracks are assumed to remain constant. This allows very fast style caching and reuse of style elements between individual tracks. Two exceptions to this rule exists:

*  The symbol field "SymbolKeyField" references a track property that may be different between two tracks with the same styling

*  Within composite "CustomSymbolizerParams", the value part of a style item may reference the contents of a track field

Example:

```xml
      <compositeitem name="CustomSymbolizerParams">
         <valueitem name="SomeColor" value="255,0,0,128"/>       
        <!-- {<fieldname>} is used to reference value in track data.
        It is only expanded to track data within compositeitem 
	CustomSymbolizerParams due to styleitem caching considerations -->
         <valueitem name="MyParamName" value="{myfield}"/>
      </compositeitem>    
```

Resolved property values within CustomSymbolizerParams are passed to custom symbol providers implementing [IRasterSymbolProvider](http://support.teleplanglobe.com/mariagdkdoc/html/BEA80435.htm) as *customParameters* to [GetSymbol](http://support.teleplanglobe.com/mariagdkdoc/html/B96657E.htm)


#### Display priorities

Track sybmols are assigned an initial, fixed display priority between 0.0 and 1.0. If the priority is adjusted for a group of tracks using the same value, their internal ordering will be unchanged. Using the style item "RelDispPri", the display priority can be adjusted.

```xml
<stylecategory name="TrackSymbol">
  <!-- ... -->
  <style>
    <statecondition key="Selected" scope="PerItem" state="Active"/>
    <valueitem name="RelDispPri" value="10.0"/>
  </style>
  <!-- ... -->
  <style>
    <fieldcondition field="cargoclass" value="hazardous"/>
    <valueitem name="RelDispPri" value="1.0" action="add"/>
  </style>
  <!-- ... -->
</stylecategory>
```

Using the above style setup, it is possible to combine display priorities in a very fine-grained manner. Consider track A and B, A has hazardous cargo, B does not. Basic display pri assigned by the system is A:0.2, B:0.6. If both are unselected, resulting disp pri is A:1.2, B:0.6, A on top. If B is selected, A:1.2, B:10.6, B on top. If both selected, A:11.2, B:10.6, A on top.

Note *action="add"* in the last valueitem. This causes any occurence of RelDispPri to be added to the existing value rather than just replacing it.






