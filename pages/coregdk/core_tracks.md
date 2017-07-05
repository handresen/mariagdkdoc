---
title: Tracks
keywords: tracks
tags: [track]
sidebar: core_sidebar
permalink: core_tracks.html
summary: Using tracks in Maria GDK.
---

# Tracks

For a vehicle, the classical interpretation of a track is the line of consecutive positions on the ground representing where the vehicle has moved. In relation to radars and radar displays, a track is created by correlating a set of raw observations (plots). The track is assigned an id, the best known current position and a position history based on the associated observations. Other information is typically also associated with the track, such as estimated course, speed and altitude.

In the context of the TPG GDK, a track has the following properties:

*  Current position (lat/lon)

*  Position history

*  Course and speed

*  Time of last observation

*  Generic, named fields 

When creating or updating a track, any user defined field can be added.

The track system in the TPG GDK focuses on display aspects, history and logging. Track correlation and source data analysis is outside the scope of the system. External trackers can easily provide input to the track system.

Note that while the track system primarily deals with dynamic data, it is also very well suited to handle large amounts of static point data. Server side statistics functions and powerful styling options allows for display of large amounts of data (millions of points).


*  [Track styling](./core_styling_track.html)

*  [Track service]()

*  [Core libraries]()

*  [Logging and log replay]()

*  [Performance tips](./core_tracks_performance_tips.html)

*  [Track service in Azure]()


