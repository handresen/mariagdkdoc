---
title: Very Simple Map Client
keywords: GDK
tags: [sampleapplication, wpf]
sidebar: clientapi_sidebar
permalink: clientapi_verysimpleclient.html
summary: This section describes how to create a very simple Maria GDK map client. 
---

{% include image.html file="clientapi/verysimpleclient/verysimpleclient.png" max-width=500 caption="Very simple map client" %}

### Prerequisites

* Make sure that all ***[Development Requirements](clientapi_development_requirements.html)*** are met!
* You will need to include the ***TPG.Maria.MapLayer*** NuGet package as a minimum.
* Sample code for this section is found in the ***MariaVerySimpleClient*** solution folder of the ***SampleProjects*** solution.
     
### Creating the Map Client  

#### Create a new project

In Visual Studio, create a WPF Application project - ***VerySimpleMapClient*** - to be used for your map client.

{% include image.html file="clientapi/verysimpleclient/mapclientproject.png" max-width=400 caption="Map Client Project" %}

You will now have a new WPF window, ***MainWindow.xaml***, with "code behind" file, ***MainWindow.xaml.cs***.

{% include image.html file="clientapi/verysimpleclient/mapclientwindow.png" max-width=500 caption="Map Client Window" %}

Set the title of the window to "Very Simple Map Client".

#### Add required NuGet packages

From the NuGet Package Manager, load the ***TPG.Maria.MapLayer*** package, as described in [Maria GDK Nuget Packages](clientapi_development_requirements.html#mariagdkpackages) in Development Requirements.

#### Include MariaUserControl

* Add ***MariaUserControl*** to the main window *xaml*.
* Also add an eventhandler for the window closing event, in the xaml and code behind. 

```xml
<Window x:Class="VerySimpleMapClient.MainWindow"
        . . .
        xmlns:mariaUserControl="clr-namespace:TPG.Maria.MariaUserControl;assembly=TPG.Maria.MariaUserControl"
        . . .
        Closing="WindowClosing">
    <Grid>
        <mariaUserControl:MariaUserControl Name="MariaCtrl" />
    </Grid>
</Window>
```


```c#
private void WindowClosing(object sender, CancelEventArgs e)
{
    MariaCtrl.Dispose();
}
```

### Creating the Map Layer with Map Service Connection

#### Service Configuration Setup 

Create service configuration for connections to the Catalog service and Template service as a minimum. See [Service Configuration](clientapi_serviceconfiguration.html)

#### Include layer handling.

In the ***MainWindow*** constructor:

* Initialize ***MariaCtrl***
* Initialize the binding and service client connetions:
* Initialize the map- and minimap- layers, and add eventhandlers for the ***LayerInitialized*** event in each 

```c#
        public MapLayer MapLayer { get; private set; }
        public IMariaMapLayer MiniMapLayer { get; private set; }

        public MainWindow()
        {
            InitializeComponent();

            MariaCtrl.Layers = new ObservableCollection<IMariaLayer>();
            MariaCtrl.IsMiniMapVisible = true;
            MariaCtrl.IsPanNavigationVisible = true;
            MariaCtrl.IsCenterPositionIndicatorEnabled = true;
            MariaCtrl.IsRulerVisible = true;
            MariaCtrl.IsScaleBarVisible = true;

            IBindingFactory bindingFactory = new BindingFactory();
            IEndpointAddressFactory endpointAddressFactory = new EndpointAddressFactory();

            var mapCatalogServiceClient = new MapCatalogServiceClientFactory(bindingFactory, endpointAddressFactory).New("MapCatalogService");
            var mapTemplateServiceClient = new MapTemplateServiceClientFactory(bindingFactory, endpointAddressFactory).New("TemplateService");

            MapLayer = new MapLayer(mapCatalogServiceClient, mapTemplateServiceClient);
            MapLayer.LayerInitialized += OnMapLayerInitialized;
            MariaCtrl.Layers.Add(MapLayer);

            MiniMapLayer = new MapLayer(mapCatalogServiceClient, mapTemplateServiceClient);
            MiniMapLayer.LayerInitialized += OnMiniMapLayerInitialized;
            MariaCtrl.MiniMapLayer = MiniMapLayer;
        }

        private void OnMapLayerInitialized()
        {
            MariaCtrl.CenterScale = 100000;
            MariaCtrl.CenterPosition = new GeoPos(60, 10);

            MapLayer.ActiveMapTemplate = PreferredMapTemplate();
        }
        private void OnMiniMapLayerInitialized()
        {
            MiniMapLayer.ActiveMapTemplate = PreferredMapTemplate();
        }

        private MapTemplate PreferredMapTemplate()
        {
            var preferred = "WorldMap";
            foreach (var template in MapLayer.ActiveMapTemplates)
            {
                if (template.Name == preferred)
                    return template;
            }
            return MapLayer.ActiveMapTemplates.Any() ? MapLayer.ActiveMapTemplates.First() : null;
        }
```

Running the project, the application window should look like this:

{% include image.html file="clientapi/verysimpleclient/verysimpleclient.png" caption="Very simple map client" %}

{% include links.html %}