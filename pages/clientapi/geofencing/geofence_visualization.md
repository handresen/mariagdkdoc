---
title: Geo Fence, Visualization
keywords: 
tags: 
sidebar: clientapi_sidebar
permalink: geofence_visualization.html
summary: This section describes how to add custom visualization of the Geo Fence information.
---

{% include image.html file="clientapi/geofencing/visualshowselected.png" caption="Geo Fence Visualization" %}

This section gives an example of visualization of Geo Fence information, from *GeoFenceRuleViewModel* and *GeoFenceNotificationsViewModel*, in a custom map layer - with additional GUI components to manage the visualization.

The following are to be visualized :

 * **Notification elements** <br>
   Line between first- and last- track position. Color according to notification level. <br>
   Ledger line to current track possition, for selected tracks or notifications only. 

 * **Moving Ranges**<br>
   Range circle(s) for Moving Range Tracks.

## Custom Classes

Create your custom Layer (*GeoFencingLayer*) and View (*GeoFencingView*) classes with corresponding factories (*GeoFencingLayerFactory* and *GeoFencingViewFactory*), as decribed in [Maria Custom Layer](clientapi_customclient.html).

```c#
public class GeoFencingLayer : GeoLayerViewModel { . . . }
public class GeoFencingLayerFactory : IMariaCustomLayerFactory { . . . }
public class GeoFencingView : Grid, IGeoLayerView { . . . }
public class GeoFencingViewFactory : IGeoLayerViewFactory { . . . }
```

### Data Classes

Add classes for data transfer from the view models to the custom layer, and between the custom layer and custom view:

```c#
public enum VisualInfoType
{
    EventPossitions,
    LedgerLine,
    LedgerEllipsis,
}

public abstract class VisualData
{
    public Pen LinePen { get; set; }
    public Point PtStart { get; set; }
}

public class VisualLine : VisualData
{
    public Point PtEnd { get; set; }
}
public class VisualEllipsis : VisualData
{
    public Brush FillBrush { get; set; }
    public double RadiusX { get; set; }
    public double RadiusY { get; set; }
}

public class VisualMovingRangeInfo
{
    public LatLonPos PtCentre { get; set; }
    public double MovingRange { get; set; }
}

public class CustomNotificationDef
{
    public NotificationDef Def { get; set; }
    public LatLonPos CurrentTrackPos { get; set; }

    public CustomNotificationDef(NotificationDef item) 
    {
        Def = item;
    }
}
```

### View

In the custom **view** class, *GeoFencingView*, add and initialize lists for screen information (*VisualData*) to be visualized and rendering code:

```c#
public List<VisualData> VisualNotifications { get; private set; }
public List<VisualData> VisualMovingRanges { get; set; }

public GeoFencingView()
{
    . . .
    VisualNotifications = new List<VisualData>();
    VisualMovingRanges = new List<VisualData>();
}
. . .
private void Render(DrawingContext dc)
{
    RenderList(dc, VisualMovingRanges);
    RenderList(dc, VisualNotifications);
}
private static void RenderList(DrawingContext dc, List<VisualData> items)
{
    foreach (var element in items)
    {
        if (element.GetType() == typeof (VisualLine))
        {
            var line = (VisualLine) element;
            dc.DrawLine(line.LinePen, line.PtStart, line.PtEnd);
        }
        else if (element.GetType() == typeof (VisualEllipsis))
        {
            var ellipsis = (VisualEllipsis) element;
            dc.DrawEllipse(ellipsis.FillBrush,
                ellipsis.LinePen, ellipsis.PtStart,
                ellipsis.RadiusX, ellipsis.RadiusY);
        }
    }
}
```

### Layer

In the custom **layer** class, *GeoFencingLayer*, add update methods, converting lists with 
geographic info for notifications (*CustomNotificationDef*) and  moving ranges (*VisualMovingRangeInfo*) 
to screen information (*VisualData*). <br>  
Also implement clear functions, and initialize pen and brush to be used for moving ranges.

