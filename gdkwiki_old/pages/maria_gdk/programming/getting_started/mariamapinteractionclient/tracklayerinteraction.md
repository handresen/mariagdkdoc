#  Track Layer

How to connect to, and display tracks from, a track service was described in **//MariaBasicMapClient//**:

*  [Creating Track Layer with Track Service Connection](maria_gdk/programming/getting_started/mariabasicmapclient/creating_track_layer).

`<WRAP center round important>`
Please note that creating, updating and removing tracks is performed towards the connected track service, and will have effect for all clients connected to this service, while selection handling and track display are performed locally on your client only.
`</WRAP>`

We will now perform further interactions towards the track layer through the track layer interfaces:
 | Interface                                                           | Accesed through | 
 | ---------                                                           | --------------- | 
 | *IMariaTrackLayer*     | *_trackLayer* |                       
 | *IMariaExtendedTrackLayer* | *_trackLayer.ExtendedTrackLayer* |

For details on how to administer the connection to the track service and maintenance of track lists and track history, please see [Utilizing track information](maria_gdk/programming&#utilizing_track_information).

###  Create tracks

Use the ***TrackData*** class to create a new track object, apply desired track info, and pass it to the track service with the ***SetTrackData*** method.

Example:

```csharp
string list = . . . // e.g. “MyList” or  _trackLayer.ActiveTrackList;
string strId = . . . // Unique Id;
ItemId id = new ItemId(list, strId);
double speed = . . .
double course = . . .
GeoPos pos = . . . 

TrackData trackData =  new TrackData(itemId, pos, course, speed) 
{ ObservationTime = DateTime.UtcNow };
trackData.SetTrackDataField("name", strId);

_trackLayer.SetTrackData(trackData);
```

The sample project contains example code for adding new tracks to the center of the screen.

{{maria_gdk:programming:maria_2012_tutorial_html_m37e3485c.png|Map area with new track}}\\ Figure: Map area with new track.

###  Update tracks

Track information can be obtained with the track layer interface ***GetTrackData*** method, and updated by the ***SetTrackData*** method.

Example:

```csharp
var dataCollection = _trackLayer.GetTrackData(arrayOfTrackIds);
foreach (var trackData in dataCollection)
{
    trackData.Pos = newPosition;
    trackData.Speed = newSpeed;
    trackData.Course = newCourse;
}

_trackLayer.SetTrackData(dataCollection);
```

The sample project contains example code for updating position, course and speed of the currently added tracks.

{{maria_gdk:programming:maria_2012_tutorial_html_m1dc792f7.png?800|Map area with updated track}}\\ Figure: Map area with updated track.

###  Track selection

The Maria Control supports selection of tracks by user action in the map area, single select or area select. For additional info, see section “”.

In addition, setting and getting selection state can be performed programmatically through the extended track layer interface.

Example:

```csharp
var selection = _trackLayer.ExtendedTrackLayer.Selected;

_trackLayer.ExtendedTrackLayer.Deselect(itemId);
_trackLayer.ExtendedTrackLayer.Select(itemId);

_trackLayer.ExtendedTrackLayer.Select(newSelection);
_trackLayer.ExtendedTrackLayer.Selected.Clear();
```

The sample project contains example code for selecting and de-selecting tracks.

{{maria_gdk:programming:maria_2012_tutorial_html_m25b4649f.png|Track selection in map area}}\\ Figure: Track selection in map area.

 ====  Remove tracks ====

The ***DeleteTrackData*** method in the track layer interface is used to remove tracks from the track store.

Example:

```csharp
_trackLayer.DeleteTrackData(instanceId);
```

The sample project contains example code for removing selected tracks.

{{maria_gdk:programming:maria_2012_tutorial_html_m59e94a3d.png|Removing tracks}}\\ Figure: Removing tracks.
