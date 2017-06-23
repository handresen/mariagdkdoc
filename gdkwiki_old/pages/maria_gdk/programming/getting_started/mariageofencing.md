##  Maria Geo Fencing

{{ :maria_gdk:programming:getting_started:mariageofencing:3-notificationmanagement:visualization.png?direct&200|}}

Maria provides Geo Fence functionality, which allows the user to define geographical shapes (lines and areas) that acts as virtual “fences”.
This sample application will illustrate Geo Fence management through data source specification, rule management and notification handling.

For more information see [Developer Reference, Geofencing](maria_gdk/programming/functionality/geofencing)!
 
`<WRAP center round info>`
The example is based on a map component corresponding to 
[Maria Basic Map Client](maria_gdk/programming/getting_started/mariabasicmapclient), with 
[additional map selection](maria_gdk/programming/getting_started/mariamapinteractionclient/maplayerinteraction#changing_map_source) and  
[tools](maria_gdk/programming/getting_started/mariamapinteractionclient/toolsinteraction).

You will need to add the following NuGet packages:

*  **//TPG.GeoFramework.GeoFencingClient//**((Currently available from the "TPG Nightly" repository only))

*  **//TPG.Maria.DrawObjectLayer//**

*  **//TPG.Maria.TrackLayer//**

*  **//TPG.Maria.CustomLayer//**
`</WRAP>`

`<WRAP center round tip>`
Sample code for this example is found in the **//MariaGeoFencing//** project, in **//MariaAdditionalComponents//** 
folder of the **//Sample Projects//** solution. 
`</WRAP>`


### Sections

The example consist of the following main steps:
 1.  [Prepare Sample Data](maria_gdk/programming/getting_started/mariageofencing/0-PrepareSampleData)
 2.  [Data Source Management](maria_gdk/programming/getting_started/mariageofencing/1-DataSourceManagement).
 3.  [Rule Management](maria_gdk/programming/getting_started/mariageofencing/2-RuleManagement)
 4.  [Notification Management](maria_gdk/programming/getting_started/mariageofencing/3-NotificationManagement)
 5.  [Visualization](maria_gdk/programming/getting_started/mariageofencing/4-Visualization)
 6.  [Service Restart](maria_gdk/programming/getting_started/mariageofencing/5-ServiceRestarted)
