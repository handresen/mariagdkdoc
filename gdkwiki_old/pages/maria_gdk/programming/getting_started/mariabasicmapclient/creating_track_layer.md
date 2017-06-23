 ===== Step 4: Creating Track Layer with Track Service Connection =====

`<WRAP center round info>`
For this part you will need **//TPG.Maria.TrackLayer//** NuGet package in addition to the previously added packages.
`</WRAP>`

The track layer is accessed programmatically through the track layer interface ***IMariaTrackLayer*** (//_trackLayer//) 
and the extended track layer interface **//IMariaExtendedTrackLayer//** (//_trackLayer.ExtendedTrackLayer//).

Create an interface class (//TrackViewModel//) for the track layer:

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

Implement the OnInitialized event handler (//OnTrackLayerInitialized//)
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

Then, include the TrackViewModel in the main interface (//MariaWindowViewModel//).

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

`<WRAP round box>`
 In addition to map information and mini map, the application should now include tracks from the specified track list of a Track Service -- and you should be able to select tracks!
`</WRAP>`

{{maria_gdk:programming:maria_2012_tutorial_html_m6ba737ca.png?direct|Map area with tracks}}

