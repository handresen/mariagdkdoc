---
title: Vector map setup in Maria GDK
keywords: vector, setup
tags: [map, vector]
sidebar: core_sidebar
permalink: core_vector_map_setup.html
summary: Setting up vector maps in Maria GDK 
---

Multiple layers of xml-files work together with the [map template-xml](./core_maps_templates.html) to create a customized map for Maria GDK to display. 

The mappackage-xml is a top level file that apply to the entire collection of datasets, while the multidataset-xml apply to specific datasets. For a complete vector map setup you will also need a set of styling-xmls per dataset (the featuregrouping-xml, the symbols-xml and the script.xml).

{% include image.html file="core/vector/maria2012_vectormaps_html_59e11ee.png" %}

[Map templates](./core_maps_templates.html) are provided to the Maria GDK client application via the [Maria2012 Map Catalog service](maria_gdk/programming/functionality/mapcatalog). 
Templates are searched for in the paths specified in the <templatesources>-section of the map catalog service app.config-file.

Multidataset- and styling-xmls must be located together with the map data they describe, while the location of mappackage-xmls are resolved from paths in the <datasources>-section of the vector map service app.config-file.

{% include callout.html content="Note that all elements and attributes in the xml-files should be lowercase unless otherwise noted." type="info" %}

## Mappackages

The mappackage-xml is used to setup and describe a collection of map datasets and thus enable the creation of a complete, complex map. <br/>
[Details Mappackages](./core_vector_mappackages.html)

## Multidatasets

The multidataset-xml is used to setup and describe similar type datasets. A dataset is typically produced from the same source data/format.<br/>
[Details Multidatasets](./core_vector_multidataset.html)

## Map visualization

The following chapters describe all elements of the symbol-, the scriptcollection- and featuregrouping-xml files.

Default placement of the files is in the folder “m6msymbols”. Placement values can be overridden using the <styleinfo> tag in the relevant multidataset-xml. All paths are relative to the placement of the multidataset-xml.

Example:

```xml
<multidataset>
  <globalsettings>
    <resourcereferences  
      styleroot="m6msymbols" 
      symbols="mgcp\symbols.xml"
      normal="mgcp\ScriptCollection.xml" 
      simple="mgcp\ScriptCollectionSimple.xml" 
      featuregrouping="mgcp\FeatureGrouping.xml"/>
      <…>
  </globalsettings>
  <…>
</multidataset>
```

Please refer to the symbols.xml and script.xml files shipped with Maria GDK and newer for a comprehensive example.

### Feature grouping

Featuregroups are used for grouping map themes into logical groups that are easy to control and style. Featuregroups is the only file that references the dataset and requires that you know what features are available. <br/>
[Details Feature Grouping](./core_vector_featuregroups.html)

### Feature symbolization

The feature symbolization xml (symbols.xml) provides pen-, brush-, symbol-, pattern- and label-definitions for use when styling a dataset. <br/>
[Details Feature Symbolization](./core_vector_symbolization.html)

### Symbolization scripting

The script-xml specifies how to build map styles for the feature groups in the corresponding featuregrouping.xml based on the resources available in the symbols.xml. <br/> 
[Details Script-xml](./core_vector_symbolization_script.html)