```c#
private readonly Pen _rangePen;
private readonly Brush _ranegBrush;
. . .
public GeoFencingLayer()
{
    . . .    
    _rangePen = new Pen(new SolidColorBrush(Colors.DodgerBlue), 2) {DashStyle = DashStyles.Dash};
    _ranegBrush = new SolidColorBrush(Colors.Transparent);
}
. . .
public void ClearVisualNotifications()
{
    _view.VisualNotifications.Clear();
    _view.Generate();
}
public void ClearVisualMovingRanges()
{
    _view.VisualMovingRanges.Clear();
    _view.Generate();
}
public void UpdateMovingRange(IEnumerable<VisualMovingRangeInfo> list)
{
    if (list == null)
        return;

    _view.VisualMovingRanges.Clear();
    foreach (var element in list)
    {
        var pixelRange = RangeToPixelLength(element.PtCentre, element.MovingRange);
        var centerPt = LatLonPosToPoint(element.PtCentre);

        _view.VisualMovingRanges.Add(new VisualEllipsis
        {
            PtStart = centerPt,
            FillBrush = _ranegBrush,
            LinePen = _rangePen,
            RadiusX = pixelRange,
            RadiusY = pixelRange
        });
    }
}

public void UpdateVisualisation(IEnumerable<CustomNotificationDef> list, bool assosiateWithTrack)
{
    if (list == null)
        return;
    
    _view.VisualNotifications.Clear();
    foreach (var element in list)
    {
        var start = LatLonPosToPoint(element.Def.FirstTrackPos);
        var end = LatLonPosToPoint(element.Def.LastTrackPos);
        _view.VisualNotifications.Add(
            new VisualLine
            {
                PtStart = start,
                PtEnd = end,
                LinePen = PenFromDefinition(element.Def, VisualInfoType.EventPossitions)
            });

        if (element.CurrentTrackPos != null && assosiateWithTrack)
        {
            var track = LatLonPosToPoint(element.CurrentTrackPos);
            _view.VisualNotifications.AddRange(new[]
            {
                new VisualLine
                {
                    PtStart =  start,
                    PtEnd =  track,
                    LinePen = PenFromDefinition(element.Def, VisualInfoType.LedgerLine)
                },
                new VisualLine
                {
                    PtStart =  end,
                    PtEnd =  track,
                    LinePen = PenFromDefinition(element.Def, VisualInfoType.LedgerLine)
                }
            });
        }
    }
}

private double RangeToPixelLength(LatLonPos posLatLon, double lenMeter)
{
    if (GeoContext != null)
    {
        Point ptS, ptE;
        var posStart = new GeoPos(posLatLon.Lat, posLatLon.Lon);
        var endpoint = Earth.BearingRangeToPos(posStart, 
                                               new BearingRange(0, lenMeter));

        GeoContext.Viewport.LatLonToXY(posStart, out ptS);
        GeoContext.Viewport.LatLonToXY(endpoint, out ptE);

        return ptS.Y - ptE.Y;
    }
    return 0.0;
}
private Point LatLonPosToPoint(LatLonPos posLatLon)
{
    if (GeoContext != null)
    {
        Point pt;
        GeoContext.Viewport.LatLonToXY(new GeoPos(posLatLon.Lat, 
                                                  posLatLon.Lon), 
                                       out pt);
        return pt;
    }

    return new Point();
}

private Pen PenFromDefinition(NotificationDef def, VisualInfoType type)
{
    var color = Colors.DimGray;
    var width = 2;
    var dash = DashStyles.Solid;

    if (def.GetUserDataTagValue(GeoFenceDefs.OwnerName, 
                                GeoFenceDefs.StateKey) != 
        GeoFenceDefs.AcknowledgedValue)
    {
        switch (def.Level)
        {
            case NotificationLevel.Low:
                color = Colors.LimeGreen;
                break;
            case NotificationLevel.Medium:
                color = Colors.Yellow;
                break;
            case NotificationLevel.High:
                color = Colors.Red;
                break;
        }
    }
    if (type == VisualInfoType.EventPossitions)
        width = 4;
    else
        dash = DashStyles.Dash;

    return new Pen(new SolidColorBrush(color), width) { DashStyle = dash };
}
```

