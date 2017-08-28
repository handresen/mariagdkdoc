---
title: Elevation Data Overview
keywords: 
tags: 
sidebar: core_sidebar
permalink: elevationdata.html
summary: This section describes . . . 
---

<div class="alert alert-danger" role="alert"> <i class="fa fa-exclamation-circle  fa-2x"></i>
<b>Under construction</b><br>
This section is under construction!<br>
Description will be added soon.
</div>

## Available elevation data

Example:

* Dataset 1: Source resolution 10m
* Dataset 2:	Source resolution 0.5m
* Dataset 3: Source resolution 50m

{% include image.html file="core\calculations\elevation\e2profile_availabledataset.png"  caption="Elevation datasets" %}

## Oversampling: Profile sample distance is larger than data set resolution

When the sample distance is larger than the dataset resolution, peaks and dips between the samples may be lost when returning the sample point elevation only. Hence, additional information for the legs between the sample points is provided; Max, min and average elevation for the leg.

{% include image.html file="core\calculations\elevation\e2dataset_steplargerthanres_grid.png" max-width=500  caption="Oversampling - grid" %}

| Point | Elevation |
| ----  | ---- | 
| Start | 25 |
| 1 | 22 |
| 2	| 17 |
| 3	| 19 |
| End |	19 |

| Leg | Min | Max | Average |
| ----  | ---- | ----  | ---- | 
| Start – 1 | 21 | 33 | 26
| 1-2 | 13 | 24 | 19 |
| 2-3 | 14 | 21 | 17 |
| 3-End | 19 | 28 | 22 |

{% include image.html file="core\calculations\elevation\e2dataset_steplargerthanres_profile.png" max-width=500  caption="Oversampling - profile" %}

## Undersampling: Profile sample distance is smaller than data set resolution

When the sample distance is smaller than the dataset resolution, with several samples within each cell, the profile will probably appear too flat within the cell and too steep between two cells. Different interpolation methods may be specified for smoothing the result.

{% include image.html file="core\calculations\elevation\e2dataset_steplessthanres_grid.png" max-width=500  caption="Undersampling - grid " %}

{% include image.html file="core\calculations\elevation\e2dataset_steplessthanres_profile.png" max-width=500  caption="Undersampling - profile" %}

## Transition between dataset with different resolution

{% include image.html file="core\calculations\elevation\e2dataset_different_res.png" max-width=500  caption="Dataset transition" %}


{% include links.html %}