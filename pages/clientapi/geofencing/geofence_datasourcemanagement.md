---
title: Geo Fence, Data Source Management
keywords: 
tags: 
sidebar: clientapi_sidebar
permalink: geofence_datasourcemanagement.html
summary: This section describes how to ...
---

Geo Fence Data Sources are accessed programmatically through the [***IDataManagerService***](http://maria.support2.teleplan.no/MariaGDKDoc/html/41274E03.htm) interface.


<div class="alert alert-info" role="alert"><i class="fa fa-exclamation-circle"></i> <b> Note:</b><br> 
Please note that Data Source Management will have effect for all clients connected to the  service.<br>
In a multi-client environment, Data Source Management will typically be a task performed by administrators only.
</div>

For additional information, see [Developer Reference, GeoFencing - Data sources](core_geofencing_datasources.html).

## Connect to service
 
Create a view model class (*GeoFenceDataSourceViewModel*) for data source management, inheriting ViewModelBase.

```csharp
 public class GeoFenceDataSourceViewModel : ViewModelBase
 {
 }
```

Connect to the DataManager part of the GeoFencingService from the constructor. The code will look like this:

```csharp
private IBindingFactory _bindingFactory = new BindingFactory();
private IEndpointAddressFactory _endpointAddressFactory = new EndpointAddressFactory();
private DataManagerServiceClient _clientDataManager;
. . .
public GeoFenceDataSourceViewModel()
{
    StatusDataMgrCon = "-";

    _clientDataManager =
        new DataManagerServiceClient(
            _bindingFactory.NewFromConfigurationFile("GeoFencingService/DataManager"),
            _endpointAddressFactory.NewFromConfigurationFile(
              "GeoFencingService/DataManager"));
    _clientDataManager.ServiceConnected += OnDataManagerServiceConnected;
    _clientDataManager.Connect();
}

private void OnDataManagerServiceConnected(object sender, EventArgs args)
{
}

```

Make sure that your App.config file contains an end point specification for GeoFencingService/DataManager. See [Service Configuration](maria_gdk/programming/getting_started/mariabasicmapclient/service_configuration).

Then, define and instansiate the GeoFenceDataSourceViewModel in the declarations and constructor of the main view model (MariaWindowViewModel).

```csharp
public GeoFenceDataSourceViewModel GeoFenceDataSourceViewModel { get; set; }
. . .
public MariaWindowViewModel()
{
    . . .
    GeoFenceDataSourceViewModel = new GeoFenceDataSourceViewModel();
    . . .
}

```

##  Defining Data Sources

When the service connection is established, the datasources can be added. The code will look like this:

```csharp
private void OnDataManagerServiceConnected(object sender, EventArgs args)
{
    AddDataSources();
}
private void AddDataSources()
{
    var trackDefinition = new DataSourceDefinition
    {
        DefinitionId = 
          new Guid("{EEFBFC23-1574-492B-B05B-B443BD6FB642}"), // or Guid of your choise!
        ObjectType = GenericObjectType.Track,
        ServiceURI = 
          _endpointAddressFactory.NewFromConfigurationFile("TrackService").Uri.ToString(),
        ListId = "MariaGeoFence.Sample"
    };
    _clientDataManager.AddDataSource(trackDefinition);

    var drawObjectDefinition = new DataSourceDefinition
    {
        DefinitionId = 
          new Guid("{2630207A-DF9A-4DC1-ADA9-BF6CEB6DBF04}"), // or Guid of your choise!
        ObjectType = GenericObjectType.GeoShape,
        ServiceURI = 
          _endpointAddressFactory.NewFromConfigurationFile(
              "DrawObjectService").Uri.ToString(),
        ListId = "MariaGeoFence.Sample"
    };
    _clientDataManager.AddDataSource(drawObjectDefinition);
}
```

{% include links.html %}