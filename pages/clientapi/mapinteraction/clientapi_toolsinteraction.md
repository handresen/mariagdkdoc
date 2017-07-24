---
title: Tools
keywords: 
tags: 
sidebar: clientapi_sidebar
permalink: clientapi_toolsinteraction.html
summary: This section describes how to interact with the Maria GDK Tool functionality.
---

## Tools in general 

Maria has several, mutual exclusive tools:

 | Tool                                                                                                                   | Action | 
 | ----                                                                                                                   | ------ | 
 | Default tool<br>(no tool) | Click => Select track or draw layer object. |                                                
 |  | Click => Select track or draw layer object.<br>* Selection fan displayed if hit on several tracks.|
 |  |Click and drag selected draw layer object => Move object. |                                                      
 |  | Click and drag map => Pan. |                                                                                    
 | Zoom tool | Click and drag => Zoom to marked area. |                                                                  
 | Distance tool | Click => New point of distance polygon. |                                                             
 |  | Double click => Conclude distance polygon. |                                                                    
 |  | Click and drag concluded distance polygon => Insert polygon point. |                                            
 | Map feature query tool | Click => Select and highlight map element. |                                                 
 | Track selection tool | Click => Select all tracks at position. |                                                      
 |  | Click and drag => Select all tracks in area. |                                                                  
 | Track move tool | Click and drag track => Move track. |                                                               

In addition, as sub-state of the default tool, we have the draw object tools.

 | Tool   | Action |
 | ----   | ------ |
 | [Creation](clientapi_drawobjectlayerinteraction.html#drawobjecttools) | Click => New point of polygon / default sized object. |
 |  | Double click => Conclude polygon. |
 |  | Click and drag => New object. |
 | [Edit points](clientapi_drawobjectlayerinteraction.html#editpoints) | Edit object control points |
 |  | Drag control points => Modify object shape. |
 |  | Drag additional control points => Insert new control points. |

Note that scrolling the mouse wheel activates zoom for all tool types!

##  Accessing the tools

Create properties in the main view model class (*MariaWindowViewModel*) for the following.

*  Collection of desired tools
*  Active tool
*  State of tools (for desired tools)

``` csharp
public ObservableCollection`<IGeoTool>` Tools { private get; set; }

private IGeoTool _activeTool;
public IGeoTool ActiveTool
{
    get { return _activeTool; }
    set
    {
        _activeTool = value;
        RefreshTools();
        NotifyPropertyChanged(() => ActiveTool);
    }
}

public bool IsZoomToolActive
{
    get
    {
        var tool = GetToolByName("ZoomTool");
        return ActiveTool == tool && tool != null;
    }
    set
    {
        ActiveTool = value ? GetToolByName("ZoomTool") : null;
        NotifyPropertyChanged(() => IsZoomToolActive);
    }
}
public bool IsDistanceToolActive
{
    get
    {
        var tool = GetToolByName("DistanceTool");
        return ActiveTool == tool && tool != null;
    }
    set
    {
        ActiveTool = value ? GetToolByName("DistanceTool") : null;
        NotifyPropertyChanged(() => IsDistanceToolActive);
    }
}
public bool IsMapFeatureQueryToolActive
{
    get
    {
        var tool = GetToolByName("MapFeatureQueryTool");
        return ActiveTool == tool && tool != null;
    }
    set
    {
        ActiveTool = value ? GetToolByName("MapFeatureQueryTool") : null;
        NotifyPropertyChanged(() => IsMapFeatureQueryToolActive);
    }
}
public bool IsTrackSelectionToolActive
{
    get
    {
        var tool = GetToolByName("TrackSelectTool");
        return ActiveTool == tool && tool != null;
    }
    set
    {
        ActiveTool = value ? GetToolByName("TrackSelectTool") : null;
        NotifyPropertyChanged(() => IsTrackSelectionToolActive);
    }
}

public bool IsMoveTrackToolActive
{
    get
    {
        var tool = GetToolByName("TrackMoveTool");
        return ActiveTool == tool && tool != null;
    }
    set
    {
        ActiveTool = value ? GetToolByName("TrackMoveTool") : null;
        NotifyPropertyChanged(() => IsMoveTrackToolActive);
    }
}
private IGeoTool GetToolByName(string name)
{
    return Tools != null ? 
             Tools.FirstOrDefault(tool => tool.ToolName == name) : null;
}
```

Add binding between Maria User control and the Tools property…

```csharp
<MariaUserControl:MariaUserControl 
                    Name="MariaCtrl"
                    Tools ="{Binding Tools, Mode=OneWayToSource }"
                    ActiveTool="{Binding ActiveTool}" 
                    . . .
```

… and, inside the wrap panel of the map window, add buttons for interaction to the desired tools with binding to the corresponding tool state.

```csharp
<ToggleButton Content="Zoom Tool" ToolTip="Zoom Tool" 
              Height="30" Name="ZoomTool" 
              IsChecked="{Binding ZoomToolActive}" />
<ToggleButton Content="Distance Tool" ToolTip="Distance Tool" 
              Height="30" Name="DistanceTool" 
              IsChecked="{Binding IsDistanceToolActive}"/>
<ToggleButton Content="Map Feature Query Tool" ToolTip="Map Feature Query Tool" 
              Height="30" Name="MapFeatureQueryTool" 
              IsChecked="{Binding IsMapFeatureQueryToolActive}"/>
<ToggleButton Content="Track Selection Tool" ToolTip="Track Selection Tool" 
              Height="30" Name="TrackSelectionTool" 
              IsChecked="{Binding IsTrackSelectionToolActive}"/>
<ToggleButton Content="Move Track Tool" ToolTip="Move Track Tool" 
              Height="30" Name="MoveTrackTool" 
              IsChecked="{Binding IsMoveTrackToolActive}"/>
```
 
Then, add the following refresh handling (//MariaWindowViewModel,// requires inheritance from *ViewModelBase*).

```csharp
private void RefreshTools()
{
    NotifyPropertyChanged(() => IsZoomToolActive);
    NotifyPropertyChanged(() => IsDistanceToolActive);
    NotifyPropertyChanged(() => IsMapFeatureQueryToolActive);
    NotifyPropertyChanged(() => IsTrackSelectionToolActive);
    NotifyPropertyChanged(() => IsMoveTrackToolActive);
}
```

 You should now be able to change between the different Maria tools!

{% include image.html file="clientapi/mapinteraction/toolzoom.png" caption="Zoom tool" %}

{% include image.html file="clientapi/mapinteraction/tooldist.png" caption="Distance tool" %}

{% include image.html file="clientapi/mapinteraction/tooltrackselect.png" caption="Track selection tool" %}

{% include image.html file="clientapi/mapinteraction/tooltrackmove.png" caption="Move track tool" %}


{% include links.html %}