---
title: Elevation Profile Client
keywords: GDK
tags: [elevationprofile, sampleapplication, wpf]
sidebar: clientapi_sidebar
permalink: clientapi_elevationprofile.html
summary: This section describes how to create a map client utilizing Maria elevation profile functionality.
---

{% include image.html file="clientapi/elevationprofile/elevationprofileclient.png" max-width=500 caption="Elevation Profile Client" %}

Maria provides display of an elevation profile window for a given set of positions, by utilizing 
***MariaElevationProfileControl***, available in the TPG.Maria.MapLayer namespace. 
The control may be displayed in connection with the map area, as we will do in this example, 
or in a separate window.

This example describes how to include elevation profiles in your map application.

* The example is based on a map component corresponding to [Maria Basic Map Client](clientapi_basicmapclient.html), with 
[additional map selection](clientapi_maplayerinteraction.html#mapsourcechange) and [tools](clientapi_toolsinteraction.html).

* Sample code for this example is found in the ***MariaElevationProfile*** project, in ***MariaAdditionalComponents*** 
folder of the ***Sample Projects*** solution.

###  Including the elevation profile control

Create a view model class (*ElevationProfileViewModel.cs*) for the elevation profile component, inheriting ViewModelBase, with the map layer as parameters to the constructor. Implement public property access to the map layer, and add an event handler for the map layer *LayerInitialized*event, populating the profile with some test positions.

```csharp
public class ElevationProfileViewModel : ViewModelBase
{
    public IMariaMapLayer MapLayer { get; private set; }
    . . .
    public ElevationProfileViewModel(IMariaMapLayer mapLayer)
    {
        MapLayer = mapLayer;

        MapLayer.LayerInitialized += OnMapLayerInitialized;
    }

    private void OnMapLayerInitialized()
    {
            MapLayer.EleveationProfilePositions = new List`<GeoPos>`
                                                      {
                                                          new GeoPos(59, 10), 
                                                          new GeoPos(60, 11)
                                                      };
      }
}
```

Add the profile component to the main window xaml, with bindings to the elevation profile view model:

```xml
<MapLayer:MariaElevationProfileControl 
                    Grid.Row="2" 
                    Name="ctrlElevationProfile"
                    MapLayer="{Binding ElevationProfileViewModel.MapLayer, Mode=OneWay}"
           />
```

Instantiate the view model in the main window view model constructor (MariaWindowViewModel.cs)

```csharp
public MariaWindowViewModel()
{
    Layers = new ObservableCollection`<IMariaLayer>`();
    _mapLayer = new TPG.Maria.MapLayer.MapLayer();

    MapViewModel = new MapViewModel(_mapLayer);
    ElevationProfileViewModel = new ElevationProfileViewModel (_mapLayer);

    Layers.Add(_mapLayer);
}
```

The map window will look something like this:


{% include image.html file="clientapi/elevationprofile/elevationprofileclient.png" caption="First elevation profile" %}

###  Changing the profile visibility

We will add a check box to our GUI to control display of the profile window. Add the following properties for bindings between the check box and the profile component:

```csharp
private bool _isElevationProfileActive = true;
public bool IsElevationProfileActive
{
    get { return _isElevationProfileActive; }
    set
    {
        _isElevationProfileActive = value;
        NotifyPropertyChanged(() => ElevationProfileVisibility);
    }
}

public Visibility ElevationProfileVisibility
{
    get { return IsElevationProfileActive ? 
                       Visibility.Visible : 
                       Visibility.Collapsed; }
}
```

And to the xaml:

```xml
<CheckBox Name="ElevationProfile"  
          Content="Elevation Profile" ToolTip="Elevation Profile" 
          IsChecked="{Binding ElevationProfileViewModel.IsElevationProfileActive}"
          />
. . .
<MapLayer:MariaElevationProfileControl 
            Grid.Row="2"
            Name="ctrlElevationProfile"
            MapLayer="{Binding ElevationProfileViewModel.MapLayer, Mode=OneWay}"  
            Visibility="{Binding ElevationProfileViewModel.ElevationProfileVisibility, Mode=OneWay}"
          />
```

You should now be able to toggle display of the profile window!

{% include image.html file="clientapi/elevationprofile/profilevisibility.png" caption="Toggle  profile window display" %}

###  Input from distance tool

We will now populate the profile component with positions from the Distance Tool.

Add a private field to access elements of the main view for access to tools etc., initialize it from an additional constructor parameter. Also add a field to hold the distance tool.

```csharp
private readonly MariaWindowViewModel _mainModel;
private DistanceTool _distanceTool;
. . .
public ElevationProfileViewModel(MariaWindowViewModel mainModel, 
                                                IMariaMapLayer mapLayer)
{
    _mainModel = mainModel;
    MapLayer = mapLayer;
    MapLayer.LayerInitialized += OnMapLayerInitialized;
}
```

We need an event handler for the tools *DistanceUpdate* event. As this event handler not should be added until the map layer and tools are initialized, we create it in the map layer initialized event handler. We can now remove the test positions.

```csharp
private void OnMapLayerInitialized()
{
    _distanceTool = (DistanceTool)_mainModel.GetToolByName("DistanceTool");
    if (_distanceTool != null)
    {
        _distanceTool.DistanceUpdateHandler += HandleDistanceUpdate;
    }
}
. . .
void HandleDistanceUpdate(object sender)
{
    UpdateElevationProfile();
}
. . .
private void UpdateElevationProfile()
{
    if (IsElevationProfileActive)
    {
        if (_mainModel.IsDistanceToolActive)
        {
            if (_distanceTool != null && _distanceTool.Positions.Count >= 2)
                MapLayer.EleveationProfilePositions = _distanceTool.Positions;
            else
                MapLayer.EleveationProfilePositions = new List`<GeoPos>`();
        }
        else
            MapLayer.EleveationProfilePositions = new List`<GeoPos>`();
    }
}
```

You should now see the profile of the line marked by the distance tool!

{% include image.html file="clientapi/elevationprofile/profiledisttool.png" caption="Profile from distance tool" %}

### Geo Units Settings

If you want to display the distance or elevation in other than default units, you can define a ***GeoUnitsSettings*** property in your view model:

```csharp
public IGeoUnitsSetting GeoUnitsSettings { get; set; }
. . .
public ElevationProfileViewModel(MariaWindowViewModel mainModel, IMariaMapLayer mapLayer)
{
        GeoUnitsSettings = new GeoUnitsSetting
                           {
                               DistanceUnit = DistanceUnit.Imperial,
                               ElevationUnit = ElevationUnit.Feet
                           };
```

and bind the profile component to it:

```xml
<MapLayer:MariaElevationProfileControl 
     MapLayer="{Binding ElevationProfileViewModel.MapLayer, Mode=OneWay}"
     GeoUnitsSettings="{Binding ElevationProfileViewModel.GeoUnitsSettings, Mode=OneWay}"
     Visibility="{Binding ElevationProfileViewModel.ElevationProfileVisibility, Mode=OneWay}"
     />
```

You should now see the profile with alternative units!

{% include image.html file="clientapi/elevationprofile/profileunits.png" caption="Alternative units" %}

###  Highlight Distance

Moving the mouse pointer over the profile area, the corresponding point on the profile is highlighted. The distance of distance to the highlighted point is available from the profile component. Here we will use this value to highlight the corresponding position of the distance tool line.

Add a property for the distance value:

```csharp
public double ElevationProfileHighlightDistance
{
    set
    {
        if (_mainModel.IsDistanceToolActive && _distanceTool != null)
        {
            _distanceTool.HighlightDistance = value;
        }
    }
}
```

and bind the profile component to it:

```xml
<MapLayer:MariaElevationProfileControl 
         MapLayer="{Binding ElevationProfileViewModel.MapLayer, Mode=OneWay}"
         GeoUnitsSettings="{Binding ElevationProfileViewModel.GeoUnitsSettings, Mode=OneWay}"
         Visibility="{Binding ElevationProfileViewModel.ElevationProfileVisibility, Mode=OneWay}"
         HighlightDistance="{Binding ElevationProfileViewModel.ElevationProfileHighlightDistance, Mode=OneWayToSource}"
         />
```

You should now see the distance highlight on the distance tool line!

{% include image.html file="clientapi/elevationprofile/profilehighltdist.png" caption="Highlighted Distance" %}

{% include links.html %}