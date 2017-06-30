---
title: Geofencing web API
keywords: geofencing, web
tags: [geofencing, interface, service, api]
sidebar: core_geofencing_sidebar
permalink: core_geofencing_webapi.html
summary: Using the geofencing web API.
---

Use [Fiddler](http://www.telerik.com/fiddler) to test http POST/DELETE.

## Notifications

Retrieve geofencing notifications newer than provided generation. Uses HTTP GET.
[http://localhost:9006/geofencingwebservice/json/notifications?generation=-1](http://localhost:9006/geofencingwebservice/json/notifications?generation=-1)

 | Parameter  | Type          | Description                                                                                                                                            | 
 | ---------  | ----          | -----------                                                                                                                                            | 
 | generation | int, required | Latest known generation of the caller. All notifications with generation higher than this number will be returned. Pass -1 to return all notifications | 

Returns [NotificationQueryResult](http://support.teleplanglobe.com/mariagdkdoc/html/556A9CF2.htm). Note that the result contains the current notification generation of the service. By passing this number as generation input paramter on subsequent calls, only new notifications will be returned.

## Rules

Retrieve active geofencing rules. Uses HTTP GET.
[http://localhost:9006/geofencingwebservice/json/rules](http://localhost:9006/geofencingwebservice/json/rules)
Returns array of [GeoFenceRuleDef](http://support.teleplanglobe.com/mariagdkdoc/html/144512D.htm).

## Setrule

Set single geofencing rule. Uses HTTP POST. [http://localhost:9006/geofencingwebservice/json/setrule](http://localhost:9006/geofencingwebservice/json/setrule). Request body contains json/xml representation of single [rule definition](http://support.teleplanglobe.com/mariagdkdoc/html/144512D.htm).
Sample request body:

```javascript
{
  "GeoFenceConditionXml":
  "<fieldcondition op=\"Eq\" field=\"Name\" value=\"Ytre Oslofjord\"\/>",
  "Id":"568c528a-459c-4c6f-87af-6be33486dcad",
  "Interactions":3,
  "NotificationTemplate":
  {
    "Description":null,
    "GeoShapeFields":["Name"],
    "Heading":"[time] [track.name] [interaction][ shape.Name]",
    "NotificationLevel":1,
    "PredefinedValues":[{"Key":"region","Value":"local"}],
    "TrackFields":["name","ais.DESTINATION","type","identity"]},
    "TrackConditionXml":null
  }
}
```
Note that "Interactions" is a bitwise combination of numerical values from [here](http://support.teleplanglobe.com/mariagdkdoc/html/AA6A629E.htm).
Returns guid of rule on success.

## Deleterule

Delete single geofencing rule. Uses HTTP DELETE. [http://localhost:9006/geofencingwebservice/json/deleterule/4bb9385f-e2b3-457b-aef1-5b77950ff9d6](http://localhost:9006/geofencingwebservice/json/deleterule/4bb9385f-e2b3-457b-aef1-5b77950ff9d6).
Returns true on successful delete.
