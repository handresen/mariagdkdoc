---
title: Creating readers for location data
keywords: geolocation, readers
tags: [geolocation]
sidebar: core_geolocation_sidebar
permalink: core_geolocation_readers.html
summary: Creating readers for converting placename information to Maria GDK databases.
---

A Maria2012 GeoLoc placename data reader must be able to convert files with placename information to a Maria2012 
readable sqlite databases. Teleplan Globe provides readers for f.ex. [GNS](http://earth-info.nga.mil/gns/html/), [GeoNames](http://www.geonames.org/) and [SSR](http://www.kartverket.no/Kart/Stedsnavn/).

{% include callout.html content="Readers should implement interface [IPlaceNameDataInterfacer](http://support.teleplanglobe.com/MariaGDKDoc/html/748D1169.htm)." type="info" %}


Each reader must implement functions CreateTables and LoadData. *CreateTables* is responsible for creating tables used by the GeoLoc service when running placename searches. Mandatory tables are Rtree for spatial searches, FTS for free text search and main table for available placename information.
Tables also utilised by the GeoLoc service when available are: feature class, feature code, country code and metadata. *LoadData* reads data from sourcefiles into the database tables.

[IPlaceNameDB](http://support.teleplanglobe.com/MariaGDKDoc/html/715B3D03.htm) is available for database creation helper functions.

## Mandatory tables

### Main table

The main table (//placenames_main//) should contain all placename information extracted from a datasource. Use the *placename_alt* column for alternative versions of the main placename data if available. Country code, feature code/class and administrative information colums are used for facets and metadata searches.  

 | Columns       | Description | Type                                                                                    | 
 | -------       | ----------- | ----                                                                                    | 
 | lat           | real        | latitude wgs84 decimal degrees                                                          | 
 | long          | real        |                                                                                         | 
 | feature_class | text        | feature class based on raw data, ex 'H' for hydrography for GNS or 'Samferdsel' for SSR | 
 | feature_code  | text        | feature code, unique code                                                               | 
 | cc1           | text        | Primary country code                                                                    | 
 | cc2           | text        | Secondary country codes, comma separated                                                | 
 | placename     | text        | Placename (reading order with diacritics)                                               | 
 | placename_alt | text        | Alternate spellings of placename, comma separated                                       | 
 | adm1          | text        | Administrative division level 1, US state, Norwegian fylke                              | 
 | adm2          | text        | Administrative division level 2, US county, Norwegian kommune                           | 
 | adm3          | text        | Administrative division level 3, US ?, Norwegian poststed                               | 


### FTS table

The Maria2012 location service uses the [FTS4](http://www.sqlite.org/fts3.html) extension in sqlite to create a table (*placenames_fts*) with a built-in full-text index. This index allows us to efficiently query the database for all rows that contain one or more words/tokens. <br/>
All strings meant to be searchable must be added to the FTS table. Using tagged metadata will ensure more exact searches F.ex. searching for *`<placename>` cc:NO* will return placenames with related country code metadata "NO", while *`<placename>` NO* will match all metadata containing "NO". 

 | Columns       | Description                  | Type | 
 | -------       | -----------                  | ---- | 
 | names         | searchable placenames        | text | 
 | meta_tagged   | searchable tagged metadata   | text | 
 | meta_untagged | searchable untagged metadata | text | 


### RTree table

The [RTree](https://www.sqlite.org/rtree.html) table (*spatial_index*) is a mandatory table used for spatial searches. The Maria2012 location database rtree table should have a primary key and two column pairs representing the minimum and maximum values (bounding box) for a 2-dimensional object. 

 | Columns | Description       | Type    | 
 | ------- | -----------       | ----    | 
 | id      | primary key       | integer | 
 | minlat  | minimum latitude  | float   | 
 | maxlat  | maximum latitude  | float   | 
 | minlong | minimum longitude | float   | 
 | maxlong | maximum longitude | float   | 


## Optional tables

### Feature class table

Optional table (*fclass*) containing feature class/theme information extracted from datasource. <br/>
Example feature classes can be 'H' for hydrography features in GNS or 'Samferdsel' for Norwegian SSR data.

 | Columns | Description                        | Type | 
 | ------- | -----------                        | ---- | 
 | code    | feature class                      | text | 
 | name    | feature name                       | text | 
 | desc    | description of feature class entry | text | 

### Feature code table

Optional table (*fcode*) containing feature code/object type information extracted from datasource. <br/>
Example features can be "lake' in GNS.

 | Columns | Description                       | Type | 
 | ------- | -----------                       | ---- | 
 | code    | feature code                      | text | 
 | name    | feature name                      | text | 
 | desc    | description of feature code entry | text | 

### Country code table

Optional table (*cc*) listing all represented country codes found in the datasource, f.ex. "NO"/"Norway".

 | Columns | Description                | Type    | 
 | ------- | -----------                | ----    | 
 | code    | feature code (primary key) | char(2) | 
 | name    | feature name               | text    | 

### Administration code tables

Optional tables (*admin1, admin2, admin3*) used for collecting administrative information f.ex. state, county, fylke etc.

 | Columns | Description         | Type | 
 | ------- | -----------         | ---- | 
 | code    | administration code | text | 
 | name    | administration name | text | 

### Metadata table

Optional table (*metadata*) used for additional data (f.ex. data source producer, source dataset etc.) collected from datasource.

 | Columns | Description                   | Type | 
 | ------- | -----------                   | ---- | 
 | name    | metadata name                 | text | 
 | value   | metadata value                | text | 
 | desc    | description of metadata entry | text | 

## Sample reader code

See [sample reader code](./core_geolocation_readers_samplecode.html).

