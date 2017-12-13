---
title: Vector map multidataset setup in Maria GDK
keywords: vector, setup
tags: [map, vector]
sidebar: core_sidebar
permalink: core_vector_multidataset.html
summary: Setting up vector map multidatasets in Maria GDK 
---

The multidataset-xml is used to setup and describe similar type datasets. A dataset is typically produced from the same source data/format.

{% include callout.html content="Note that all entries in this file are treated as hints and may be overridden by the map renderer. Also, all \"limits\" refer to selected scalebase (nominal or actual) multiplied by nominalscalefactor. For \"nominal\", these do not usually match actual map scale." type="info" %}


{% include callout.html content="Note that all limits can be given as zoomlevels since version 2.0. Use postfix \"L\" to parse limits as zoomlevel instead of mapscale (f.ex. minvisible=\"15L\" maxvisible=\"6L\"). Mapscales will be converted to corresponding zoomlevels.
Thinningtab and scalebase is obsolete since version 2.0." type="info" %}

Example:

```xml
<multidataset>
  <globalsettings>
    <multidatasetlimits minvisible="1K" maxvisible="1000000K" 
                        simplestyle="10000K" label="10M"/>
    <resourcereferences styleroot="m6msymbols" 
                        symbols="n5000\symbols.xml" 
                        normal="n5000\ScriptCollection.xml" 
                        simple="n5000\ScriptCollection.xml" 
                        featuregrouping="n5000\featuregrouping.xml"/>
    <groupsetting drawpri="300">
      <limits minvisible="1K" maxvisible="100000K" 
              simplestyle="10000K" label="10M"/>
    </groupsetting>
  </globalsettings>

  <groupsettingtemplates>
    <groupsettingtemplate grouppattern="Arealdekke" geotype="area">
      <groupsetting  drawpri="700" thinning="1.0">
        <limits maxvisible="10M"/>
      </groupsetting>
    </groupsettingtemplate>	

    <groupsettingtemplate grouppattern="Arealdekke" geotype="line">
      <groupsetting  drawpri="400" thinning="1.0">
        <limits maxvisible="10M"/>
      </groupsetting>
    </groupsettingtemplate>	

    <groupsettingtemplate grouppattern="Roads\Motorvei" geotype="line">
      <groupsetting  drawpri="290" thinning="1.0">
        <limits maxvisible="10M"/>
      </groupsetting>
    </groupsettingtemplate>

    <groupsettingtemplate grouppattern="Buildings" geotype="line">
      <groupsetting  drawpri="150" thinning="1.0">
        <limits maxvisible="10M"/>
      </groupsetting>
    </groupsettingtemplate>	

    <groupsettingtemplate grouppattern="Navn" geotype="point">
      <groupsetting drawpri="50" thinning="1.0">
        <limits maxvisible="10M" />
      </groupsetting>
    </groupsettingtemplate>
  </groupsettingtemplates>  

  <add filepattern="N5000.m6mindex"/>
</multidataset>
```

## Multidataset

`<multidataset>` is the root node of the multidataset-xml.

**Note:** thinningtab is obsolete since version 2.0.

 | **Child element**                                | **Description**                   | **Properties** | 
 | -----------------                                | ---------------                   | -------------- | 
 | [globalsettings](#globalsettings)               | Global settings for multidataset. | C              | 
 | [thinningtab](#thinningtab)                     | Feature thinning table.           | O C            | 
 | [groupsettingtemplates](#groupsettingtemplates) | Templates for group settings.     | O R C          | 
 | [add](#add)                                     | Filters used to add data files.   | O R A          | 

### Globalsettings

**Note:** scalebase is obsolete since version 2.0.

 | **Child element**  | **Description**                                                                                  | **Properties** | 
 | -----------------  | ---------------                                                                                  | -------------- | 
 | multidatasetlimits | Collection of scale limits that apply globally to this multidataset.                             | O A            | 
 | scalebase          | Global map scale base.                                                                           | O A            | 
 | nominalscalefactor | Global scale factor.                                                                             | O A            | 
 | cache              | Global map cache settings.                                                                       | O C            | 
 | groupsetting       | Default groupsettings.                                                                           | O C A          | 
 | resourcereferences | Collection of style information hints to be used by the map renderer.                            | A              | 
 | datasetclipborder  | Dataset clip border setting. Used to prevent clipping of large point symbols at dataset borders. | O A            | 

##### Multidatasetlimits

Scale/level limits that apply gobally to this multidataset. See [#limits](#limits) for further information.
Also used for setting overzoom parameter.

Overzoom is used for drawing maps on levels where no data exists. The number of levels specified by the overzoom parameter controls the number of levels the mapservice can search upwards in the maplevel tree for data to draw. The first level encountered to contain data will be used.

**Note:** overzoom is implemented post version 2.0.1.

 | **Attribute** | **Description**                       | **Properties** | 
 | ------------- | ---------------                       | -------------- | 
 | overzoom      | Number of levels. Default value is 0. | \\             | 

Example:

```xml
<multidatasetlimits minvisible="0" maxvisible="256M" simplestyle="256M" 
                    label="10M" overzoom="4"/>
```

#### Groupsetting

{% include callout.html content="Groupsettings under globalsettings act as a default template for the groupsettings listed in the groupsettingtemplate section of the xml." type="info" %}

 | **Child element**          | **Description**                                                                             | **Properties** | 
 | -----------------          | ---------------                                                                             | -------------- | 
 | [limits](#limits)         | Collection of scale limits that apply globally to this multidataset.                        | O A            | 
 | [clipborder](#clipborder) | Layer clip border setting. Used to prevent clipping of large point symbols at tile borders. | O A            | 

 | **Attribute** | **Description**                                                                 | **Properties** | 
 | ------------- | ---------------                                                                 | -------------- | 
 | drawpri       | Layer draw priority. Lower number equals higher priority. Default value is 100. |              | 
 | thinning      | Default value is -1.0. (No thinning)                                            |              | 

##### Limits

All “limits” refer to selected “scalebase” (“nominal” or “actual”) multiplied by “nominalscalefactor”. For “nominal”, these do not usually match actual map scale.

 | **Attribute** | **Description**                                                                | **Properties** | 
 | ------------- | ---------------                                                                | -------------- | 
 | minvisible    | Minimum scale for rendering layer. Use of scale factors (k/K/m/M) are allowed. |              | 
 | maxvisible    | Maximum scale for rendering layer. Use of scale factors (k/K/m/M) are allowed. |              | 
 | simplestyle   | Scale threshold (minimum scale) for showing simple style.                      |              | 
 | label         | Scale threshold (maximum scale) for showing label.                             |              | 

##### Clipborder

{% include callout.html content="Clipborder is only used when drawing points." type="info" %} 

 | **Attribute** | **Description**              | **Properties** | 
 | ------------- | ---------------              | -------------- | 
 | value         | Clip border value in pixels. |              | 

### Thinningtab

{% include callout.html content="Thinningtab is obsolete since version 2.0." type="info" %} 

{% include callout.html content="Thinning tabs may produce ugly artifacts and have no use for regular maps. Use with caution." type="info" %} 

```xml
  <thinningtab active="true">
    <entry scale="1" accuracy="1.0"/>
    <entry scale="10k" accuracy="4.0"/>
    <entry scale="25k" accuracy="8.0"/>
    <entry scale="50k" accuracy="15.0"/>
    <entry scale="100k" accuracy="30.0"/>
    <entry scale="250k" accuracy="150.0"/>
    <entry scale="500k" accuracy="300.0"/>
    <entry scale="1M" accuracy="600.0"/>
    <entry scale="5M" accuracy="3000.0"/>
  </thinningtab>
```

 | **Child element** | **Description**       | **Properties** | 
 | ----------------- | ---------------       | -------------- | 
 | [entry](#entry)  | Thinning table entry. | A R            | 

 | **Attribute** | **Description**                                                                | **Properties** | 
 | ------------- | ---------------                                                                | -------------- | 
 | active        | True if thinning table values should be used. Default value is false/inactive. |              | 

#### Entry

 | **Attribute** | **Description**          | **Properties** | 
 | ------------- | ---------------          | -------------- | 
 | scale         | Map scale threshold.     |              | 
 | accuracy      | Thinning accuracy value. |              | 

#### Groupsettingtemplates

 | **Child element**                              | **Description**                        | **Properties** | 
 | -----------------                              | ---------------                        | -------------- | 
 | [groupsettingtemplate](#groupsettingtemplate) | Feature group settings template entry. | R C A          | 

#### Groupsettingtemplate

 | **Child element**              | **Description**        | **Properties** | 
 | -----------------              | ---------------        | -------------- | 
 | [groupsetting](#groupsetting) | Featuregroup settings. | C A            | 

 | **Attribute** | **Description**                                                                                                                  | **Properties** | 
 | ------------- | ---------------                                                                                                                  | -------------- | 
 | grouppattern  | Featuregroup namepattern filter.                                                                                                 |              | 
 | geotype       | Geotype in featuregroup. Valid values are “point”, “line”, “area” and “unknown”. Default value is “unknown”. |              | 

##### Groupsetting

See paragraph [Groupsetting](#groupsetting).
Values in this groupsetting block overrides the values in the global groupsetting block.

### Add

 | **Attribute** | **Description**          | **Properties** | 
 | ------------- | ---------------          | -------------- | 
 | filepattern	| Filepattern filter. Used to select datasets.	|R           | 
