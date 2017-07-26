---
title: Geo Fencing Client
keywords: GDK
tags: [geofencing, sampleapplication, wpf, custom_layer]
sidebar: clientapi_sidebar
permalink: clientapi_geofencing.html
summary: This section describes how to create a map client utilizing Maria geo fencing functionality.
---

{% include image.html file="clientapi/geofencing/geofencingclient.png" max-width=400 caption="Geo Fencing Client" %}

Maria provides Geo Fence functionality, which allows the user to define geographical shapes (lines and areas) that acts as virtual “fences”.
This sample application will illustrate Geo Fence management through data source specification, rule management and notification handling.

For more information see [Developer Reference, Geofencing](core_geofencing.html)!
 
* The example is based on a map component corresponding to 
[Maria Basic Map Client](maria_gdk/programming/getting_started/mariabasicmapclient), with 
[additional map selection](maria_gdk/programming/getting_started/mariamapinteractionclient/maplayerinteraction#changing_map_source) and  
[tools](maria_gdk/programming/getting_started/mariamapinteractionclient/toolsinteraction).

* You will need to add the following NuGet packages:
  *  ***TPG.GeoFramework.GeoFencingClient***((Currently available from the "TPG Nightly" repository only))
  *  ***TPG.Maria.DrawObjectLayer***
  *  ***TPG.Maria.TrackLayer***
  *  ***TPG.Maria.CustomLayer***

* Sample code for this example is found in the ***MariaGeoFencing*** project, in ***MariaAdditionalComponents*** 
folder of the ***Sample Projects*** solution. 

### Sections

The example consist of the following main steps:

 1.  [Preparations](geofence_preparation.html)
 1.  [Data Source Management](geofence_datasourcemanagement.html).
 1.  [Rule Management](geofence_rulemanagement.html)
 1.  [Notification Management](geofence_notificmanagement.html)
 1.  [Visualization](geofence_visualization.html)
 1.  [Detecting Service Restart](geofence_servicerestart.html) 

{% include links.html %}