## Tools

Maria has several, mutual exclusive tools:

 | Tool                                                                                                                   | Action | 
 | ----                                                                                                                   | ------ | 
 | Default tool (no tool) | Click => Select track or draw layer object. |                                                
 | ::: | Click => Select track or draw layer object. \\ * `<wrap>`Selection fan displayed if hit on several tracks.`</wrap>`|
 | ::: |Click and drag selected draw layer object => Move object. |                                                      
 | ::: | Click and drag map => Pan. |                                                                                    
 | Zoom tool | Click and drag => Zoom to marked area. |                                                                  
 | Distance tool | Click => New point of distance polygon. |                                                             
 | ::: | Double click => Conclude distance polygon. |                                                                    
 | ::: | Click and drag concluded distance polygon => Insert polygon point. |                                            
 | Map feature query tool | Click => Select and highlight map element. |                                                 
 | Track selection tool | Click => Select all tracks at position. |                                                      
 | ::: | Click and drag => Select all tracks in area. |                                                                  
 | Track move tool | Click and drag track => Move track. |                                                               

In addition, as sub-state of the default tool, we have the draw object tools.
 | Tool                                                                                                                                                                                                           | Action | 
 | ----                                                                                                                                                                                                           | ------ | 
 | [Creation](maria_gdk/programming/getting_started/mariamapinteractionclient/drawobjectlayerinteraction&#user_created_objects_in_map_draw_object_tools) | Click ? New point of polygon / default sized object. |
 | ::: | Double click => Conclude polygon. |                                                                                                                                                                     
 | ::: | Click and drag => New object. |                                                                                                                                                                         
 | [Edit points](maria_gdk/programming/getting_started/mariamapinteractionclient/drawobjectlayerinteraction&#edit_points_mode) | Edit object control points |                                                    
 | ::: | Drag control points => Modify object shape. |                                                                                                                                                           
 | ::: | Drag additional control points => Insert new control points. |                                                                                                                                          

`<WRAP center round important>`
Note that scrolling the mouse wheel activates zoom for all tool types!
`</WRAP>`

###  Accessing the tools

Create properties in the main interface class (//MariaWindowViewModel//) for the following.

*  Collection of desired tools \\ `<code csharp>`
public ObservableCollection`<IGeoTool>` Tools { private get; set; }
`</code>`

*  Active tool \\ `<code csharp>`
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
`</code>`

*  State of tools (for desired tools) \\ `<code csharp>`
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
`</code>`

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

`<WRAP center round box>`
 You should now be able to change between the different Maria tools!
`</WRAP>`


{{maria_gdk:programming:maria_2012_tutorial_html_mc92aff7.png|Zoom tool}}\\ Figure: Zoom tool.
{{maria_gdk:programming:maria_2012_tutorial_html_328a6ca3.png|Distance tool}}\\ Figure: Distance tool.
{{maria_gdk:programming:maria_2012_tutorial_html_m515417cb.png|Track selection tool}}\\ Figure: Track selection tool.
{{maria_gdk:programming:maria_2012_tutorial_html_m48f48e2d.png|Move track tool}}\\ Figure: Move track tool.
