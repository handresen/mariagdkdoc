---
title: Map Client
keywords:  
tags: 
sidebar: clientapi_sidebar
permalink: clientapi_mapclient.html
summary: This section describes how to create Maria GDK map client window.
---
## Creating the Map Client Window
Create a  ***WPF Window*** (main application window or sub window) - *MariaWpfWindow* - to be used for your map client.<br>
For building Maria GDK clients in Windows forms applications, see [Maria Windows-Forms Client](clientapi_winformsclient.html).

{% include image.html file="clientapi/basicmap/creatingmapclientwindow.png" caption="Creating the map window component" %}

### Including MariaUserControl
* For this part you will need ***TPG.Maria.MariaUserControl*** NuGet package as a minimum.\\
See [Loading Maria GDK, NuGet Packages](maria_gdk/programming/loading_maria_2012_packages)


Add the MariaUserControl to the xaml of your window, name of your choice, optionally including properties for the integrated map controls.

```xml
<Window x:Class="BasicMapClient.MariaWpfWindow"
 . . . 
 xmlns:MariaUserControl= "clr-namespace:TPG.Maria.MariaUserControl; assembly=TPG.Maria.MariaUserControl"
 Title="MariaWpfWindow" Height="550" Width="525" >
 <Grid>
 <MariaUserControl:MariaUserControl Name="MariaCtrl"
                                    IsMiniMapVisible="True"
                                    IsPanNavigationVisible="True"
                                    IsScaleBarVisible="True"
                                    IsRulerVisible="True"
                                    />
  </Grid>
</Window>
```
    
### Main view model

 Create a class (*MariaWindowViewModel*) for communication with the Maria component.

In the constructor of your window (*MariaWpfWindow*) Set the data context of your client window to be this class.

```csharp
public MariaWpfWindow()
{
  InitializeComponent();
  DataContext = new MariaWindowViewModel();
}
```

### Handle the WindowClosing event

In the window holding your *MariaUserControl* (*MariaWpfWindow*), implement an event handler for the *WindowClosing* event, disposing the *MariaUserControl* object.

In the XAML :

```xml
<Window x:Class="BasicMapClient.MariaWpfWindow"
 . . . 
 Title="MariaWpfWindow" Height="550" Width="525" Closing="WindowClosing">
   <Grid>
      <MariaUserControl:MariaUserControl
      . . .
```
In the code behind:

```csharp
private void WindowClosing(object sender, System.ComponentModel.CancelEventArgs e)
{
  MariaCtrl.Dispose();
}
```

### Preparing for creation of Maria layers

In the main view model class, *MariaWindowViewModel*, create an auto property to hold a list of Maria layers, *Layers*, and add a constructor, initializing the propery.

```csharp
public ObservableCollection<IMariaLayer>Layers { get; set; }

internal MariaWindowViewModel()
{
    Layers = new ObservableCollection<IMariaLayer>();
}
```
Then, bind the Layers property of the MariaUserControl to this list.

```xml
<MariaUserControl:MariaUserControl Name="MariaCtrl"
                                   Layers="{Binding Layers}"
                                   IsMiniMapVisible="True"
                                   IsPanNavigationVisible="True"
                                   IsScaleBarVisible="True"
                                   IsRulerVisible="True"
                                   />
```

<div class="alert alert-success" role="alert"><i class="fa fa-arrow-circle-right"></i><b> Run:</b><br> 
Building and running your application, the window area should now show an empty area including navigation controls according to your specifications!
</div>


{% include image.html file="clientapi/basicmap/emptymapwithcontrols.png" caption="Empty map with controls" %}

### Layer interaction in general
For each of the desired layers (will be described in detail for each layer type):

*  Create an instance of the corresponding GDK layer class 
*  Add the created layer to the *MariaWindowViewModel* Layers property. 
*  Create a separate view model class for each layer (may be skipped, if few layers)
*  Add event handler(s) for the *LayerInitialized* event(s). 

The layers are accessed programmatically from your application  through the different GDK layer interfaces.

<div class="alert alert-info" role="alert"><i class="fa fa-exclamation-circle"></i> <b> Note:</b><br> 
-  Rendering of the layers will be in the same order as they are in the Layers property.<br>
-  No access should be performed against the layer until the <b>LayerInitialized</b> event has been called.
</div>

{% include links.html %}