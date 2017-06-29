---
title: Track clustering styling
keywords: styling
tags: [navigation]
sidebar: core_styling_track_sidebar
permalink: core_styling_track_details_clustering.html
summary: Track clustering styling details. 
---

# Track clustering

Track clustering can be used to group tracks based on four algorithms: 

*  **Grid clustering** which divides the view into a grid and creates a cluster of all tracks within a cell.

*  **Sequential clustering** which starts with one track and clusters all tracks around it which are within a range, then it moves on to the next unclustered track and does the same.

*  **Combined clustering** which combines sequential and grid clustering. If the track count in the current view is less than 10000 sequential clustering is used otherwise grid clustering is used.

*  **Group clustering** which groups tracks based on a parent child relationship between tracks.

The appearance of the clusters is controlled by the following style:

```xml
<stylecategory name="TrackClustering"/>
  <style>  
    <valueitem name="MinCount" value="2"/>
    <valueitem name="CanCluster" value="true"/>
    <valueitem name="ParentFieldID" value="GroupParent"/>
    <valueitem name="IDFieldID" value="id"/>  
    <valueitem name="BackgroundColor" value="0,128,255,100"/>
    <valueitem name="OutlineColor" value="0,0,0,150"/>
    <valueitem name="OutlineThickness" value="2"/>
    <valueitem name="OutlineColorAboveMaxCount" value="0,0,0,150"/>
    <valueitem name="OutlineThicknessAboveMaxCount" value="1"/>
    <valueitem name="BoundsBufferWidth" value="10"/>      
    <valueitem name="ShowSymbolLocation" value="true"/>
    <valueitem name="SymbolLocationRadius" value="1.0"/>
    <valueitem name="SymbolLocationColor" value="0,0,0,200"/>
    <valueitem name="SymbolAsSymbolLocation" value="false"/>
    <valueitem name="FontFamilyName" value="Segoe UI"/>
    <valueitem name="FontColor" value="0,0,0,255"/>
    <valueitem name="FontSize" value="11"/>           
    <valueitem name="ShowClusterCount" value="true"/>
    <valueitem name="ShowSymbol" value="true"/>
    <valueitem name="ShowSymbolSelection" value="false"/>
  </style>
</stylecategory>
```

 | Name                              | Description                                                                                                                                                                                                                                                                                                    | 
 | ----                              | -----------                                                                                                                                                                                                                                                                                                    | 
 | **MinCount**                      | Minimum number of tracks in a cluster befored the tracks are clustered.                                                                                                                                                                                                                                        | 
 | **CanCluster**                    | Sets if the tracks can be clustered. Values are true or false.                                                                                                                                                                                                                                                 | 
 | **ParentFieldID**                 | Name of the field used to specify the parent track for a cluster when Group clustering is used.                                                                                                                                                                                                                | 
 | **IDFieldID**                     | If desired specifies a different ID field name than the default for the track ID when Group clustering is used.                                                                                                                                                                                                | 
 | **BackgroundColor**               | The background color.                                                                                                                                                                                                                                                                                          | 
 | **OutlineColor**                  | The outline color.                                                                                                                                                                                                                                                                                             | 
 | **OutlineThickness**              | Line thickness of outline.                                                                                                                                                                                                                                                                                     | 
 | **OutlineColorAboveMaxCount**     | The outline color for clusters where the cluster's fan cannot be displayed because the number of tracks exceed the maximum number of tracks for the fan. The maximum count can be set on the [Selection fan style](maria_gdk/programming/functionality/styling/track/stylingdetails/selectionfan).             | 
 | **OutlineThicknessAboveMaxCount** | Line thickness of the outline for clusters where the cluster's fan cannot be displayed because the number of tracks exceed the maximum number of tracks for the fan. The maximum count can be set on the [Selection fan style](maria_gdk/programming/functionality/styling/track/stylingdetails/selectionfan). | 
 | **BoundsBufferWidth**             | The minimum number of pixels between the tracks and the boundary line for Bounds clustering.                                                                                                                                                                                                                   | 
 | **ShowSymbolLocation**            | Sets if the track symbol or location (colored circle) for clustered tracks is displayed for Bounds clustering. Values are true or false.                                                                                                                                                                       | 
 | **SymbolLocationRadius**          | The radius in pixels for track symbols and track locations (colored circle) for Bounds clustering.                                                                                                                                                                                                             | 
 | **SymbolLocationColor**           | The color of the track locations (colored circle) for Bounds clustering.                                                                                                                                                                                                                                       | 
 | **SymbolAsSymbolLocation**        | Sets if the track symbol for clusterd tracks is displayed instead of their location (colored circle) for Bounds clustering. Values are true or false.                                                                                                                                                          | 
 | **FontFamilyName**                | Font name for clustering count text.                                                                                                                                                                                                                                                                           | 
 | **FontColor**                     | Font color for clustering count text.                                                                                                                                                                                                                                                                          | 
 | **FontSize**                      | Font size for clustering count text.                                                                                                                                                                                                                                                                           | 
 | **ShowClusterCount**              | Sets if the track count of a cluster is displayed at the center of the cluster. Values are true or false.                                                                                                                                                                                                      | 
 | **ShowSymbol**                    | Sets if a track symbol is displayed at the center of the cluster. Values are true or false.                                                                                                                                                                                                                    | 
 | **ShowSymbolSelection**           | Sets if selected tracks should be displayed at their position even though they are clustered. Values are true or false.                                                                                                                                                                                        | 

## Track clustering and global clustering state

Each clustering algorithm has a global state associated with it (this is set on the track layer in the application code). When a global state for a clustering algorithm is set the styling can be conditionally changed for that algorithm. 

The four global states are:

*  **track.groupclustering** Group clustering state

*  **track.gridclustering** Grid clustering state

*  **track.sequentialclustering** Sequential clustering state

*  **track.combinedclustering** Combined clustering state

As an example when the global grid clustering state is set the following style will change the background color to red for Grid clustering.

```xml
<style>
  <statecondition key="track.gridclustering" scope="Global" state="Active" />
  <valueitem name="BackgroundColor" value="255,0,0,100" />
</style>
```
