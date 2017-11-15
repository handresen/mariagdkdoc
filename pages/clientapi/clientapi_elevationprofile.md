---
title: Elevation Profile Client
keywords: GDK
tags: [elevationprofile, sampleapplication, wpf]
sidebar: clientapi_sidebar
permalink: clientapi_elevationprofile.html
summary: This section describes how to create a map client utilizing Maria elevation profile functionality.
---

{% include image.html file="clientapi/elevationprofile/elevationprofileclient.png" max-width=500 caption="Elevation Profile Client" %}

Maria provides display of elevation profile windows for a given set of positions, by utilizing 
***MariaElevationProfileControl*** (available in the *TPG.Maria.MapLayer* namespace) or ***EnhancedElevationProfileControl*** (available in the *TPG.EnhancedElevation.Profile* namespace). 
The controls may be displayed in connection with the map area, as we will do in this example, or in separate windows.

This example describes how to include elevation profiles in your map application. Profile possitions are provided by use of the distance tool or from draw object (programatically created polyline within the displayed map area). 

* The example is based on a map component corresponding to [Maria Basic Map Client](clientapi_basicmapclient.html).
* You will need to include the following NuGet packages:
  * ***TPG.Maria.MapLayer***
  * ***TPG.Maria.DrawObjectLayer***
  * ***TPG.EnhancedElevation.Profile***, for ***EnhancedElevationProfileControl*** only.
* Sample code for this example is found in the ***MariaElevationProfile*** project, in the *MariaAdditionalComponents* folder of the ***Sample Projects*** solution.

## General preparation, common to both profile components

Create a view model class (*ProfilesViewModel*) inheriting ViewModelBase, with the  *MariaWindowViewModel* and ***IMariaDrawObjectLayer*** as constructor parameters.

```c#
public class ProfilesViewModel : ViewModelBase
{
    private MariaWindowViewModel _mainModel;
    private readonly IMariaDrawObjectLayer _drawLayer;

   public ProfilesViewModel(MariaWindowViewModel mainModel,
                            IMariaDrawObjectLayer mariaDrawObjectLayer)
   {
       _mainModel = mainModel;
       _drawLayer = mariaDrawObjectLayer;
   }
}    
```

Instansiate the view model in the main view model.

```c#
. . .
public ProfilesViewModel ProfilesViewModel { get; private set; }
private readonly IMariaDrawObjectLayer _drawobjLayer;

public MariaWindowViewModel()
{
    . . .

    _drawobjLayer = new DrawObjectLayer(new DefaultDrawObjectTypeDefinitionProvider())
        {InitializeCreationWorkflows = true};

    ProfilesViewModel = new ProfilesViewModel(this, _drawobjLayer, _mapLayer);
    Layers.Add(_drawobjLayer);
}
```

Add the following controls to your application window:

