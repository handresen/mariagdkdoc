---
title: Map Layer
keywords: 
tags: [map]
sidebar: clientapi_sidebar
permalink: clientapi_maplayer.html
summary: This section describes how to create a map layer with connection to the Maria Map services.
---
## Creating a Map Layer with Map Service Connection

The map layer is accessed programmatically through the map layer interface, [***IMariaMapLayer***](http://maria.support2.teleplan.no/MariaGDKDoc/html/C8909007.htm),
and the extended map layer interface [***IMariaExtendedMapLayer***](http://maria.support2.teleplan.no/MariaGDKDoc/html/BA316F90.htm) .

* For this part you will need to include ***TPG.Maria.MapLayer*** NuGet package.

Create a view model class (*MapViewModel*) to handle intaractions toward the map layer, and add the following variables and properties:

```csharp
private readonly IMariaMapLayer _mapLayer;

public IMariaMapLayer MiniMapLayer { get; set; }
public GeoPos CenterPosition { get; set; }
public double Scale { get; set; }
```

Create the MapViewModel constructor, initializing map and minimap event handling:

```csharp
public MapViewModel(IMariaMapLayer mapLayer, IMariaMapLayer miniMapLayer)
{
    _mapLayer = mapLayer;
    _mapLayer.LayerInitialized += OnMapLayerInitialized;
    
    MiniMapLayer = miniMapLayer;
    MiniMapLayer.LayerInitialized += OnMiniMapLayerInitialized;
}
```

 Add the *LayerInitialized* event handlers for the map and mini map:

```csharp
private void OnMapLayerInitialized()
{
    Scale = 50000;
    CenterPosition = new GeoPos(60, 10);

    _mapLayer.ActiveMapTemplate = PreferredMapTemplate();
}
private void OnMiniMapLayerInitialized()
{
    MiniMapLayer.ActiveMapTemplate = PreferredMapTemplate();
}

private MapTemplate PreferredMapTemplate()
{
    var preferred = "WorldMap";
    foreach (var template in _mapLayer.ActiveMapTemplates)
    {
        if (template.Name == preferred)
            return template;
    }
    return _mapLayer.ActiveMapTemplates.Any() ? _mapLayer.ActiveMapTemplates.Last() : null;
}
```

Verify that your *app.config* file contains endpoint definitions for the ***MapCatalogService*** and the ***TemplateService***, as described in the [Service Configuration](clientapi_serviceconfiguration.html) section.

Then, include the MapViewModel in the main view model (*MariaWindowViewModel*).

Declarations:

```csharp
public MapViewModel MapViewModel { get; set; }

private IMariaMapLayer _mapLayer;
private MapLayer _miniMapLayer;
```

and in the constructor

```csharp
IBindingFactory bindingFactory = new BindingFactory();
IEndpointAddressFactory endpointAddressFactory = new EndpointAddressFactory();

var mapCatalogServiceClient  = 
    new MapCatalogServiceClientFactory(bindingFactory, endpointAddressFactory).New("MapCatalogService");
var mapTemplateServiceClient = 
    new MapTemplateServiceClientFactory(bindingFactory, endpointAddressFactory).New("TemplateService");

_mapLayer = new MapLayer(mapCatalogServiceClient, mapTemplateServiceClient);
_miniMapLayer = new MapLayer(mapCatalogServiceClient, mapTemplateServiceClient);

MapViewModel = new MapViewModel(_mapLayer, _miniMapLayer);
Layers.Add(_mapLayer);
```

Update MariaUserControl binding

```xml
<MariaUserControl:MariaUserControl Name="MariaCtrl"
                                   Layers="{Binding Layers}"
                                   IsMiniMapVisible="True"
                                   IsPanNavigationVisible="True" 
                                   IsScaleBarVisible="True" 
                                   IsRulerVisible="True" 
                                   CenterScale="{Binding MapViewModel.Scale}" 
                                   CenterPosition="{Binding MapViewModel.CenterPosition}" 
                                   MiniMapLayer="{Binding MapViewModel.MiniMapLayer}"     
                                   />
                                                                      
```

<div class="alert alert-success" role="alert"><i class="fa fa-arrow-circle-right"></i><b> Run:</b><br> 
Running your application, the window area should now include map information and mini map - and you should be able to navigate the map!
</div>

{% include image.html file="clientapi/basicmap/basicmapclientmap.png" caption="Map area with map information" %}

{% include links.html %}