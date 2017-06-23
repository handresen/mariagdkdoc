# Maria HTTP/REST API

`<WRAP center round todo 60%>`

*  Move from root

*  Consider writing general REST section

*  Move specific subject content to appropriate sections (map/catalog)

*  Remove/translate Norwegian text
`</WRAP>`


## Map services

A map service can be set up to answer on HTTP requests in a RESTful manner by setting up a REST endpoint in the  app config. In the following example we have a connection to the catalog service on port 9008 as well as a REST endpoint on the URL /tilemapservice

```xml
  `<system.serviceModel>`
    `<behaviors>`
      `<endpointBehaviors>`
        `<behavior name="RestBehavior">`
          `<webHttp automaticFormatSelectionEnabled="True" />`
        `</behavior>`
      `</endpointBehaviors>`
    `</behaviors>`
    `<bindings>`
      `<webHttpBinding>`
        `<binding name="RestBinding" />`
      `</webHttpBinding>`
    `</bindings>`
    `<services>`
      `<service name="TPG.GeoFramework.CacheService.MariaCacheService">`
        `<clear />`
        `<endpoint address="/tilemapservice" behaviorConfiguration="RestBehavior" binding="webHttpBinding" bindingConfiguration="RestBinding" contract="TPG.GeoFramework.MapServiceInterfaces.IMariaSimpleWebMapService" />`
      `</service>`
    `</services>`
  `</system.serviceModel>`
```

Please note that this is not a complete app.cfg example, you usually will need other bindings for connecting to the catalog service, etc.

With the correct endpoint set up, you should be able to get a map tile simply by GET'ing a HTTP request URL with the following format:

''%%http://`<hostname>`:`<port>`/tilemapservice/getmaptile/`<mapsignature>`/`<level>`/`<x>`/`<y>`.`<ext>`%%''

