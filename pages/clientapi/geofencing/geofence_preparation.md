---
title: Geo Fence, Preparations
keywords: 
tags: 
sidebar: clientapi_sidebar
permalink: geofence_preparation.html
summary: This section describes how to connect to the track and draw object services, and create and modify tracks and objects.
---

To demonstrate the Geo Fencing functionality, we need Tracks in a track service, and Geo Shapes (Draw objects) in a Draw Object service. We also need to move the tracks around, modify the draw objects, and to remove tracks and objects.

The following sections handles connecting to the services, and track/draw object management.

## Track Information

The tracks we are working with in this sample belongs to the ***MariaGeoFence.Sample*** track store (track list), from a track service running on localhost.  

### Track preparation

Create track layer and *TrackViewModel* as described in [Basic Map Client](clientapi_tracklayer.html#createtracklayerandconnect).

Then, in *TrackViewModel*, implement an event handler for the ServiceConnected event in addition to the LayerInitialized event. The constructor and event handlers will be looking something like this:
 

```csharp
public TrackViewModel(IMariaTrackLayer trackLayer)
{
    _trackLayer = trackLayer;
    _trackLayer.LayerInitialized += OnTrackLayerInitialized;
    _trackLayer.ServiceConnected += OnTrackServiceConnected;
}
private void OnTrackServiceConnected(object sender, MariaServiceEventArgs args)
{
    if (_trackLayer.TrackServices.Count > 0)
    {
        var activeName = "MariaGeoFence.Sample";
        _trackLayer.ActiveTrackService = _trackLayer.TrackServices[0];

        _trackLayer.TrackLists = new ObservableCollection`<string>`(_trackLayer.GetTrackLists());

        if (!_trackLayer.TrackLists.Contains(activeName))
            _trackLayer.TrackLists.Add(activeName);

        _trackLayer.ActiveTrackList = activeName;
    }
}
private void OnTrackLayerInitialized()
{
    _trackLayer.TrackServices = new ObservableCollection`<IMariaService>` { new MariaService("TrackService")};
}
```

Make sure that your *App.config* file contains an end point specification for the Track Service. See [Service Configuration.](clientapi_serviceconfiguration.html)

Then, define and instansiate the TrackViewModel in the declarations and constructor of the main view model (*MariaWindowViewModel*).

```csharp
public TrackViewModel TrackViewModel { get; set; }
. . .
public MariaWindowViewModel()
{
    . . .
    TrackViewModel = new TrackViewModel();
    . . .
}
```

### Track Management

Add the following to your application window:

<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="60%" />
</colgroup>

<thead>
<tr class="header">
<th>Function</th>
<th>GUI element</th>
<th>Description</th>
</tr>
</thead>

<tbody>
<tr>
<td markdown="span">Add</td>
<td markdown="span">Button</td>
<td markdown="span">
Add track, e.g. at random position within the screen area.<br>
See [Map Interaction Client](clientapi_tracklayerinteraction.html#createtracks), or MariaGeoFencing sample project.
</td>
</tr>

<tr>
<td markdown="span">Update</td>
<td markdown="span">Button</td>
<td markdown="span">
Update all tracks to new position.<br>
See [Map Interaction Client](clientapi_tracklayerinteraction.html#updatetracks), or  MariaGeoFencing sample project.
</td>
</tr>

<tr>
<td markdown="span">Remove</td>
<td markdown="span">Button</td>
<td markdown="span">
Remove all selected tracks.<br>
See [Map Interaction Client](clientapi_tracklayerinteraction.html#removeobject), or  MariaGeoFencing sample project.
</td>
</tr>

<tr>
<td markdown="span">Track Move</td>
<td markdown="span">Check box</td>
<td markdown="span">
Activate the Track Move tool.<br>
See [Map Interaction Client](clientapi_toolsinteraction.html), or  MariaGeoFencing sample project.
</td>
</tr>
</tbody>
</table>

{% include image.html file="clientapi/geofencing/geofencetrackmngt.png" max-width=400 caption="Track Management" %}

## Geo Shape Information

The draw objects we are working with should belong to the ***MariaGeoFence.Sample*** draw object store, from a local draw object service.

### Draw Object preparation

Create draw layer and *DrawObjectViewModel* as described in [Map Interaction Client](clientapi_drawobjectlayerinteraction.html#createdrawobjectlayer).

Then, in *DrawObjectViewModel*, implement an event handler for the ServiceConnected event in addition to the LayerInitialized event. Add service connection when the layer is initialized, and creation/selection of draw object store when the service is connected. 

The constructor and event handlers will be looking something like this:

```csharp
public DrawObjectViewModel(IMariaDrawObjectLayer drawObjectLayer, MariaWindowViewModel parent)
{
    _parent = parent;

    _drawObjectLayer = drawObjectLayer;
    _drawObjectLayer.LayerInitialized += DrawObjectLayerOnLayerInitialized;
    _drawObjectLayer.ServiceConnected += OnDrawObjectLayerServiceConnected;

    _drawObjectLayer.ExtendedDrawObjectLayer.ActiveCreationWorkflowCompleted += 
      OnExtendedDrawObjectLayerActiveCreationWorkflowCompleted;
}
private void DrawObjectLayerOnLayerInitialized()
{
    _drawObjectLayer.DefaultStyleXml = "`<styleset>``<stylecategory name=\"DrawObjects\"/>``</styleset>`";

    DrawObjectServices = new ObservableCollection`<IMariaService>`
    {
        new MariaService("DrawObjectService")
    };

    _parent.DisplayFilters.Add(_drawObjectLayer.DisplayFilter);

    if (_drawObjectLayer != null &&
        _drawObjectLayer.GenericCreationWorkflows != null &&
        _drawObjectLayer.GenericCreationWorkflows.Count > 0)
    {
        foreach (var gwf in _drawObjectLayer.GenericCreationWorkflows.Reverse())
            _drawObjectLayer.CreationWorkflows.Insert(0, gwf);
    }
}

private void OnDrawObjectLayerServiceConnected(object sender, MariaServiceEventArgs args)
{
    if (DrawObjectServices.Count > 0)
    {
        var activeName = _parent.StoreId;
        ActiveDrawObjectService = DrawObjectServices[0];

        DrawObjectServiceStores =
            new ObservableCollection`<string>`(_drawObjectLayer.GetDrawObjectServiceStores());


        if (!DrawObjectServiceStores.Contains(activeName))
        {
            DrawObjectServiceStores.Add(activeName);
        }

        ActiveDrawObjectServiceStore = activeName;
    }
}
```

Make sure that your *App.config* file contains an end point specification for the Draw Object Service. See [Service Configuration Setup.](clientapi_serviceconfiguration.html)

Then, define and instansiate the DrawObjectViewModel in the declarations and constructor of the main view model (MariaWindowViewModel).

```csharp
public DrawObjectViewModel DrawObjectViewModel { get; set; }
. . .
public MariaWindowViewModel()
{
    . . .
    DrawObjectViewModel = new DrawObjectViewModel();
    . . .
}
```


### Draw Object Management

Add the following to your Application window:

<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="60%" />
</colgroup>

<thead>
<tr class="header">
<th>Function</th>
<th>GUI element</th>
<th>Description</th>
</tr>
</thead>

<tbody>
<tr>
<td markdown="span">Add</td>
<td markdown="span">Button</td>
<td markdown="span">
Add draw object, e.g. at random position within the screen area. See [Map Interaction Client](clientapi_drawobjectlayerinteraction.html#createdrawobjectsprogramatically), or MariaGeoFencing sample project.
</td>
</tr>

<tr>
<td markdown="span">Pt.Edit</td>
<td markdown="span">Button</td>
<td markdown="span">
Point edit mode for selected draw object. See [Map Interaction Client](clientapi_drawobjectlayerinteraction.html#editpoints), or  MariaGeoFencing sample project.
</td>
</tr>

<tr>
<td markdown="span">Remove</td>
<td markdown="span">Button</td>
<td markdown="span">
Remove selected draw objects. See [Map Interaction Client](clientapi_drawobjectlayerinteraction.html#removeobject), or  MariaGeoFencing sample project.
</td>
</tr>
</tbody>
</table>

{% include image.html file="clientapi/geofencing/geofencedrawobjectmngt.png" max-width=400 caption="Draw object management" %}

{% include links.html %}