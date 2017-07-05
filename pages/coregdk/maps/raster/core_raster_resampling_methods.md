---
title: Raster service resampling and interpolation methods.
keywords: raster, resampling, interpolation
tags: [map, raster]
sidebar: core_sidebar
permalink: core_raster_resampling_methods.html
summary: Supported interpolation and resampling methods for reprojecting and scaling images.
---

# Resampling and interpolation methods

The raster service supports various interpolation methods that are used when reprojecting and scaling images. The choice of resampling method may have huge impact on both the quality of the produced maps and the processing time of each map tile. These effects are linked, so that higher quality tiles also means higher processing time. 

These methods can be specified per dataset in the raster map package XML by using the ''resampleMethod'' parameter, like this:

```xml
	<param name="resampleMethod" value="Bilinear"/>
```

The supported resampling methods are, in order of quality/processing time. The number in parenthesis indicates typical processing time of a 256x256 tile.

 1.  NearestNeighbor (44ms)
 2.  Bilinear (56ms)
 3.  Cubic (74ms)
 4.  CubicSpline (322ms)
 5.  Lanczos (472ms)

We see that Lanczos takes more than 10x as long as NearestNeighbor, so you shouldn't use it unless you think you need it, or if processing time is no concern at all (if you have time to precache a large dataset for example).

In general, if you have a dataset with overviews or pyramids, such as ECW or QM2 files, you will never need to scale anything more than 0.5x - 2x. In this case it is usually enough with Bilinear resampling. However, if you have a dataset with no overviews you may have to scale the image down quite much when zooming out. In this case, you need a higher order resampling method, such as Lanczos. The following figure illustrates downsampling of a map image, first to half its size with nearest neighbor and Bilinear, next downsampling to quarter size with more methods. Here you can clearly see that Bilinear is not enough when you need heavier downsampling.

{% include image.html file="core/raster/interpolationmethods.png" %}
