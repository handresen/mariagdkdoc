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
 | [*IMariaTrackLayer*](http://maria.support2.teleplan.no/MariaGDKDoc/html/B2FB03E9.htm)     | *_trackLayer* |                       
 | [*IMariaExtendedTrackLayer*](http://maria.support2.teleplan.no/MariaGDKDoc/html/AA8CECEC.htm) | *_trackLayer.ExtendedTrackLayer* |


##  Create tracks {#createtracks}

Use the ***TrackData*** class to create new track objects. Apply desired track info, and pass it to the track service with the ***SetTrackData*** method.

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

For your convinience, here here you have methods for adding basic tracks at a random possition (*RandomProvider* class) within the current screen area:

```csharp
public void OnAddTrack()
{
    if (_trackLayer.ActiveTrackList == null)
        return;

    var strId = "Track-" + _trackCnt++ + "_" + DateTime.UtcNow.ToString("yyyy-MM-dd HH:mm:ss");
    var itemId = new ItemId(_trackLayer.ActiveTrackList, strId);
    var speed = RandomProvider.GetRandomDouble(5.0, 50.0);
    var course = RandomProvider.GetRandomDouble(0, 360.0);


    var pos = RandomProvider.GetRandomPosition(_trackLayer.GeoContext.Viewport.GeoRect);
    var trackData = new TrackData(itemId, pos, course, speed) { ObservationTime = DateTime.UtcNow };
    trackData.Fields["name"] = strId;


    switch (_trackCnt % 4)
    {
        case 0:
            trackData.Fields["symbol.2525code"] = "SUS*------*****";
            trackData.Fields["identity"] = "Undef";
            trackData.Fields["type"] = "U";
            break;
        case 1:
            trackData.Fields["symbol.2525code"] = "SFS*------*****";
            trackData.Fields["identity"] = "Friend";
            trackData.Fields["type"] = "F";
            break;
        case 2:
            trackData.Fields["symbol.2525code"] = "SHS*------*****";
            trackData.Fields["identity"] = "Hostile";
            trackData.Fields["type"] = "H";          
            break;
        case 3:
            trackData.Fields["symbol.2525code"] = "SNS*------*****";
            trackData.Fields["identity"] = "Neutral";
            trackData.Fields["type"] = "N";
            break;
    }

    _trackLayer.SetTrackData(trackData);
    _trackCnt++;
}

public static class RandomProvider
{
    private static Random _rand = new Random();

    public static int GetRandomInt(int minimum, int maximum)
    {
        return _rand.Next(minimum, maximum);
    }

    public static double GetRandomDouble(double minimum, double maximum)
    {
        return _rand.NextDouble() * (maximum - minimum) + minimum;
    }

    public static GeoPos GetRandomPosition(GeoRect geoRect)
    {
        return new GeoPos(GetRandomDouble(geoRect.UpperLeft.Lat,
                geoRect.LowerRight.Lat),
            GetRandomDouble(geoRect.UpperLeft.Lon,
                geoRect.LowerRight.Lon));
    }

    public static GeoPoint GetRandomGeoPoint(GeoRect geoRect)
    {
        var pt = GetRandomPosition(geoRect);
        return new GeoPoint() { Latitude = pt.Lat, Longitude = pt.Lon };
    }
}
```

{% include image.html file="clientapi/interactionclient/trackcreated.png" caption="Map area with new track" %}

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

Here you have a code example for updating position, course and speed of the currently added tracks.

```c#
public void OnUpdateTrack()
{
    var dataCollection = _trackLayer.GetTrackData();
    foreach (var track in dataCollection)
    {
        var rect = _trackLayer.GeoContext.Viewport.GeoRect;

        rect.Center = track.Pos ?? RandomProvider.GetRandomPosition(rect);
        rect.InflateRelative(0.3);

        track.Course = RandomProvider.GetRandomDouble(-179.9, 180.0);
        track.Speed = RandomProvider.GetRandomDouble(5.0, 50.0);
        track.Pos = RandomProvider.GetRandomPosition(rect);

        _trackLayer.SetTrackData(track);
    }
}
```

{% include image.html file="clientapi/interactionclient/trackupdate.png" caption="Map area with updated track" %}

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

{% include image.html file="clientapi/interactionclient/trackselect.png" caption="Track selection in map area" %}

## Remove tracks {#removetracks}

The ***DeleteTrackData*** method in the track layer interface is used to remove tracks from the track store.

Example:

```csharp
_trackLayer.DeleteTrackData(instanceId);
```

The sample project contains example code for removing selected tracks.

{% include image.html file="clientapi/interactionclient/trackremove.png" caption="Removing tracks" %}

{% include links.html %}