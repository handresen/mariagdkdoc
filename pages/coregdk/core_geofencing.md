---
title: Geofencing
keywords: geofencing, tracks, drawobjects, rules, fences
tags: [geofencing]
sidebar: core_geofencing_sidebar
permalink: core_geofencing.html
summary: Geofencing allows the user to define geographical shapes that acts as virtual fences. 
---

Geofencing allows the user to define geographical shapes that acts as virtual "fences". By defining interaction rules, it is possible to generate events based on how tracks interact with these shapes.

A geofence consists of the following components:

*  A geographical shape. Various object types are supported, for instance lines, polygon areas, sectors and range circles

*  Interaction type, one of *entering*, *leaving* or *crossing*

*  Track filter

*  Report template
 
When a track interacts with a geofence in accordance with a rule, a report is generated. A report template determines the contents of the report. Typically, the report includes:

*  Time of interaction

*  Track details

*  Shape/object details

*  Static information contained in the template, for instance textual warnings and importance of the event

{% include image.html file="./core/geofencing/geofencingserviceoverview.png" %}

The core geofencing component is the geofencing service. It is responsible for fetching tracks and fence objects from respective services, for handling rule setup and execution as well as for providing clients with events.

{% include callout.html content="Rules by default apply to all tracks and shapes/objects in the service. Take care to define filters that only include the desired combination of tracks and shapes" type="info" %}

## Example

This example was generated using recorded AIS data and sample symbology. Notifications are displayed using the geofencing service diagnostic application.

{% include image.html file="./core/geofencing/map.png" caption="Track entering \"Langesund\" area" %}
{% include image.html file="./core/geofencing/notifications.png" caption="Notification details" %}

## Sections

[Geofencing service](./core_geofencing_service.html)<br/>
[Data sources](./core_geofencing_datasources.html)<br/>
[Rules](./core_geofencing_rules.html)<br/>
[Notifications](./core_geofencing_notifications.html)<br/>
[Track to track fences](./core_geofencing_movingrange.html)<br/>
[Web API](./core_geofencing_webapi.html)<br/>


