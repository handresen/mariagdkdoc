---
title: Geofencing service
keywords: geofencing
tags: [navigation]
sidebar: core_geofencing_sidebar
permalink: core_geofencing_service.html
summary: WCF service providing geofencing functionality.
---

# Geofencing service

The geofencing service is a WCF service that provides core geofencing functionality. The following service interfaces are provided:

 | Name                                                                                               | Description                                            | 
 | ----                                                                                               | -----------                                            | 
 | [IDataManagerServiceBase](http://support.teleplanglobe.com/MariaGDKDoc/html/32457EC9.htm)          | Data source management                                 | 
 | [IGeoFencingRuleServiceBase](http://support.teleplanglobe.com/MariaGDKDoc/html/36DA80D0.htm)       | Rule management                                        | 
 | [INotificationHandlingServiceBase](http://support.teleplanglobe.com/MariaGDKDoc/html/754E0C07.htm) | Notification handling, including event log and restore | 


WCF basic http binding is used by default. 

{% include callout.html content="Co-hosting with other services is possible in order to reduce the number of required IP ports. On a single user system, co-hosting with track service and draw object service is an option. See [Hosting](http://blogs.msdn.com/b/dkaufman/archive/2008/06/13/hosting-multiple-service-implementation-on-the-same-port-with-wcf.aspx)" type="info" %}


### Deployment and configuration

Service deployment and configuration is decided by the system integrator. Several scenarios are possible:

*  Single geofencing service is used to service all clients. Rule and data source setup is performed by setup/admin function

*  One geofencing service per client. This allows easy setup of personal notifications. Client is responsible for data source and rule setup

*  Multiple services, multiple clients. Each geofencing service produces specific types of notifications. Using multiple data sources allows separation of data sources

#### Client sample

Use standard WCF app config to connect to one the required interfaces, or connect by using code:

```csharp
var eaDataManager = new EndpointAddress(
  "http://localhost:9004/GeoFencingService/DataManager");
var b = Binding b = new BasicHttpBinding();
var dataManagerClient = new DataManagerServiceClient(b, eaDataManager);

// In production code where automatic reconnects are required,
// use Connect() instead of ConnectAndWait()
dataManagerClient.ConnectAndWait();
dataManagerClient.GetDataSources();
```
Service paths for the geofencing interfaces are:

```csharp
"http://<host>:<port>/GeoFencingService/DataManager"
"http://<host>:<port>/GeoFencingService/FenceRule"
"http://<host>:<port>/GeoFencingService/NotificationHandling"
```
    
### Geofencing test client

The geofencing test client is distributed with the geofencing service. It can be used to inspect a running geofencing service and perform basic service monitoring.

{% include image.html file="./core/geofencing/testclient.png" %}
 
