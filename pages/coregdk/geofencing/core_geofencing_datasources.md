---
title: Geofencing datasources
keywords: geofencing, datasource, setup, tracks, drawobjects, services, interfaces
tags: [geofencing, track, drawobject, interface]
sidebar: core_geofencing_sidebar
permalink: core_geofencing_datasources.html
summary: Setting up datasources for geofencing.
---

Data sources are set up using the [IDataManagerServiceBase](http://support.teleplanglobe.com/MariaGDKDoc/html/32457EC9.htm) interface. 

Each data source entry causes the geo fencing service to fetch data from that source. One data source refers to a tracklist within a specified service or to a draw object list within a single draw object service.

Note that tracks and objects are fetched at preset intervals and are not synchronized to track updates or draw object updates. By using versioning/generations, only changed tracks or draw objects are fetched.

The geofencing service is responsible for handling cases where the data sources are temporarily unavailable, for instance if a source service is restarted.

{% include image.html file="./core/geofencing/geofencingdatasourceoverview.png" %}

