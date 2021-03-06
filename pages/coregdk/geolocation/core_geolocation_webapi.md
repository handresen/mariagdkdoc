---
title: Geolocation web API
keywords: geolocation
tags: [geolocation, api, interface, service]
sidebar: core_geolocation_sidebar
permalink: core_geolocation_webapi.html
summary: Using the geolocation web API.
---

## Database information (dbinfo)

Provides a list of placename databases available for query. Uses HTTP GET.
[http://localhost:9005/webgeoloc/json/dbinfo](http://localhost:9005/webgeoloc/json/dbinfo).

No parameters.

Returns array of [LocationDatabaseInfo](http://support.teleplanglobe.com/mariagdkdoc/html/180C1DC4.htm):

```json
[
  {
    "DataSource":"c:\\maria2012\\geoloc\\geonames.location.sqlite",
    "MetaInfo":null
  },
  {
    "DataSource":"c:\\maria2012\\geoloc\\gns_no.location.sqlite",
    "MetaInfo":null
  },
  {
    "DataSource":"c:\\maria2012\\geoloc\\ssr.location.sqlite",
    "MetaInfo":null
  },
  {
    "DataSource":"c:\\maria2012\\geoloc\\world.location.sqlite",
    "MetaInfo":null
  }
]
```

## Placename queries (find)

Query placename database. Uses HTTP GET. \\
[http://localhost:9005/webgeoloc/json/find/ssr/sandvika?lat=60&lon=50&facets=false&maxreturnhits=2](http://localhost:9005/webgeoloc/json/find/ssr/sandvika?lat=60&lon=50&facets=false&maxreturnhits=2)

 | parameter     | type   | description                                                                                | 
 | ---------     | ----   | -----------                                                                                | 
 | datasource    | string | Name of database to query.                                                                 | 
 | query         | string | Query string. See [performing placename searches](.service#Performing placename searches). | 
 | lat           | double | Latitude. Used to set the start latitude for a query. Default latitude is 0.0.             | 
 | lon           | double | Longitude. Used to set the start longitude for a query. Default longitude is 0.0.          | 
 | maxreturnhits | int    | Controls the maximum number of matches return from query. Default value is 100.            | 
 | facets        | bool   | If true will extract facets from query results. Default value is false.                    | 

Returns [PlaceNameQueryResult](http://support.teleplanglobe.com/mariagdkdoc/html/B597738D.htm):

```json
{
  "Facets":null,
  "Matches":
  [
    {
      "MatchInfo":1,
      "PlaceName":      
      {        
        "Adm1":"",
        "Adm2":"",
        "Adm3":"",
        "City":null,
        "Country":"NO",
        "Distance":1438469.8849214201,
        "FeatureClass":"FC4",
        "FeatureCode":"N83",
        "Id":760721,
        "Name":"Sandvika",
        "Position":
        {
          "Lat":70.377036,
          "Lon":31.098339
        }
      }
    },
    {
      "MatchInfo":1,
      "PlaceName":
      {
        "Adm1":"",
        "Adm2":"",
        "Adm3":"",
        "City":null,
        "Country":"NO",
        "Distance":1443062.1046825086,
        "FeatureClass":"FC4",
        "FeatureCode":"N83",
        "Id":558979,
        "Name":"Sandvika",
        "Position":
        {
          "Lat":70.078058,
          "Lon":30.1298
        }
      }
    }
  ],
  "Status":0,
  "TotalServerMatchCount":100
}
```

Use facets in search query to narrow down matches ("kyll cc_FI").
[http://localhost:9005/webgeoloc/json/find/world/kyll%20cc_FI?facets=false&maxreturnhits=2](http://localhost:9005/webgeoloc/json/find/world/kyll%20cc_FI?facets=false&maxreturnhits=2)

Returns array with max 2 matches where placename contains the substring "kyll" and facet "cc" (country) is "FI".

```json
{
  "Facets":null,
  "Matches":
  [
    {
      "MatchInfo":2,
      "PlaceName":
      {
        "Adm1":"",
        "Adm2":"",
        "Adm3":"",
        "City":null,
        "Country":"FI",
        "Distance":7174187.1638730653,
        "FeatureClass":"P",
        "FeatureCode":"PPL",
        "Id":2688180,
        "Name":"Kylliälä",
        "Position":
        {
          "Lat":61.116667,
          "Lon":27.2
        }
      }
    },
    {
      "MatchInfo":2,
      "PlaceName":
      {
        "Adm1":"",
        "Adm2":"",
        "Adm3":"",
        "City":null,
        "Country":"FI",
        "Distance":7187173.8056581328,
        "FeatureClass":"P",
        "FeatureCode":"PPL",
        "Id":2688189,
        "Name":"Kylliälä",
        "Position":
        {
          "Lat":61.133333,
          "Lon":27.616667
        }
      }
    }
  ],
  "Status":0,
  "TotalServerMatchCount":8
}
```

## Find placenames inside range (findrange)

Provides a list of placenames found inside a range. Uses HTTP GET. \\
[http://localhost:9005/webgeoloc/json/findrange/ssr?rangemeters=400&lat=59.92&lon=10.70&maxcount=2](http://localhost:9005/webgeoloc/json/findrange/ssr?rangemeters=400&lat=59.92&lon=10.70&maxcount=2)


 | parameter   | type   | description                                                                       | 
 | ---------   | ----   | -----------                                                                       | 
 | datasource  | string | Name of database to query.                                                        | 
 | rangemeters | int    | Range in meters.                                                                  | 
 | lat         | double | Latitude. Used to set the start latitude for a query. Default latitude is 0.0.    | 
 | lon         | double | Longitude. Used to set the start longitude for a query. Default longitude is 0.0. | 
 | maxcount    | int    | Controls the maximum number of matches returned from query. Default value is 100. | 

Returns array of [PlaceNameItem](http://support.teleplanglobe.com/mariagdkdoc/html/B7F470FE.htm):

```json
[
  {
    "Adm1":"",
    "Adm2":"",
    "Adm3":"",
    "City":null,
    "Country":"NO",
    "Distance":129.910504317276,
    "FeatureClass":"FC6",
    "FeatureCode":"N140",
    "Id":924186,
    "Name":"Bygdøy allé",
    "Position":
    {
      "Lat":59.919039,
      "Lon":10.701328}
  },
  {
    "Adm1":"",
    "Adm2":"",
    "Adm3":"",
    "City":null,
    "Country":"NO",
    "Distance":384.71061547280669,
    "FeatureClass":"FC2",
    "FeatureCode":"N36",
    "Id":926460,
    "Name":"Frognerelva",
    "Position":
    {
      "Lat":59.923042,
      "Lon":10.696703
    }
  }
]
```