### Update the rule- and notification- veiw models 

Add fields and constructor parameters for the track layer, the draw object layer, the custom layer and the geo fencing layer (field only) to *GeoFenceRuleViewModel* and *GeoFenceNotificationsViewModel*, and add event handlers for the following events:                                

* *GeoFenceRuleViewModel*<br>
  **Track Layer:** ExtendedTrackLayer.CurrentTrackDisplayItemsChanged<br>
  **Custom Layer:** LayerInitialized

* *GeoFenceNotificationsViewModel*<br>
  **Track Layer:** ExtendedTrackLayer.TrackSelectionChanged<br> 
  **Track Layer:** ExtendedTrackLayer.CurrentTrackDisplayItemsChanged<br>
  **Draw Object Layer:** ExtendedDrawObjectLayer.LayerSelectionChanged<br>
  **Draw Object Layer:** ExtendedDrawObjectLayer.LayerChanged <br>
  **Custom Layer:** LayerInitialized
   
For the *GeoFenceRuleViewModel*:

```c#
. . .
private readonly IMariaDrawObjectLayer _drawObjectLayer;
private readonly IMariaTrackLayer _trackLayer;
private readonly CustomLayer<GeoFencingLayer> _customLayer;
private GeoFencingLayer _geoFencingLayer;
. . .
public GeoFenceRuleViewModel(
       IMariaTrackLayer trackLayer, 
       IMariaDrawObjectLayer drawObjectLayer, 
       CustomLayer<GeoFencingLayer> customLayer)
{
    _trackLayer = trackLayer;
    _drawObjectLayer = drawObjectLayer;
    _customLayer = customLayer;
    _geoFencingLayer = _customLayer.Instance;

    _trackLayer.ExtendedTrackLayer.TrackSelectionChanged += OnTrackSelectionChanged;
    _trackLayer.ExtendedTrackLayer.CurrentTrackDisplayItemsChanged += CurrentTrackDisplayItemsChanged;

    _customLayer.LayerInitialized += OnGeoFenceLayerInitialized;  
    . . .
}
private void OnGeoFenceLayerInitialized() { _geoFencingLayer = _customLayer.Instance; }
private void CurrentTrackDisplayItemsChanged() { }
private void OnTrackSelectionChanged(object sender, TrackSelectionChangedEventArgs args) { }
. . .
```

And for the *GeoFenceNotificationsViewModel*:

```c#
. . .
private readonly IMariaDrawObjectLayer _drawObjectLayer;
private readonly IMariaTrackLayer _trackLayer;
private readonly CustomLayer<GeoFencingLayer> _customLayer;
private GeoFencingLayer _geoFencingLayer;
. . .
public GeoFenceNotificationsViewModel 
    IMariaTrackLayer trackLayer, 
    IMariaDrawObjectLayer drawObjectLayer, 
    CustomLayer<GeoFencingLayer> customLayer)
{
    _trackLayer = trackLayer;
    _drawObjectLayer = drawObjectLayer;
    _customLayer = customLayer;
    _geoFencingLayer = _customLayer.Instance;

    _drawObjectLayer.ExtendedDrawObjectLayer.LayerSelectionChanged += OnGeoshapeSelectionChanged;
    _drawObjectLayer.ExtendedDrawObjectLayer.LayerChanged += DrawObjectLayerOnLayerChanged;
    _trackLayer.ExtendedTrackLayer.TrackSelectionChanged += OnTrackSelectionChanged;
    _trackLayer.ExtendedTrackLayer.CurrentTrackDisplayItemsChanged += CurrentTrackDisplayItemsChanged;

    _customLayer.LayerInitialized += OnGeoFenceLayerInitialized; 
    . . .
}
private void OnGeoFenceLayerInitialized() { _geoFencingLayer = _customLayer.Instance; }
private void CurrentTrackDisplayItemsChanged() { }
private void OnTrackSelectionChanged(object sender, TrackSelectionChangedEventArgs args) { }
private void DrawObjectLayerOnLayerChanged(object sender, DataStoreChangedEventArgs args) { }
private void OnGeoshapeSelectionChanged(object sender, DrawObjectSelectionChangedEventArgs args) { }
. . .
```
### Additional GUI with properties

