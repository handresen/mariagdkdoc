---
title: Geolocation service
keywords: geolocation
tags: [geolocation, service]
sidebar: core_geolocation_sidebar
permalink: core_geolocation_service.html
summary: WCF service providing geolocation functionality for Maria GDK.
---

The geolocation service is a WCF-service that provides functionality for placename searches via a sqlite/FTS database. 

The following service interfaces are provided:

 | Name                                                                               | Description      | 
 | ----                                                                               | -----------      | 
 | [ILocationService](http://support.teleplanglobe.com/MariaGDKDoc/html/8BEE5ECE.htm) | Placename search | 


WCF basic http binding is used by default.

## Deployment and configuration


### Locating databases

The Geolocation service searches for available placename databases in the default folders provided at installation. If needed, [links.xml](./../../../maps/config/links)-files can be used to point to yet other folders containing databases.

Databases are identified using the extension *.location.sqlite*.

Example database.links.xml-file:

```xml
<links>
  <link path="H:\GeoLocation\" recursiondepth=”3”/>
  <link path="C:\GeoLocation\databases\"/>
</links>
```

## Supported readers

{% include callout.html content="Describe supported readers, example input formats, versions supported." type="info" %}

## Performing placename searches

Searches can include multiple terms separated with comma or space. All terms must be found to make a match in the database.
Wildcard-character '\*' are permitted, but only at the end of a string/substring. The service will automatically try to add '\*' to the end of submitted search-strings, but not if the string already contains a wildcard character.

```text
Example: 
Say the database contains the placenames Bred Sund, Sunde, Sundnes and Brandal på Sunnmøre. 

Searching for "sun" will be expanded by the service to "sun*" and return 4 matches. 
Searching for "sund" will be expanded to "sund*" and return 3 matches (Bred Sund, Sunde and Sundnes). 
Searching for "sun* br*" will return 2 matches (Bred Sund and Brandal på Sunnmøre). 
Searching for "sun* br" will NOT be expanded by the service since it already contains a wildcard, and return 0 matches.
```

The location service will look for matches in the FTS table contained in the sqlite database. This table contains placenames as well as tagged and untagged metadata (f.ex. country codes). Using tags will allow more specific searces, f.ex. searching for all norwegian schools with placenames starting with "sund" can be done like this:

*"sund cc:NO fclass:school"*

Returned placename matches are ranked according to distance from center position in map. If searching without wildcards, all excact matches are returned first (ranked by distance), then all non-excact matches (also ranked by distance). If searching with wildcards, results are ranked by distance.

```text
Example: 
The database contains placenames Sandelva, Sande Skole, Sande, Sanden and Sande Stadion (listed by distance from center, that is Sande is closest to center and Sande Station furthest away).

Searching for "Sande" will return Sande Skole, Sande, Sande Stadion, Sandelva, Sanden.
Searching for "Sande*" will return Sandelva, Sande Skole, Sande, Sanden and Sande Stadion.
```

### Setting up placename queries

[PlacenameQuery](http://support.teleplanglobe.com/MariaGDKDoc/html/76C14EAC.htm) data contract specifies the query parameters needed when performing a placename search. \\

Example query:

```csharp
var query = new PlaceNameQuery
{
  AutoAddLocationWildcard = true,
  CenterPos = new GeoPos(60.0, 10.0),
  ExtractFacets = true,
  MaxInternalHits = 10000,
  MaxReturnHits = 10,
  NameSearchString = "sand"
};
```

Searching for "Sand" with AutoWildcard set to true, will f.x. return both "Sand" and "Sandvika", while AutoWildcard set to false will require an exact match and only return "Sand". The string must be at least 2 characters long and not already contain wildcards for AutoAddLocationWildcard to work. Wildcard-characters inside a search substring are not allowed. 

CenterPos influences how the search results are ranked. If exact matches are found, results will be sorted in order of shortest distance from centerpos. 

If true, [facets](#using-facets) are extracted from the search results. Use the Facets-parameter to control which facets should be extracted. 

The NameSearchString is the primary placename text search string. Terms separated by spaces and commas must all be found. Wildcard '*' is allowed. Use metadata to narrow down the search results by adding tagged values on the form tag:value or tag=value (:norway or cc:no or cc=no). Use "-" to exclude terms from the search. 

Use ResultOffset to limit the number of results returned from a query and enable paging of results.

F.x. MaxReturnHits set to 10 will return maximum 10 results from a query. If total hits found on server is 25, and ResultOffset is set to 10, the query will return entries 11-20. If ResultOffset is set to 20, query will return only the last five entries (21-25).

There is a max limit on returned searches set to 20000. If a search exceeds this limit, no matches are returned.

### Using facets

A search query will return a list of facets related to the search query results. Facets are used to narrow down the search results and perform more specific searches, f.ex. only display results related to Administrative Regions in Norway. 

It is the reader for the input-data who decides how to map data to facets. Facet groups with single entries are ignored. 

There are six categories of facets available:

 | Category      | Description                                                                              | 
 | --------      | -----------                                                                              | 
 | Feature Class | Feature class based on raw data, ex 'H' for hydrography for GNS or 'Samferdsel' for SSR. | 
 | Feature Code  | Feature code, unique code.                                                               | 
 | CC/Country    | Primary country code.                                                                    | 
 | Adm1          | Administrative division level 1, US state, Norwegian fylke.                              | 
 | Adm2          | Administrative division level 2, US county, Norwegian kommune.                           | 
 | Adm3          | Administrative division level 3, US ?, Norwegian poststed                                | 

