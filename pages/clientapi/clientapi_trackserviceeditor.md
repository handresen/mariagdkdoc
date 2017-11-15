---
title: Track Editor (Windows Forms)
keywords: GDK
tags: [track, service, sampleapplication, win_forms]
sidebar: clientapi_sidebar
permalink: clientapi_trackserviceeditor.html
summary: This section describes how to create an application interacting with the Maria GDK Track Service.
---

{% include image.html file="clientapi/trackserviceeditor/trackserviceeditor.png" max-width=300 caption="Maria Track Editor" %}

##  Track service information

{% include image.html file="clientapi/trackserviceeditor/trackinfooverview.png" max-width=400 caption="Track info overview" %}

###  Track Lists

The track information is organized in track lists, identified by track list name.

###  History settings

History options are specified separately for each track list.

###  Tracks

A Maira GDK track has the following properties:

* Current position (lat/lon)
* Position history
* Course and speed
* Time of last observation

In adition, generic named fields (custom fields) can be added. For further information, see [Core functionality, Tracks](core_tracks.html)

###  Historic info.

Previous positions with timestamp.

##  Track Editor Application

* For this part you will need to include the ***TPG.Maria.TrackLayer*** package  as a minimum.
* You also need to have a Track Service available.
* Sample code for this section is the ***MariaTrackEditor_Forms*** project, in the ***Sample Projects*** solution.

{% include image.html file="clientapi/trackserviceeditor/trackserviceeditor.png" caption="Track editor, example application" %}

{% include links.html %}