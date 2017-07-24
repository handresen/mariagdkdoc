---
title: Service Configuration
keywords: 
tags: 
sidebar: clientapi_sidebar
permalink: clientapi_serviceconfiguration.html
summary: This section describes how to specify service connection info from app.config.
---

To enable the service related layers to receive information from their services, they need to establish connections!
One way of doing this is by specification in the application configuration (app.config). 

Here is a service connection configuration example with connection info for some of the Maria GDK services,within the ***system.serviceModel*** tag. <br>
For further details on how to administrate Maria GDK services, refer to the system documentation.<br>

```xml
<system.serviceModel>
  <bindings>
    <basicHttpBinding>
      <binding name="myHttpBinding"
               maxBufferSize="2147483647" 
               maxReceivedMessageSize="2147483647" 
               closeTimeout="00:10:00" 
               openTimeout="00:10:00" 
               receiveTimeout="00:10:00" 
               sendTimeout="00:10:00">
        <readerQuotas maxArrayLength="2147483647"/>
      </binding>
    </basicHttpBinding>
  </bindings>
  <client>
    <!-- Catalog service -->
    <endpoint name="MapCatalogService"
              address="http://localhost:9008/catalog"
              contract="TPG.GeoFramework.MapServiceInterfaces.IMapCatalogService"
              binding="basicHttpBinding"
              bindingConfiguration="myHttpBinding"/>

    <!-- Template service -->
    <endpoint name="TemplateService"
              address="http://localhost:9008/maptemplates"
              contract="TPG.GeoFramework.MapTemplateServiceContracts.IMapTemplateService"
              binding="basicHttpBinding"
              bindingConfiguration="myHttpBinding"/>

    <!-- Track service -->
    <endpoint name="TrackService"
              address="http://localhost:9008/tracks"
              contract="TPG.GeoFramework.TrackServiceInterfaces.IMariaTrackService"
              binding="basicHttpBinding"
              bindingConfiguration="myHttpBinding"/>
      
      <!-- Enhanced Elevation service -->
      <endpoint name="EnahncedElevationService" 
                address="http://localhost:9008/enhancedElevation"
                contract="TPG.EnhancedElevationServiceClient.IEnhancedElevationService"
                binding="basicHttpBinding" 
                bindingConfiguration="myHttpBinding"/>

      <!-- Location service -->
      <endpoint name="LocationService" 
                address="http://localhost:9008/location/geoloc" 
                contract="TPG.GeoFramework.LocationServiceInterfaces.ILocationService" 
                binding="basicHttpBinding" 
                bindingConfiguration="myHttpBinding"
                behaviorConfiguration="myEndpointBehavior"/>

      <!-- Draw object service -->
      <endpoint name="DrawObjectService"
                address="http://localhost:9008/drawobjects"
                contract="TPG.GeoFramework.DrawObjectServiceInterfaces.IDrawObjectService" 
                binding="customBinding" 
                bindingConfiguration="binaryHttpBinding"/>

      <!-- Geo Fencing Service -->
      <endpoint name="GeoFencingService/DataManager" 
                address="http://localhost:9008/GeoFencingService/DataManager" 
                contract="TPG.GeoFramework.GeoFencingServiceInterfaces.IDataManagerService" 
                binding="basicHttpBinding" 
                bindingConfiguration="myHttpBinding" />
      
      <endpoint name="GeoFencingService/FenceRule" 
                address="http://localhost:9008/GeoFencingService/FenceRule" 
                contract="TPG.GeoFramework.GeoFencingServiceInterfaces.IGeoFencingRuleService" 
                binding="basicHttpBinding" 
                bindingConfiguration="myHttpBinding" />
      
      <endpoint name="GeoFencingService/NotificationHandling" 
                address="http://localhost:9008/GeoFencingService/NotificationHandling" 
                contract="TPG.GeoFramework.GeoFencingServiceInterfaces.INotificationHandlingService" 
                binding="basicHttpBinding" 
                bindingConfiguration="myHttpBinding" />                          
  </client>
  <behaviors>
    <endpointBehaviors>
      <behavior name="myEndpointBehavior">
        <dataContractSerializer maxItemsInObjectGraph="2147483647"/>
      </behavior>
    </endpointBehaviors>
  </behaviors>
  <extensions></extensions>
</system.serviceModel>
```

{% include links.html %}