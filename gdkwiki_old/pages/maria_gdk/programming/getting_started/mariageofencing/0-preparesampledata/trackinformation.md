## Track Information

The tracks we are working with in this sample belongs to the **//MariaGeoFence.Sample//** track store (track list), from a track service running on localhost.  

### Track preparation

Create track layer and *TrackViewModel* as described in [Creating Track Layer with Track Service Connection](maria_gdk/programming/getting_started/mariabasicmapclient/creating_track_layer).

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

Make sure that your *App.config* file contains an end point specification for the Track Service. See [Service Configuration.](maria_gdk/programming/getting_started/mariabasicmapclient/service_configuration)

Then, define and instansiate the TrackViewModel in the declarations and constructor of the main view model (//MariaWindowViewModel//).

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

 | Function                                                                                                                                                                                                                                                                | GUI element | Description | 
 | --------                                                                                                                                                                                                                                                                | ----------- | ----------- | 
 | Add | Button | Add track, e.g. at random position within the screen area ((See  [Track Layer->Create tracks](maria_gdk/programming/getting_started/mariamapinteractionclient/tracklayerinteraction#create_tracks), or  MariaGeoFencing sample project for details.)). |
 | Update | Button | Update all tracks to new position((See  [Track Layer->Update tracks](maria_gdk/programming/getting_started/mariamapinteractionclient/tracklayerinteraction&#update_tracks), or MariaGeoFencing sample project for details.)). |                      
 | Remove | Button | Remove all selected tracks((See  [Track Layer->Remove tracks](maria_gdk/programming/getting_started/mariamapinteractionclient/tracklayerinteraction&#remove_tracks), or  MariaGeoFencing sample project for details.)). |                            
 | Track Move | Check box | Activate the Track Move tool((See  [Tools](maria_gdk/programming/getting_started/mariamapinteractionclient/toolsinteraction), or  MariaGeoFencing sample project for details.)). |                                                            

{{:maria_gdk:programming:getting_started:mariageofencing:geofencetrackmngt.png?direct&300|Track Management}}

