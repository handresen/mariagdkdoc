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

| Function | GUI element | Description |
|:---|:---|:---|
| Add | Button | Add track, e.g. at random position within the screen area. <br> See [Create tracks](clientapi_tracklayerinteraction.html#createtracks) in Map Interaction Client. |
| Update | Button | Update all tracks to new position. <br> See [Update tracks](clientapi_tracklayerinteraction.html#updatetracks) in Map Interaction Client. |
| Remove | Button | Remove all selected tracks. <br> See [Remove tracks](clientapi_tracklayerinteraction.html#removetracks) in Map Interaction Client. |
| Track Move | Check box | Activate the Track Move tool. <br> See [Tools](clientapi_toolsinteraction.html) in Map Interaction Client. |

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

| Function | GUI element | Description |
|:---|:---|:---|
| Add | Button | Add draw object, e.g. at random position within the screen area. See [Create draw objects programmatically](clientapi_drawobjectlayerinteraction.html#createdrawobjectsprogramatically) in Map Interaction Client. |
| Pt.Edit | Button | Point edit mode for selected draw object. See [Edit points mode](clientapi_drawobjectlayerinteraction.html#editpoints) in Map Interaction Client. |
| Remove | Button | Remove selected draw objects. See [Remove draw objects](clientapi_drawobjectlayerinteraction.html#removeobject) in Map Interaction Client. |

{% include image.html file="clientapi/geofencing/geofencedrawobjectmngt.png" max-width=400 caption="Draw object management" %}

{% include links.html %}