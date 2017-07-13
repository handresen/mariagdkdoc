---
title: Maria GDK service discovery
keywords: service addin installation
tags: [service]
sidebar: core_sidebar
permalink: core_setup_service_discovery.html
summary: Using WCF UDP discovery with Maria GDK services. 
---

Many Maria GDK services can be set up to provide WCF UDP discovery. This includes, but is not limited to:

*  Catalog service (IMapCatalogService)

*  Preparation service (IMariaMapPreparationService)

*  Track service (IMariaTrackService)

*  Draw object store service (IDrawObjectService)

*  Location/placename search service (ILocationService)

Discovery allows a service client to search for and connect to available service at runtime.
   
### Service setup

Set "DiscoveryEnabled" to **true** in the settings.json service configuration file. Note that service discovery is disabled by default for all services.

Example:

```javascript
{
    "Version": "1.0",

    "GeneralSettings": {
        "InstallDir": "...",
        "ServicePort": "9008",
        "RegisterAs": "http://XXX:[port]",
        "LogPath": "%temp%\\Maria2012Log",
        "MaxLogFiles": "3",
        "MaxLogSize": "10MB"
    },
    "MapCatalogSettings": {
        "DiscoveryEnabled": true
    },
	 "TrackSettings": {
        "DiscoveryEnabled": true
    }
  // ...
}
```

### Discovering services

Use the class [ServiceDiscoveryHelper](http://support.teleplanglobe.com/mariagdkdoc/html/CB808DC3.htm) to perform simple service discovery. 

```csharp
ServiceDiscoveryQuery sdq = new ServiceDiscoveryQuery();
sdq.Duration = TimeSpan.FromSeconds(1);
sdq.Interfaces.Add("IMapCatalogService");
sdq.Interfaces.Add("IMapTemplateService");

var discoveryHelper = new ServiceDiscoveryHelper();
var discovered = discoveryHelper.DiscoverServices(
  sdq, a=>Console.WriteLine(a.Address.AbsoluteUri+ " ver:"+a.Version));
```

#### Version filtering

Set ServiceDiscoveryQuery member [WantedVersion](http://support.teleplanglobe.com/mariagdkdoc/html/98B62DCB.htm) to filter the results based on reported version. The version is set based on the assembly version of the service implementation. By specifying only the most significant parts of the version, eg "3.1", all patch versions 3.1.x will be returned.

```csharp
sdq.WantedVersion="3.1";
```