Add properties for visualization handling, initialize in the *LayerInitialized* event handler. 

#### GeoFenceRuleViewModel

```c#
private bool _showMovingRanges;
public bool ShowMovingRanges
{
    get { return _showMovingRanges; }
    set
    {
        _showMovingRanges = value;
    }
}
private void OnGeoFenceLayerInitialized()
{
    _geoFencingLayer = _customLayer.Instance;
    ShowMovingRanges = true;
}

```

#### GeoFenceNotificationsViewModel

```c#
private bool _visualizeNotifications;
public bool VisualizeNotifications
{
    get { return _visualizeNotifications; }
    set
    {
        _visualizeNotifications = value;

        NotifyPropertyChanged(() => VisualizeNotifications);
        NotifyPropertyChanged(() => VisualizeSelectedNotification);
        NotifyPropertyChanged(() => VisualizeSelectedTracksOrGeoShapes);
    }
}
private bool _visualizeSelectedNotification;
public bool VisualizeSelectedNotification
{
    get { return _visualizeSelectedNotification; }
    set
    {
        _visualizeSelectedNotification = value;

        if (value)
        {
            _visualizeSelectedTracksOrGeoShapes = false;
        }

        NotifyPropertyChanged(() => VisualizeSelectedNotification);
        NotifyPropertyChanged(() => VisualizeSelectedTracksOrGeoShapes);

        PollNotifications();
    }
}
private bool _visualizeSelectedTracksOrGeoShapes;
public bool VisualizeSelectedTracksOrGeoShapes
{
    get { return _visualizeSelectedTracksOrGeoShapes; }
    set
    {
        _visualizeSelectedTracksOrGeoShapes = value;
        if (value)
        {
            _visualizeSelectedNotification = false;
        }

        NotifyPropertyChanged(() => VisualizeSelectedNotification);
        NotifyPropertyChanged(() => VisualizeSelectedTracksOrGeoShapes);

        PollNotifications();
    }
}
. . .
private void OnGeoFenceLayerInitialized()
{
    _geoFencingLayer = _customLayer.Instance;

    VisualizeNotifications = true;
    VisualizeSelectedTracksOrGeoShapes = false;
    VisualizeSelectedNotification = false;
}
```
 
Extend the condition creator to handle visualization as well! See samplecode for details.

### Main window XAML

Then, add visualization GUI:

```xml
<GroupBox Header="Visualiisation" >
    <StackPanel Orientation="Vertical">
        <CheckBox  Content="Visualize Notifications"
                   IsChecked="{Binding GeoFenceNotificationsViewModel.VisualizeNotifications}" />
        <CheckBox Content="Selected Notification"
                  IsEnabled="{Binding GeoFenceNotificationsViewModel.VisualizeNotifications}"
                  IsChecked="{Binding GeoFenceNotificationsViewModel.VisualizeSelectedNotification}" />
        <CheckBox Content="Selected Tracks/Geo Shapes"
                  IsEnabled="{Binding GeoFenceNotificationsViewModel.VisualizeNotifications}"
                  IsChecked="{Binding GeoFenceNotificationsViewModel.VisualizeSelectedTracksOrGeoShapes}" />
        <CheckBox Content="Visualization Moving Ranges"
                  IsChecked="{Binding GeoFenceRuleViewModel.ShowMovingRanges}" />
    </StackPanel>
</GroupBox>
```

### View Models, final tutch

Finally, update the view models, *GeoFenceRuleViewModel* and *GeoFenceNotificationsViewModel*, 
to utilize the visualization.

Implement update methods for notifications and moving ranges respectively, and call update/polling when required.

#### GeoFenceRuleViewModel

