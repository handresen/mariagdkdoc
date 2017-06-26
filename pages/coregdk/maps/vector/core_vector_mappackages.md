---
title: Vector mapackages setup in Maria GDK
keywords: vector, setup
tags: [navigation]
sidebar: core_sidebar
permalink: core_vector_mappackages.html
summary: Setting up vector mappackages in Maria GDK 
---
 
# Mappackages

The mappackage-xml is used to setup and describe a collection of map datasets and thus enable the creation of a complete, complex map.

Note that scalebase and/or nominalscalefactor specified in the mappackage-xml will override settings for scalebase and/or nominalscalefactor in the multidatasetfiles.

Example:

```xml
<mappackage visibility=”Visible”>
  <mapversion value="1.0.0"/>
  <scalebase type="nominal"/>
  <nominalscalefactor value="1.0"/>
  <vectormapdataset id="Norge 0-25M">
    <multidataset name="OpenStreet" 
                  file="OpenStreetDataset/OpenStreetMap.m6mmultidataset.xml" 
                  minscale="500" 
                  maxscale="15000"/> 
    <multidataset name="Norge N50" 
                  file="Datasets N50/NorgeN50.m6mmultidataset.xml" 
                  maxscale="50000"/> 
    <multidataset name="Norge N250" 
                  file="n250-5mShape\NorgeN250.m6mmultidataset.xml" 
                  maxscale="250000"/> 
    <multidataset name="Norge N500" 
                  file="n250-5mShape\NorgeN500.m6mmultidataset.xml" 
                  maxscale="500000"/> 
    <multidataset name="Norge N1000" 
                  file="n250-5mShape\NorgeN1000.m6mmultidataset.xml" 
                  maxscale="1000000"/> 
    <multidataset name="Norge N2000" 
                  file="n250-5mShape\NorgeN2000.m6mmultidataset.xml" 
                  maxscale="2000000"/> 
    <multidataset name="Norge N5000" 
                  file="n250-5mShape\NorgeN5000.m6mmultidataset.xml" 
                  maxscale="25000000"/>
  </vectormapdataset>
</mappackage>
```

## Mappackage

