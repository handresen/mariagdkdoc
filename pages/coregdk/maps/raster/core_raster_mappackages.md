---
title: Raster mappackages
keywords: raster, setup, xml
tags: [navigation]
sidebar: core_sidebar
permalink: core_raster_mappackages.html
summary: Explaining the raster mappackages xml-files. 
---

## Raster map packages XML

This page contains documentation on the parameters and names which make up a raster map packages xml file.

Datasets are evaluated from top to bottom in the xml-file, so if you have multiple datasets that cover the same area and scale/resolution range, the last dataset will cover the first.

The raster map packages file should be named ''*.rastermappackages.xml'' or (versions post 2.0.1) ''*.rastermappackage.xml''. The latter is equivalent to ''*.mappackage.xml'' for vector maps and contains only one mappackage where as the ''*.rastermappackage**s**.xml'' can contain multiple mappackages.


{% include callout.html content="Note that all elements and attributes in the xml-files should be lowercase unless otherwise noted." type="info" %}

### Example rastermappackages.xml

```xml
<mappackages>
  
  <mappackage signature="Oslo">
    <rasterdataref id="Blue Marble"/>
    <rasterdataref id="Oslo Raster"/>
  </mappackage>
  
  <mappackage signature="GMTED2010">
    <params>
      <param name="mapContentType" value="Elevation"/>
    </params>
    <rastermapdataset id="GMTED 2010">
      <dataset path="Raster\GMTED2010\GMTED2010.vrt"/>
    </rastermapdataset>
  </mappackage>
  
  <mappackage signature="N50">
    <rastermapdataset>
      <dataset dir="Raster\N50\utm33"  extension=".qm2"/>
      <params>
	<param name="proj4" value="+proj=utm +zone=33 +datum=WGS84"/>
	<param name="resampleMethod" value="Bilinear"/>
	<param name="minScale" value="5000"/>
	<param name="maxScale" value="50000"/>
      </params>
    </rastermapdataset>
  </mappackage>
  
  <mappackage signature="NormalMapElevations" name="Normal map elevation data">
    <params>
      <param name="resampleMethod" value="Bilinear"/>
      <param name="mapContentType" value="ElevationNormals"/>
    </params>
    <rastermapdataset>
      <dataset path="Raster\dted2\dted2no.vrt"/>
      <params>
        <param name="minResolution" value="250"/>
      </params>
    </rastermapdataset>
    <rastermapdataset>
      <dataset path="Raster\SRTM30\SRTM30.vrt"/>
      <params>
        <param name="maxResolution" value="250"/>
      </params>
    </rastermapdataset>
  </mappackage>
  
</mappackages>
```

### Example rastermappackage.xml (versions newer than 2.0.1)

```xml
<mappackage signature="NE2_HR_87a0eec4-0fe3-48cc-84f6-38bf9d0f755b" name="NE_background">
   
   <rastermapdataset id="NE_background">
      <dataset path="Data\NE2_HR_LC_SR_W.tif.gpkg" recursive="false" />
   </rastermapdataset>
   <params>
     <param name="projection" value="native" />
   </params>
   
</mappackage>
```

## Mappackages

`<mappackages>` is the root of the mappackage**s**-xml and acts as a container for one or more map packages.
In mappackage-xml's, with only one map package, `<mappackage>` is the root element.

 | Child element | Description               | Properties | 
 | ------------- | -----------               | ---------- | 
 | mappackage    | Raster dataset container. | O R C      | 

### Mappackage

