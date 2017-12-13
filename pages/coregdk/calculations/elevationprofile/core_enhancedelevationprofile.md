---
title: Core - Enhanced Elevation Profile
keywords: 
tags: [elevationprofile, service]
sidebar: core_sidebar
permalink: core_enhancedelevationprofile.html
summary: This section describes Maria GDK enhanced elevation profile functionality. 
---

{% include image.html file="core\calculations\elevation\e2profile_example.png" max-width=500 caption="Enhanced Elevation " %}

# Enhanced Elevation Service

{% include image.html file="core\calculations\elevation\e2profile_servicearchitecture.png" max-width=400 caption="Enhanced Elevation Service Architecture" %}

More details are found in [Elevation Data Overview](elevationdata.html)

## Interfaces

* Enhanced Elevation, Interface
  ***IEnhancedElevationService***

* Enhanced Elevation, WEB Interface
  ***IEnhancedElevationWebInterface*** 

# Enhanced Elevation Profile in client applications

Maria GDK provides an Enhanced Elevation Profile component, with helper classes for conneting and communicationg with the EnhancedElevation Service, and profile properties handling. 

The profile is calulated from a list of possitions, e.g. from the Maria GDK Distance tool.

{% include image.html file="core\calculations\elevation\e2profile_clientarchitecture.png" max-width=400 caption="Enhanced Elevation Client Architecture" %}

* ***EnhancedElevationProfileControl*** (TPG.EnhancedElevation.Profile)
* ***EnhancedElevationProfileViewModel*** (TPG.EnhancedElevation.Profile)
* ***EnhancedElevationServiceEngine*** (TPG.EnhancedElevationServiceClient)

{% include image.html file="core\calculations\elevation\e2profile_example_params.png" caption="Profile component example" %}

{% include links.html %}