`<mappackage>` is the root node of the mappackage-xml.

 | **Attribute** | **Description**                                                                                                                                                | **Properties** | 
 | ------------- | ---------------                                                                                                                                                | -------------- | 
 | visibility    | Valid values are “Visible” and “Hidden”. Defines the map visibility in the map catalog service. If attribute not present the default value is Visible. |              | 

 | **Child element**                          | **Description**                                                                                                                          | **Properties** | 
 | -----------------                          | ---------------                                                                                                                          | -------------- | 
 | [scalebase](#scalebase)                   | Map scale base.                                                                                                                          | O A            | 
 | [mapversion](#mapversion)                 | Semantic version number for map package.                                                                                                 | O A            | 
 | [nominalscalefactor](#nominalscalefactor) | Scale factor.                                                                                                                            | O A            | 
 | [vectormapdataset](#vectormapdataset)     | Adds a group of multidatasets. Enables serverside merging of datasets at the same scale level. Can be referenced from other mappackages. | O R C A        | 
 | [group](#group)                           | Group of multidatasets. Can be used for serverside merging of datasets at the same scale level.                                          | O R C A        | 
 | [vectordataref](#vectordataref)           | References a vectormapdataset defined in another mappackage.                                                                             | O R A          | 

#### Scalebase

 | **Attribute** | **Description**                                                                  | **Properties** | 
 | ------------- | ---------------                                                                  | -------------- | 
 | type          | Valid values are “nominal” and “actual”. Default value is “nominal”. | O              | 

#### Mapversion

 | **Attribute** | **Description**                                                                                        | **Properties** | 
 | ------------- | ---------------                                                                                        | -------------- | 
 | value         | String representing semantic version number, MAJOR.MINOR.PATCH[-RELEASESTATE], for instance "2.1.1-RC" | O              | 
 
#### Nominalscalefactor

 | **Attribute** | **Description**                                    | **Properties** | 
 | ------------- | ---------------                                    | -------------- | 
 | value         | Factor used to adjust scale. Default value is 1.0. | O              | 

{% include image.html file="./core/vector/maria2012_vectormaps_html_59748625.png" caption="(Left) nominal scalebase, scalefactor 3.0. (Right) nominal scalebase, scalefactor 0.5." %}


{% include image.html file="./core/vector/maria2012_vectormaps_html_mfa1b6b.jpg" caption="(Left) actual scalebase. (Right) nominal scalebase." %}

#### Vectormapdataset

Vectormapdataset enable serverside merging of datasets. These can be referenced from other mappackages using vectordataref elements.

```xml
<vectormapdataset id="Norge2013 0-25M" maskcoverage="MaskOpaque">
  <multidataset name="Norge N50 2013" 
                file="Norge 2013\N50\NorgeN50.m6mmultidataset.xml" 
                maxscale="40000"/>
  <multidataset name="Norge N250 2013" 
                file="Norge 2013\N250\NorgeN250.m6mmultidataset.xml" 
                maxscale="200000"/> 
  <multidataset name="Norge N500 2013" 
                file="Norge 2013\N500\NorgeN500.m6mmultidataset.xml" 
                maxscale="400000"/>
  <multidataset name="Norge N1000 2013" 
                file="Norge 2013\N1000\NorgeN1000.m6mmultidataset.xml" 
                maxscale="800000"/>
  <multidataset name="Norge N2000 2013" 
                file="Norge 2013\N2000\NorgeN2000.m6mmultidataset.xml" 
                maxscale="2500000"/>
  <multidataset name="Norge N5000 2013" 
                file="Norge 2013\N5000\NorgeN5000.m6mmultidataset.xml" 
                maxscale="25000000"/>
</vectormapdataset>
```

 | **Child element**             | **Description**                    | **Properties** | 
 | -----------------             | ---------------                    | -------------- | 
 | [multidataset](#multidataset) | Adds map multidataset information. | O R A          | 

 | **Attribute** | **Description**                                                                                                                                                                                                                                                            | **Properties** | 
 | ------------- | ---------------                                                                                                                                                                                                                                                            | -------------- | 
 | id            | Key that identifies the vectormapdataset. Used when referencing a dataset from another mappackage.                                                                                                                                                                         | O              | 
 | maskcoverage  | Mask mode. Valid values are “NoMasking” (no pixels included in mask), “MaskSemiTransparent” (all not fully transparent pixels included in mask), “MaskOpaque” (only fully opaque (a=255) pixels included in mask). Default value is “MaskSemiTransparent”. | O              | 

##### Multidataset

 | **Attribute** | **Description**                                                                            | **Properties** | 
 | ------------- | ---------------                                                                            | -------------- | 
 | name          | Key that identifies the multidataset.                                                      | \\             | 
 | file          | Relative path to the map multidataset xml file.                                            | \\             | 
 | minscale      | Minimum valid map scale for this multidataset. Use of scale factors (k/K/m/M) are allowed. | O              | 
 | maxscale      | Maximum valid map scale for this multidataset. Use of scale factors (k/K/m/M) are allowed. | O              | 
 
#### Group

Mappackage groups enable serverside merging of datasets. This functionality is largely replaced by vectormapdataset.

Example:

```xml
<mappackage>
  <scalebase type="nominal"/>
  <nominalscalefactor value="1.0"/>		
  <group maskcoverage="MaskSemiTransparent">		
    <multidataset file="mgcp\FMGTMGCPTestdata.m6mmultidataset.xml" 
                  minscale="5000" maxscale="25000000"/>
  </group>
  <group>
    <multidataset file="vmap0/vmap0.m6mmultidataset.xml" 
                  minscale="10000" maxscale="50000000"/> 
  </group>
</mappackage>
```

 | **Child element**             | **Description**                    | **Properties** | 
 | -----------------             | ---------------                    | -------------- | 
 | [multidataset](#multidataset) | Adds map multidataset information. | O R A          | 

 | **Attribute** | **Description**                                                                                                                                                                                                                                                            | **Properties** | 
 | ------------- | ---------------                                                                                                                                                                                                                                                            | -------------- | 
 | maskcoverage  | Mask mode. Valid values are “NoMasking” (no pixels included in mask), “MaskSemiTransparent” (all not fully transparent pixels included in mask), “MaskOpaque” (only fully opaque (a=255) pixels included in mask). Default value is “MaskSemiTransparent”. | O              | 
 

#### Vectordataref

Using `<vectordataref>` makes it possible for a mappackage to reference datasets located in other mappackages. The dataset(s) to reference must be wrapped inside a `<vectormapdataset>`-block and the `<vectormapdataset>` must have an defined id (f.ex. id="Norge2013 0-25M").

{% include callout.html content="Note that the maskcoverage-attribute on a vectordataref will override the maskcoverage-attribute on the referenced vectormapdataset." type="info" %}
 
Example:

```xml
This mappackage...
<mappackage>  	
  <scalebase type="nominal"/>
  <nominalscalefactor value="1.0"/>
  <vectordataref id="Norge2013 0-25M" maskcoverage="MaskSemiTransparent"/>
</mappackage>

...references dataset in this mappackage:
<mappackage>
  <scalebase type="nominal"/>
  <nominalscalefactor value="1.0"/>
  <vectormapdataset id="Norge2013 0-25M" maskcoverage="MaskOpaque">
    <multidataset name="N50" 
                  file="Norge\N50\N50.m6mmultidataset.xml" maxscale="40000"/>
    <multidataset name="N250" 
                  file="Norge\N250\N250.m6mmultidataset.xml" maxscale="200000"/>
  </vectormapdataset>
</mappackage>
```