A mappackage is a collection of parameters and datasets.

 | Child element                         | Description                          | Properties | 
 | -------------                         | -----------                          | ---------- | 
 | [params](#params)                     | Collection of mappackage parameters. | O C        | 
 | [rastermapdataset](#rastermapdataset) | A collection of raster datasets.     | O R C A    | 
 | [rasterdataref](#rasterdataref)       | A reference to a rastermapdataset    | O R A      | 
 | [revisions](#revisions)               | Obsolete                             |          | 

 | Attribute | Description                                  | Properties | 
 | --------- | -----------                                  | ---------- | 
 | signature | A unique name that identifies a map package. |          | 

#### Params

 | Child element   | Description                                                                       | Properties | 
 | -------------   | -----------                                                                       | ---------- | 
 | [param](#param) | Map package parameter. F.ex. surface type, resample method, map content type etc. | O R A      | 

##### Param

 | Attribute | Description      | Properties | 
 | --------- | -----------      | ---------- | 
 | name      | Parameter name.  |          | 
 | value     | Parameter value. |          | 

Valid parameters are:

 | Attribute            | Description                                                                                                                                                                                                                                                                                                                                                                                                      | Properties | 
 | ---------            | -----------                                                                                                                                                                                                                                                                                                                                                                                                      | ---------- | 
 | proj4                | Projection string in PROJ4 format. Overrides any projection info in the data files. A list of map projections and their corresponding proj4/WKT strings can be found at [spatialreference.org](http://www.spatialreference.org/)                                                                                                                                                                                 |          | 
 | projWKT              | Projection string in WKT format. Overrides any projection info in the data files. A list of map projections and their corresponding proj4/WKT strings can be found at [spatialreference.org](http://www.spatialreference.org/)                                                                                                                                                                                   |          | 
 | minResolution        | Minimum valid resolution in units per pixel for this dataset, for example meters/pixel or degrees/pixel. Requesting an image with lower resolution will give an "OutsideResolution" result unless another dataset covers the same area with lower resolution.                                                                                                                                                    |          | 
 | maxResolution        | Maximum valid resolution in units per pixel for this dataset. Requesting an image with higher resolution will give an "OutsideResolution" result unless another dataset covers the same area with higher resolution.                                                                                                                                                                                             |          | 
 | minScale             | Minimum valid (nominal) scale for this dataset. Requesting an image with lower scale (more detail) will give an ''OutsideResolution'' result unless another dataset covers the same area with lower ''minScale''.                                                                                                                                                                                                |         | 
 | maxScale             | Maximum valid (nominal) scale for this dataset. Requesting an image with higher scale (less detail) will give an ''OutsideResolution'' result unless another dataset covers the same area with higher ''maxScale''.                                                                                                                                                                                              |          | 
 | resampleMethod       | Resampling method. Valid values are: "NearestNeighbour" (default), "Bilinear", "Cubic", "CubicSpline" and "Lanczos".                                                                                                                                                                                                                                                                                             |          | 
 | mapContentType       | Intended usage of the raster data. If not given, a regular raster map is assumed. Other values can be "Elevation" for elevation data or "ElevationNormals" for normal maps used for shading.                                                                                                                                                                                                                     |          | 
 | nodata               | Nodata value. For elevation data, samples equal to this value are ignored when assembling the output image. For paletted images, pixels with this palette index are treated as transparent. This value is not used for RGBA images which instead uses the alpha channel for transparency, or transparentColor.                                                                                                   |          | 
 | transparentColor     | RGBA color to treat as transparent. This is used as "nodata" for RGBA color data sets. The value should be given as a hex color string in RGBA order, i.e #RRGGBBAA, for example "#ffffffff" for full white.                                                                                                                                                                                                     |          | 
 | transparentTolerance | Tolerance for transparent color comparisons. In some cases you may want to mask away a certain color and colors that are close to it. This value indicates how much a color can differ from the transparent color (by component) and still be considered transparent. For example, if the transparent color is "#ffffffff" an tolerance is 1, both #fefefefe" and #fffffffe" will also be considered transparent |          | 
 | subDataId            | For data with subdatasets this indicates the index of the subdataset we want to use. If not given, all subdatasets are used. The ID is an integer in the region 1-N where N is the number of subdatasets.                                                                                                                                                                                                        |          | 
 | subDataList          | For data with subdatasets this indicates a comma-separated list of subdatasets we want to use. If not given, all subdatasets are used. The ID is an integer in the region 1-N where N is the number of subdatasets.                                                                                                                                                                                              |          | 
 | projection           | This parameter can be given on map package level to indicate the preferred projection of this dataset(s). It can be either a WKT or an EPSG code. You can also set this to "native" which will give you the native projection of the datasets (if the data sets have different projections, the first suitable projection is used.)                                                                              |          | 

#### Rastermapdataset

A raster map dataset defines a set of files with common parameters. Typically a map signature can consist of different datasets at 
different scale or resolution ranges. 

 | Child element       | Description         | Properties | 
 | -------------       | -----------         | ---------- | 
 | [dataset](#dataset) | Raster map dataset. | O R A      | 
 | [params](#params)   | Dataset parameters. | O C        | 

 | Attribute | Description                                                                                        | Properties | 
 | --------- | -----------                                                                                        | ---------- | 
 | id        | String identifying this rastermapdataset. Used when referencing a dataset from another mappackage. |          | 

##### Dataset

 | Attribute | Description                                                                         | Properties | 
 | --------- | -----------                                                                         | ---------- | 
 | path      | Path to dataset. Path can be absolute or relative.                                  |          | 
 | dir       | Dataset directory.                                                                  |          | 
 | extension | File extension. Used with dir attribute. Note that this parameter is case sensitive |          | 
 | recursive | Recursive file search. Used with dir attribute. Valid values are true/false.        |          | 

#### Rasterdataref

 | Attribute | Description                            | Properties | 
 | --------- | -----------                            | ---------- | 
 | id        | String identifying a rastermapdataset. | O          | 

#### Revisions

Obsolete.

## General tips and notes

*  For high resolution data sets without overviews or low resolution pyramids, make sure to set up limits on the maxScale or minResolution. If not, the server will try to read the entire data set when you request a map that is zoomed far out.

*  Use Bilinear resampling on imagery for good looking results.

*  Generate overviews if your rasterdata is to be used in lower resolution than the original data.
    
