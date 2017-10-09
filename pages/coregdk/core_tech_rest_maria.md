---
title: Maria GDK HTTP/REST Api
keywords: api
tags: [api]
sidebar: core_sidebar
permalink: core_tech_rest_maria.html
summary: Using REST in Maria GDK. 
---

## Map services

A map service can be set up to answer on HTTP requests in a RESTful manner by setting up a REST endpoint in the app config. In the following example we have a REST endpoint on the URL /web/tilemapservice

```xml
<system.serviceModel>
   <behaviors>
     <endpointBehaviors>
       <behavior name="RestBehavior">
         <webHttp automaticFormatSelectionEnabled="True"/>
       </behavior>
     </endpointBehaviors>
   </behaviors>
   <bindings>
     <webHttpBinding>
       <binding name="RestBinding"/>
     </webHttpBinding>
   </bindings>
   <services>
     <service name="TPG.GeoFramework.RasterMapService.MariaRasterMapService">
       <clear/>
       <endpoint address="/web/tilemapservice"
                 behaviorConfiguration="RestBehavior"
                 binding="webHttpBinding"
                 bindingConfiguration="RestBinding"
                 contract="TPG.GeoFramework.MapServiceInterfaces.IMariaSimpleWebMapService" />
     </service>
   </services>
 </system.serviceModel>
```

Please note that this is not a complete app.cfg example, you usually will need other bindings for connecting to the catalog service, etc.

By default all service addins installed with the service hoster that support a REST connection have preconfigured REST endpoints. 

With the correct endpoint set up, you should be able to get a map tile simply by GET'ing a HTTP request URL with the following format:

http://\<hostname\>:\<port\>/\<service\>/web/tilemapservice/getmaptile/\<mapsignature\>/\<level\>/\<x\>/\<y\>.\<ext\>

Where 

   * `<hostname>` and `<port>` are the address and port of your service. 
   * `<mapsignature>` is a valid map signature for a map available on this service. See Catalog service section below for how to list all available signatures on a service.
   * `<service>` is the service instance which can be found in the ServiceUri tag returned from the catalog service getservices query, see Catalog service section below.
   * `<level>` is the quadtree zoom level of the tile you want. Level 0 is the root tile covering the entire world.
   * `<x>`,`<y>` Tile coordinates for the wanted tile. This service uses the same tiling as [google maps](https///developers.google.com/maps/documentation/javascript/v2/overlays?csw=1#Google_Maps_Coordinates).
   * `<ext>` File format extension. Can currently be either ''jpg'' for JPEG (lossy) compression or ''png'' for PNG (lossless) compression.

**Example:**

"http://localhost:9008/rastermap/e74f48c4-54ff-412f-894b-c1ee977768c0/web/tilemapservice/getmaptile/BlueMarble/7/67/37.jpg"

In the example above `rastermap/e74f48c4-54ff-412f-894b-c1ee977768c0` is the service instance.

If a request fails, it will return an HTTP error code + the following XML-encoded structure as body:

```xml
<MariaWebMapError>
   <ErrorMessage>Text</ErrorMessage>
   <ResultCode>Integer</ResultCode>
</MariaWebMapError>
```

Where 'ResultCode' corresponds to the enum 'FetchResultCode' from the MARIA GDK API, and the 'ErrorMessage' is some human readable text.

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

"http://\<hostname\>:\<port\>/catalog/web/json/getservices/\<serviceType\>"

where

   * `<hostname>` and `<port>` of course are the address and port of your service. 
   * `<serviceType>` is the type of service you want to query for. Possible values are ''RasterService'', ''VectorService'' and ''CacheService'', but in practice you should only use the CacheService when it's available.

Example:

"http://localhost:9008/catalog/web/json/getservices/RasterService"

This will by default return an JSON array of map entry structures, like for example:

```json
[  
   {  
      "DefaultServiceType":2,
      "MapEntries":[  
         {  
            "MapServiceType":2,
            "MapSignature":"BlueMarble",
            "MapType":2,
            "MapVersion":"1.0.0-D",
            "ResolutionAreas":[  
               {  
                  "Height":170.10225755961318,
                  "Latitude":0,
                  "Longitude":0,
                  "MaxScale":-1,
                  "MinScale":-1,
                  "Resolution":611.49622628141015,
                  "Width":359.99999999999994
               }
            ],
            "TileCacheSpecs":[  
               {  
                  "BaseSize":{  
                     "_height":40075016.685578473,
                     "_width":40075016.685578473
                  },
                  "Origin":{  
                     "_x":-3.7252902984619141E-09,
                     "_y":3.7252902984619141E-09
                  },
                  "PointPixels":false,
                  "Projection":"EPSG:3857",
                  "RootNominalScale":626172135.71216357,
                  "TileSize":256
               }
            ],
            "Visibility":0
         }
      ],
      "ServiceRuntimeInfo":{  
         "ErrorLog":null,
         "FailedRequests":0,
         "LastAliveUTC":"\/Date(1507038457664)\/",
         "ServiceCallCount":null,
         "ServiceLoad":0,
         "SuccessfulRequests":0,
         "TimeLastReportUTC":"\/Date(1506947263781)\/"
      },
      "ServiceTimeout":60,
      "ServiceUri":"http:\/\/tpg-tmd-5520:9008\/rastermap\/284ccb35-d013-4cd5-a65f-bc97261734f2",
      "SessionId":"7a512165-14a0-4954-88f5-e936e0bfb3c1"
   }
]
```