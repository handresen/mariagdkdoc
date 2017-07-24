---
title: Track Layer
keywords: 
tags: 
sidebar: clientapi_sidebar
permalink: clientapi_tracklayer.html
summary: This section describes how to create a track layer with connection to the Maria Track service.
---

## Creating Track Layer with Track Service Connection {#createtracklayerandconnect}

The track layer is accessed programmatically through the track layer interface ***IMariaTrackLayer*** (*_trackLayer*) 
and the extended track layer interface ***IMariaExtendedTrackLayer*** (*_trackLayer.ExtendedTrackLayer*).

*  For this part you will need ***TPG.Maria.TrackLayer*** NuGet package in addition to the previously added packages.

Create a view model class (*TrackViewModel*) for the track layer:

```csharp
public class TrackViewModel 
{    
}
```

Add the following local variable

```csharp
private readonly IMariaTrackLayer _trackLayer;
```

Add the TrackViewModel constructor

```csharp
public TrackViewModel(IMariaTrackLayer trackLayer)
{
    _trackLayer = trackLayer;
    _trackLayer.LayerInitialized += OnTrackLayerInitialized;
}
```

Implement the OnInitialized event handler (*OnTrackLayerInitialized*)
For the TrackList value, use a track list available in your Track Service.

```csharp
private void OnTrackLayerInitialized()
{  
    _trackLayer.TrackLists = new ObservableCollection`<string>` { "ais.test" };
    _trackLayer.TrackServices = new ObservableCollection`<IMariaService>`
          {
              new MariaService("TrackService")
          };

    if (_trackLayer.TrackServices.Count > 0)
    {
        _trackLayer.ActiveTrackService = _trackLayer.TrackServices[0];
        _trackLayer.ActiveTrackList = _trackLayer.TrackLists[0];
    }
}
```
Verify that your *app.config* file contains endpoint definitions for the ***catalog service*** and the ***template service***, as described in the [Service Configuration](clientapi_serviceconfiguration.html) section.

Then, include the TrackViewModel in the main view model (*MariaWindowViewModel*).

Declarations:

```csharp
private readonly IMariaTrackLayer _trackLayer;
public TrackViewModel TrackViewModel { get; set; }
```

and in the constructor

```csharp
_trackLayer = new TPG.Maria.TrackLayer.TrackLayer();
TrackViewModel = new TrackViewModel(_trackLayer);
Layers.Add(_trackLayer);
```

In addition to map information and mini map, the application should now include tracks from the specified track list of a Track Service -- and you should be able to select tracks!

{% include image.html file="clientapi/basicmap/basicmapclient.png" max-width=500 caption="Map area with track information" %}


{% include links.html %}