Where 

   * `<hostname>` and `<port>` are the address and port of your server. 
   * `<mapsignature>` is a valid map signature for a map available on this server. See below for how to list all available signatures on a server.
   * `<level>` is the quadtree zoom level of the tile you want. Level 0 is the root tile covering the entire world.
   * `<x>`,`<y>` Tile coordinates for the wanted tile. This service uses the same tiling as [google maps](https///developers.google.com/maps/documentation/javascript/v2/overlays?csw=1#Google_Maps_Coordinates).
   * `<ext>` File format extension. Can currently be either ''jpg'' for JPEG (lossy) compression or ''png'' for PNG (lossless) compression.

**Example:**

 ''http://localhost:9007/tilemapservice/getmaptile/bluemarble2012/7/67/37.jpg''

If a request fails, it will return an HTTP error code + the following XML-encoded structure as body:

```xml
`<MariaWebMapError>`
   `<ErrorMessage>`Text`</ErrorMessage>`
   `<ResultCode>`Integer`</ResultCode>`
`</MariaWebMapError>`
```

Where ''ResultCode'' corresponds to the enum ''FetchResultCode'' from the MARIA GDK API, and the ''ErrorMessage'' is some human readable text.

Common error results can be:

 | HTTP status           | FetchResultCode   | Meaning |                                        
 | -----------           | ---------------   | ---------                                        
 | 404 (Not found)       | OutsideResolution | Requested a tile with too high or low resolution. | 
 | 404 (Not found)       | OutsideArea       | Requested a tile outside of the data bounds.      | 
 | 404 (Not found)       | Unavailable       | Varies, but may be a temporary error              | 
 | 504 (Gateway Timeout) | NotReady/None     | Timeout while waiting for tile data.              | 
 | 400 (Bad request)     | Varies            | Varies                                            | 

## Catalog service

To query the catalog service for available map data, you simply GET a URL with the following format

''%%http://`<server>`:`<port>`/catservice/getservices/`<serviceType>`%%''

where

   * `<hostname>` and `<port>` of course are the address and port of your server. 
   * `<serviceType>` is the type of service you want to query for. Possible values are ''RasterService'', ''VectorService'' and ''CacheService'', but in practice you should only use the CacheService when it's available.

Example:

''http://localhost:9008/catservice/getservices/CacheService''

This will by default return an XML array of ServiceEntry structures, like for example:

```xml
`<ArrayOfServiceEntry xmlns="http://schemas.datacontract.org/2004/07/TPG.GeoFramework.MapServiceInterfaces" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">`
  `<ServiceEntry>`
    `<DefaultServiceType>`CacheService`</DefaultServiceType>`
    `<MapEntries>`
      `<MapEntry>`
        `<CurrentRevision>`
          0
        `</CurrentRevision>`
        `<FeatureGroups i:nil="true"/>`
        `<MapServiceType>`CacheService`</MapServiceType>`
        `<MapSignature>`bluemarble2012`</MapSignature>`
        `<MapType>`RasterMap`</MapType>`
        `<ResolutionAreas>`
          `<MapResolutionArea>`
            `<Height>`180`</Height>`
            `<Latitude>`0`</Latitude>`
            `<Longitude>`0`</Longitude>`
            `<MaxScale>`
              -1
            `</MaxScale>`
            `<MinScale>`-1`</MinScale>`
            `<Resolution>`463`</Resolution>`
            `<Width>`360`</Width>`
          `</MapResolutionArea>`
          `<MapResolutionArea>`
            `<Height>`0.8274728874939683`</Height>`
            `<Latitude>`59.893163736632623`</Latitude>`
            `<Longitude>`10.720201868443979`</Longitude>`
            `<MaxScale>`-1`</MaxScale>`
            `<MinScale>`
              -1
            `</MinScale>`
            `<Resolution>`0.5`</Resolution>`
            `<Width>`0.72826911277841333`</Width>`
          `</MapResolutionArea>`
        `</ResolutionAreas>`
        `<TileCacheSpec i:nil="true"/>`
      `</MapEntry>`
      `<MapEntry>`
        `<CurrentRevision>`0`</CurrentRevision>`
        `<FeatureGroups i:nil="true"/>`
        `<MapServiceType>`CacheService`</MapServiceType>`
        `<MapSignature>`elevationdata`</MapSignature>`
        `<MapType>`
          Elevation
        `</MapType>`
        `<ResolutionAreas>`
          `<MapResolutionArea>`
            `<Height>`14.2115904996746`</Height>`
            `<Latitude>`64.80144449214059`</Latitude>`
            `<Longitude>`17.808311943386613`</Longitude>`
            `<MaxScale>`-1`</MaxScale>`
            `<MinScale>`-1`</MinScale>`
            `<Resolution>`20`</Resolution>`
            `<Width>`
              29.485544960895936
            `</Width>`
          `</MapResolutionArea>`
          `<MapResolutionArea>`
            `<Height>`149.99999999994`</Height>`
            `<Latitude>`15.00000000003`</Latitude>`
            `<Longitude>`-7.1992189987213351E-11`</Longitude>`
            `<MaxScale>`-1`</MaxScale>`
            `<MinScale>`-1`</MinScale>`
            `<Resolution>`60`</Resolution>`
            `<Width>`
              359.999999999856
            `</Width>`
          `</MapResolutionArea>`
        `</ResolutionAreas>`
        `<TileCacheSpec i:nil="true"/>`
      `</MapEntry>`
      ...
    `</MapEntries>`
    `<ServiceRuntimeInfo>`
      `<ErrorLog i:nil="true" xmlns:a="http://schemas.microsoft.com/2003/10/Serialization/Arrays"/>`
      `<FailedRequests>`0`</FailedRequests>`
      `<LastAliveUTC>`2014-06-04T15:22:54.5292432Z`</LastAliveUTC>`
      `<ServiceCallCount i:nil="true" xmlns:a="http://schemas.microsoft.com/2003/10/Serialization/Arrays"/>`
      `<ServiceLoad>`0`</ServiceLoad>`
      `<SuccessfulRequests>`0`</SuccessfulRequests>`
      `<TimeLastReportUTC>`
        2014-06-04T15:22:54.5292432Z
      `</TimeLastReportUTC>`
    `</ServiceRuntimeInfo>`
    `<ServiceTimeout>`60`</ServiceTimeout>`
    `<ServiceUri>`http://tpg-980-ths:9007/`</ServiceUri>`
    `<SessionId>`c8ba6445-db64-4800-bcd7-f1b58835bef6`</SessionId>`
  `</ServiceEntry>`
`</ArrayOfServiceEntry>`
```

Denne returnerer i utgangspunktet XML, men hvis du vil ha JSON kan du enten spesifisere request-headeren «accept: application/json», eller bruke URL’en

http://localhost:9008/catservice/json/getservices/CacheService

Den siste er altså en egen endpoint som returnerer JSON default på de samme API-kallene.

Jeg valgte å bruke de samme serialiserte objektene vi bruker internt. De inneholder litt mer info enn du ba om, men det ville blitt for dumt å lage egne nedstrippede klasser på siden av de her. Det du får tilbake er en serialisert liste av TPG.GeoFramework.MapServiceInterfaces.ServiceEntry. Her finner du bl.a. Uri’en til servicen. Hver ServiceEntry består av en liste med MapEntries. I MapEntryen finner du kartsignatur og en liste med ResolutionAreas. Et ResolutionArea er en geografisk bounding box med maks oppløsning (En kartsignatur kan bestå av flere datasett med variabel oppløsning). Merk at VectorService-entries ikke har noen ResolutionAreas, da disse i prinsippet kan ha så høy oppløsning som helst.

Hver MapEntry kan også i teorien ha en TileCacheSpec. Som regel er denne null, og du angir tilingen fra klienten, men hvis den er satt, betyr det at dette datasettet kun støtter et begrenset sett av tilinger. Du bør derfor for sikkerhets skyld sjekke dette før du vet at du kan bruke google-tilingen på det.
