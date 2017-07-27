---
title: Track Interaction
keywords: 
tags: 
sidebar: clientapi_sidebar
permalink: clientapi_tracklayerinteraction.html
summary: This section describes how to interact with Maria track layer functionality.
---

## Track Layer preparations

How to connect to, and display tracks from, a track service was described in ***MariaBasicMapClient***, [Creating Track Layer with Track Service Connection](clientapi_tracklayer.html).

Please note that creating, updating and removing tracks is performed towards the connected track service, and will have effect for all clients connected to this service, while selection handling and track display are performed locally on your client only.

We will now perform further interactions towards the track layer through the track layer interfaces:

 | Interface | Accesed through | 
 | --------- | --------------- | 
 | *IMariaTrackLayer*     | *_trackLayer* |                       
 | *IMariaExtendedTrackLayer* | *_trackLayer.ExtendedTrackLayer* |


##  Create tracks {#createtracks}

Use the ***TrackData*** class to create a new track object, apply desired track info, and pass it to the track service with the ***SetTrackData*** method.

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

For your convinience, here is a method adding a new track at the center of the screen:

```csharp
private int _trackCnt = 0;
private void AddTrack()
{
    var list = _trackLayer.ActiveTrackList;
    var strId = "Track-" + _trackCnt;
    var itemId = new ItemId(list, strId);
    const double speed = 0.0;
    const double course = 0.0;
    var pos = _trackLayer.GeoContext.CenterPosition;

    var trackData = new TrackData(itemId, pos, course, speed) {ObservationTime = DateTime.UtcNow};

    trackData.Fields["name"] = strId;

    switch (_trackCnt%4)
    {
        case 0:
            trackData.Fields["symbol.2525code"] = "SUS*------*****";
            break;
        case 1:
            trackData.Fields["symbol.2525code"] = "SFS*------*****";
            break;
        case 2:
            trackData.Fields["symbol.2525code"] = "SHS*------*****";
            break;
        case 3:
            trackData.Fields["symbol.2525code"] = "SNS*------*****";
            break;
    }

    _trackLayer.SetTrackData(trackData);
    _myIds.Add(itemId.InstanceId, itemId);
    _trackCnt++;
}
```

{% include image.html file="clientapi/mapinteraction/trackcreated.png" caption="Map area with new track" %}

##  Update tracks {#updatetracks}

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

{% include image.html file="clientapi/mapinteraction/trackupdate.png" caption="Map area with updated track" %}

##  Track selection

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

{% include image.html file="clientapi/mapinteraction/trackselect.png" caption="Track selection in map area" %}

## Remove tracks {#removetracks}

The ***DeleteTrackData*** method in the track layer interface is used to remove tracks from the track store.

Example:

```csharp
_trackLayer.DeleteTrackData(instanceId);
```

The sample project contains example code for removing selected tracks.

{% include image.html file="clientapi/mapinteraction/trackremove.png" caption="Removing tracks" %}

{% include links.html %}