| Group | Function | GUI element | Description | Binding | 
|:---|:---|:---|:---|:---|
| Map | Map selection | ComboBox | Selecting map template. <br>Described in [Changing map source](clientapi_maplayerinteraction.html#mapsourcechange) | MapViewModel: ***AvailableMapTemplateNames*** <br> MapViewModel: ***ActiveMapTemplateName*** |
| Navigation | Ruler  | CheckBox | Controls display of the *Ruler* component. | Direct binding to ***IsRulerVisible*** in the *MariaCtrl* element.  |
| Navigation | Pan Navigation  | CheckBox | Controls display of the *Pan Navigation* component. | Direct binding to ***IsPanNavigationVisible*** in the *MariaCtrl* element. |
| Navigation | Scale Bar  | CheckBox | Controls display of the *Scale Bar* component. | Direct binding to ***IsScaleBarVisible*** in the *MariaCtrl* element.  |
| Navigation | Mini Map  | CheckBox | Controls display of the *Mini Map* component. | Direct binding to ***IsMiniMapVisible*** in the *MariaCtrl* element. |
| Tools | Distance Tool | CheckBox | Activates Maria GDK distance tool. <br>Described in [tools](clientapi_toolsinteraction.html) | MariaWindowViewModel: ***IsDistanceToolActive*** <br> ProfilesViewModel: ***ChkDistToolCmd*** |
| Tools | Zoom tool |CheckBox | Activates Maria GDK distance tool. <br>Described in [tools](clientapi_toolsinteraction.html) | MariaWindowViewModel: ***IsZoomToolActive*** |
| Tools | Zoom Out | Button| Activates Maria GDK distance tool. <br>Described in  [tools](clientapi_toolsinteraction.html) |MapViewModel: ***ZoomOutCommand*** |
| Draw Objects | Add | Button | Add draw object at random position within the screen area. See [Create draw objects programmatically](clientapi_drawobjectlayerinteraction.html#createdrawobjectsprogramatically) in Map Interaction Client. |ProfilesViewModel: ***AddObjectCmnd*** |
| Draw Objects | Remove | Button | Remove selected draw objects. See [Remove draw objects](clientapi_drawobjectlayerinteraction.html#removeobject) in Map Interaction Client. |ProfilesViewModel: ***RemoveSelectedObjectsCmnd*** |
| Elevation Profile | Local Profile | Check box | Controls the display of the local profile component. <br> Omit if you are *not implementing* ***MariaElevationProfileControl***. |ProfilesViewModel: ***IsElevationProfileActive*** |
| Elevation Profile | Enhanced Profile | Check box | Controls the display of the enhanced profile component. <br> Omit if you are *not implementing* ***EnhancedElevationProfileControl***. | ProfilesViewModel: ***IsEnhancedProfileActive*** |
| Elevation Profile | Enhanced Properties | Check box | Controls the display of the enhanced profile properties. <br> Omit if you are *not implementing* ***EnhancedElevationProfileControl***. |   ProfilesViewModel: ***IsEnhancedPropertiesActive*** <br>  ***IsEnhancedProfileActive*** |

Add properties and event handlers to handle the components to *ProfilesViewModel*.  For details see the sample code view model and xaml.

Your application will now look something like this, and you should be able to use the distancetool and add and remove draw objects.
{% include image.html file="clientapi/elevationprofile/elevationprofile_prepgui.png" max-width=500 caption="Elevation Profile Preparations" %}


To prepare for interaction between the components and the draw layer and tools, add the following event handlers to *ProfilesViewModel*:

```c#
    public ProfilesViewModel(MariaWindowViewModel mainModel,
                             IMariaDrawObjectLayer mariaDrawObjectLayer)
    {
        . . .
        _mainModel.ToolsLoaded += OnToolsLoaded;
        _drawLayer.LayerInitialized += OnDrawLayerInitialized;
    }

    private void OnToolsLoaded(object sender)
    {
        _distanceTool = (DistanceTool)_mainModel.GetToolByName("DistanceTool");
        if (_distanceTool != null)
        {
            _distanceTool.DistanceUpdateHandler += HandleDistanceUpdate;
        }
    }

    private void OnDrawLayerInitialized()
    {
        _drawLayer.ExtendedDrawObjectLayer.LayerSelectionChanged += ExtendedDrawObjectLayerOnLayerSelectionChanged;
        _drawLayer.ExtendedDrawObjectLayer.LayerChanged += ExtendedDrawObjectLayerOnLayerChanged;
    }

    private void HandleDistanceUpdate(object sender)
    {
            UpdateProfiles();
    }  

    private void ExtendedDrawObjectLayerOnLayerChanged(object sender, DataStoreChangedEventArgs args)
    {
        UpdateProfiles();
    }

    private void ExtendedDrawObjectLayerOnLayerSelectionChanged(object sender, DrawObjectSelectionChangedEventArgs args)
    {
        UpdateProfiles();
    }

    private void UpdateProfiles()
    {
        // Handle update of the different profiles here....
    }

```

## Including the elevation profile controls

### Local Elevation Profile, MariaElevationProfileControl

As this profile component is managed through Maria GDK Map Layer, we need to extend the ProfilesViewModel constructor to include the map layer.


```c#
    public ProfilesViewModel(
        MariaWindowViewModel mainModel,
        IMariaDrawObjectLayer mariaDrawObjectLayer, 
        IMariaMapLayer mapLayer)
    {
        . . .
        MapLayer = mapLayer;
    }
```

Add the profile component to the main window xaml, with bindings to the elevation profile view model:

```xml
    <mapLayer:MariaElevationProfileControl Grid.Row="2"
                                           MapLayer="{Binding ProfilesViewModel.MapLayer, Mode=OneWay}"
                                           Visibility="{Binding ProfilesViewModel.ElevationProfileVisibility, Mode=OneWay}" />
```

Running your application, the profile component should be displayed and removed according to the state of the visibility checkbox (***ElevationProfileVisibility***).

To populate the profile component with the desired profile from the distance tool or selected draw object, add the following code:

```c#
    private void UpdateProfiles()
    {
        UpdateLocalProfile();
    }

    private void UpdateLocalProfile()
    {
        if (IsElevationProfileActive)
        {
            if (_mainModel.IsDistanceToolActive)
            {
                if (_distanceTool != null && _distanceTool.Positions.Count >= 2)
                {
                    MapLayer.EleveationProfilePositions = _distanceTool.Positions;
                    return;
                }
            }

            var selected = _drawLayer.ExtendedDrawObjectLayer.SelectedDrawObjectIds.ToArray();
            if (selected.Length == 1)
            {
                var obj = _drawLayer.GetDrawObjectFromStore(selected.First());
                if (obj != null && obj.Points.Length >= 2)
                {
                    var poslist = new List<GeoPos>();
                    foreach (var pt in obj.Points)
                    {
                        poslist.Add(new GeoPos(pt.Latitude, pt.Longitude));
                    }
                    MapLayer.EleveationProfilePositions = poslist;
                    return;
                }
            }
        }

        MapLayer.EleveationProfilePositions = new List<GeoPos>();
    }
```
The result should be like this:

{% include image.html file="clientapi/elevationprofile/elevationprofile_local.png" caption="Local profile from draw objects and distance tool." %}

### Enhanced Elevation Profile, EnhancedElevationProfileControl.

For this control you need to be connected to the EnhancedElevationService. Make sure that your *app.config* file contains endpoint definitions for the ***EnahncedElevationService***, as described in the [Service Configuration](clientapi_serviceconfiguration.html) section.

Connetion and communication towards the service is managed by the ***EnhancedElevationServiceEngine*** class, and profile properties managed through the ***EnhancedElevationProfileViewModel*** class,  bouth available in the *TPG.EnhancedElevation.Profile* namespace. 

Add properties and perform initialization when the Maria layers are initialized, e.g. in the ***OnDrawLayerInitialized*** event handler.

```c#
public EnhancedElevationServiceEngine EnhancedElevationServiceEngine { get; set; }
public EnhancedElevationProfileViewModel EnhancedProfileViewModel { get; set; }

. . .

private void OnDrawLayerInitialized()
{
    . . .

    EnhancedElevationServiceEngine = new EnhancedElevationServiceEngine();
    EnhancedElevationServiceEngine.ConnectToElevationService("EnahncedElevationService");
    EnhancedProfileViewModel = new EnhancedElevationProfileViewModel(new GeoUnitsSetting());
    EnhancedProfileViewModel.ProfileParametersChanged += OnEnhancedProfileParametersChanged;
}

private void OnEnhancedProfileParametersChanged(object sender)
{
    UpdateProfiles();
}

```

Add a user control for the profile properties, see sample project for details. <br>
Add the properties and profile components with bindings to the main window xaml:

```xml
<components:EnhancedPropertiesCtrl Grid.Row="1" Grid.Column="1"
                                   Visibility="{Binding ProfilesViewModel.EnhancedPropertiesVisibility, Mode=OneWay}"/>

<profile:EnhancedElevationProfileControl 
        Grid.Row="2" Grid.Column="0" Grid.ColumnSpan="2"
        Name="EnhancedElevationProfileControl" Height="200" 
        Visibility="{Binding ProfilesViewModel.EnhancedProfileVisibility, Mode=OneWay}"
        ShowMinimumAndMaximumElevation="True"
        EnhancedElevationServiceEngine="{Binding ProfilesViewModel.EnhancedElevationServiceEngine}"
        ProfilePossitionInfo="{Binding ProfilesViewModel.EnhancedProfileViewModel.ProfilePossitionInfo, 
                                       Mode=OneWay}" 
        ServiceAddress="{Binding ProfilesViewModel.EnhancedProfileViewModel.ServiceAddress, 
                                 Mode=OneWayToSource}"
        ServiceConnected="{Binding ProfilesViewModel.EnhancedProfileViewModel.ServiceConnected, 
                                   Mode=OneWayToSource}"
        PlotColors="{Binding ProfilesViewModel.EnhancedProfileViewModel.PlotColors}"
        IncludeAverage="{Binding ProfilesViewModel.EnhancedProfileViewModel.IncludeAverage}"
        IncludeMinMax="{Binding ProfilesViewModel.EnhancedProfileViewModel.IncludeMinMax}"
        InterpolationMethod="{Binding ProfilesViewModel.EnhancedProfileViewModel.InterpolationMethod}"
        PixelsPerSample="{Binding ProfilesViewModel.EnhancedProfileViewModel.PixelsPerSample}"
        ResType="{Binding ProfilesViewModel.EnhancedProfileViewModel.ResolutionType}"
        Resolution="{Binding ProfilesViewModel.EnhancedProfileViewModel.Resolution}" />
```

Then, handle population of the component from the distance tool or selected draw object:

```c#
private void UpdateProfiles()
{
    . . .
    UpdateEnhancedProfile();
}

private void UpdateEnhancedProfile()
{
    if (IsEnhancedProfileActive && EnhancedElevationProfileViewModel != null)
    {
        if (_mainModel.IsDistanceToolActive)
        {
            if (_distanceTool != null && _distanceTool.Positions.Count >= 2)
            {
                EnhancedElevationProfileViewModel.ProfilePossitionInfo = new ProfilePossitionInfo
                {
                    Positions = _distanceTool.Positions,
                    TotalDistance = _distanceTool.TotalDistance
                };
                return;
            }
        }

        var selected = _drawLayer.ExtendedDrawObjectLayer.SelectedDrawObjectIds.ToArray();
        if (selected.Length == 1)
        {
            var obj = _drawLayer.GetDrawObjectFromStore(selected.First());
            if (obj != null && obj.Points.Length >= 2)
            {
                var poslist = new List<GeoPos>();
                var dist = 0.0;
                GeoPos? prevPos = null;
                foreach (var pt in obj.Points)
                {
                    var pos = new GeoPos(pt.Latitude, pt.Longitude);
                    if (prevPos.HasValue)
                    {
                        dist += Earth.MetersBetween(prevPos.Value, pos);
                    }

                    poslist.Add(pos);
                    prevPos = pos;
                }

                EnhancedElevationProfileViewModel.ProfilePossitionInfo = new ProfilePossitionInfo
                {
                    Positions = poslist,
                    TotalDistance = dist
                };
                return;
            }
        }

        EnhancedElevationProfileViewModel.ProfilePossitionInfo = new ProfilePossitionInfo();
    }
}

``` 

The result should be like this:

{% include image.html file="clientapi/elevationprofile/elevationprofile_enhanced.png" caption="Enhanced profile from draw objects and distance tool." %}

## Alternative units, Geo Units Settings

If you want to display the distance or elevation in other than default units, you can define a ***GeoUnitsSettings*** property in your view model. 

```c#
public IGeoUnitsSetting GeoUnitsSettings { get; set; }
. . .
public ElevationProfileViewModel(MariaWindowViewModel mainModel, IMariaMapLayer mapLayer)
{
    GeoUnitsSettings = new GeoUnitsSetting
    {
        DistanceUnit = DistanceUnit.Miles,
        ElevationUnit = ElevationUnit.Feet,
    };

```

and bind the profile component to it:

```xml
<MapLayer:MariaElevationProfileControl 
     . . .
     GeoUnitsSettings="{Binding ElevationProfileViewModel.GeoUnitsSettings, Mode=OneWay}"  />

. . .
<profile:EnhancedElevationProfileControl 
     . . . 
     GeoUnitsSettings="{Binding GeoUnitsSettings, Mode=OneWay}" />
```

You should now see the profile with alternative units!

You can of course add GUI to change the settings while running as well.

{% include image.html file="clientapi/elevationprofile/profileunits.png" caption="Alternative units" %}

## Possition highlighting between profiles and distance tool

###  Highlighting profile possitions on the distance tool

Moving the mouse pointer horisontally in the profile areas, the corresponding point on the profile is highlighted. This point can be highlihted on the distance tool as well.

In the profile view model, add a property for the distance value:

```c#
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

and bind the profile components ***HighlightDistance*** property to it:

```xml
<MapLayer:MariaElevationProfileControl 
     . . .
     HighlightDistance="{Binding ElevationProfileViewModel.ElevationProfileHighlightDistance,
                                 Mode=OneWayToSource}" />
. . .
<profile:EnhancedElevationProfileControl 
     . . . 
     HighlightDistance="{Binding ElevationProfileViewModel.ElevationProfileHighlightDistance,
                                 Mode=OneWayToSource}" />

```

You should now see the distance highlighted on the distance tool line!

{% include image.html file="clientapi/elevationprofile/profilehighltdist.png" caption="Highlighted Distance from profile" %}

###  Highlighting distance tool possition on the profile

Implemented for the ***EnhancedElevationProfileControl*** only.
When hovering the mouse over the distance tool, a mark is shown on the tool line. This point can be marked on the profile.

In the profile view model, add event handler and property for the distance value:

```c#
private double _distanceToolHighlightDistance;
. . .

public double DistanceToolHighlightDistance
{
    get { return _distanceToolHighlightDistance; }
    set
    {
        _distanceToolHighlightDistance = value;
        NotifyPropertyChanged(() => DistanceToolHighlightDistance);
    }
}

private void OnToolsLoaded(object sender)
{
    _distanceTool = (DistanceTool)_mainModel.GetToolByName("DistanceTool");
    if (_distanceTool != null)
    {
        _distanceTool.DistanceUpdateHandler += HandleDistanceUpdate;
        _distanceTool.ToolHighlightDistanceUpdate += OnDistanceToolHighlightDistanceUpdate;
    }
}

private void OnDistanceToolHighlightDistanceUpdate(object sender, double distance)
{
    DistanceToolHighlightDistance = distance;
}

```

and bind the profile components ***DistanceToolHighlightDistance*** property to it:

```xml
. . .
<profile:EnhancedElevationProfileControl 
     . . . 
     ToolHighlightDistance="{Binding ProfilesViewModel.DistanceToolHighlightDistance, Mode=OneWay}" />

```

You should now see the possition from the tool  marked in the profile!

{% include image.html file="clientapi/elevationprofile/profilehighltdist_fromtool.png" caption="Highlighted Distance from tool" %}

{% include links.html %}