```c#
public bool ShowMovingRanges
{
    . . .
    set
    {
        . . .
        if (value)
            UpdateMovingRange();
        else
            _geoFencingLayer.ClearVisualMovingRanges();
    }
}
. . .
private void CurrentTrackDisplayItemsChanged()
{
    UpdateMovingRange();
}  
. . .
private void UpdateMovingRange()
{
    if (_geoFencingLayer == null)
        return;

    var listOfMrInfo = new List<VisualMovingRangeInfo>();
    if (ShowMovingRanges && _mrdList != null)
    {
        //var rect = _trackLayer.GeoContext.Viewport.GeoRect;
        //rect.InflateRelative(2.0);
        var list = _trackLayer.ActiveTrackList;

        foreach (var mrd in _mrdList)
        {
            var tracks = _trackLayer.GetTrackData(mrd.TrackId);
            if (tracks == null || tracks.Length < 1 || !tracks[0].Pos.HasValue)
                continue;

            listOfMrInfo.Add(new VisualMovingRangeInfo
            {
                PtCentre = new LatLonPos(tracks[0].Pos.Value.Lat, tracks[0].Pos.Value.Lon),
                MovingRange = mrd.RangeMeters
            });           
        }
    }

    _geoFencingLayer.UpdateMovingRange(listOfMrInfo);
}
```

#### GeoFenceNotificationsViewModel

```c#
public int SelectedNotificationItemIndex
{
    . . .
    set
    {
        . . .
        UpdateVisualisation();
    }
}
. . .
public bool VisualizeNotifications
{
    . . .
    set
    {
        . . .
        if (value)
            UpdateVisualisation();
        else
            _geoFencingLayer.ClearVisualNotifications();
    }
}
. . .  
private void CurrentTrackDisplayItemsChanged()
{
    UpdateVisualisation();
}
private void OnTrackSelectionChanged(object sender, TrackSelectionChangedEventArgs args)
{
    if (!VisualizeSelectedNotification)
        PollNotifications();
    else
        UpdateVisualisation();
}
private void DrawObjectLayerOnLayerChanged(object sender, DataStoreChangedEventArgs args)
{
    UpdateVisualisation();
}
private void OnGeoshapeSelectionChanged(object sender, DrawObjectSelectionChangedEventArgs args)
{
    if (!VisualizeSelectedNotification)
        PollNotifications();
    else
        UpdateVisualisation();
}
private void PollNotifications()
{
    . . .
    UpdateVisualisation();
}
private void UpdateVisualisation()
{
    if (_geoFencingLayer == null)
        return;

    if (!VisualizeNotifications || _notificationResult == null || _notificationResult.Notifications == null)
        return;

    if (VisualizeSelectedNotification)
    {
        if (!IsNotificationItemSelected)
            return;

        var customList = CreateCustomNotifications(_notificationResult.Notifications[SelectedNotificationItemIndex]);
        _geoFencingLayer.UpdateVisualisation(customList, true);
    }
    else
    {
        var customList = CreateCustomNotifications(_notificationResult.Notifications.ToArray());
        _geoFencingLayer.UpdateVisualisation(customList, VisualizeSelectedTracksOrGeoShapes);
    }
}
private IEnumerable<CustomNotificationDef> CreateCustomNotifications(params NotificationDef[] list)
{
    var customList = new List<CustomNotificationDef>();

    foreach (var item in list)
    {
        var rec = new CustomNotificationDef(item);

        var tracks = _trackLayer.GetTrackData(item.TrackId.ObjectId);

        if (tracks != null && tracks.Length > 0 && tracks[0].Pos.HasValue)
            rec.CurrentTrackPos = new LatLonPos(tracks[0].Pos.Value.Lat, tracks[0].Pos.Value.Lon);
        customList.Add(rec);
    }
    return customList;
}
```

After moving tracks around to cretae notifications, with shapes, rules and moving ranges defined, visualisation can be be altered as shown below:

*  Supress visualization:
{% include image.html file="clientapi/geofencing/visualizationsupressed.png" caption="Supress visualization" %}

*  Show all:
{% include image.html file="clientapi/geofencing/visualizationshowall.png" caption="Visualization - Show all" %}

*  Show selected notification:
{% include image.html file="clientapi/geofencing/visualizationshownotification.png" caption="Visualization - Show selected" %}

*  Show selected tracks/shapes:
{% include image.html file="clientapi/geofencing/visualshowselected.png" caption="Show selected tracks/shapes" %}

{% include links.html %}

