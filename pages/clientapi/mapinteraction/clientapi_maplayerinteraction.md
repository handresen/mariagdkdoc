---
title: Map Interaction
keywords: 
tags: [map]
sidebar: clientapi_sidebar
permalink: clientapi_maplayerinteraction.html
summary: This section describes how to interact with Maria User control and Maria map layer functionality.
---
##  Maria navigation control management

***MariaUserControl*** contains built-in navigation controls and navigation commands.

To enable/disable navigation controls, add four check boxes (Ruler, Pan Navigation, Scale Bar and Mini Map) - and bind them to ***MariaUserControl***.

```xml
<CheckBox Content="Ruler" ToolTip="Ruler" 
              Name="IsRulerVisible" 
              IsChecked="{Binding IsRulerVisible, ElementName=MariaCtrl}" />
<CheckBox Content="Pan Navigation" ToolTip="Pan Navigation" 
              Name="IsPanNavigationVisible" 
              IsChecked="{Binding IsPanNavigationVisible, ElementName=MariaCtrl}" />
<CheckBox Content="Scale Bar" ToolTip="Scale Bar" 
              Name="IsScaleBarVisible" 
              IsChecked="{Binding IsScaleBarVisible, ElementName=MariaCtrl}" />
<CheckBox Content="Mini Map" ToolTip="Mini Map" 
              Name="IsMiniMapVisible1" 
              IsChecked="{Binding IsMiniMapVisible, ElementName=MariaCtrl}" />
```

You should now be able to toggle the display of the navigation tools.

{% include image.html file="clientapi/mapinteraction/managenavigationctrls.png" caption="Manage built in navigation controls" %}

##  Navigating the map

To access the navigation commands directly, add four pushbuttons (Pan Left, Pan Right, Pan Up, Pan Down) to your main window (wrap panel) -- and bind them to the ***MariaUserControl***.

```xml
<Button Content="Pan Left" ToolTip="Pan Left" 
        Name="PanLeftCommand" 
        Command="{Binding PanLeftCommand, ElementName=MariaCtrl}" />
<Button Content="PanRight" ToolTip="PanRight" 
        Name="PanRightCommand" 
        Command="{Binding PanRightCommand, ElementName=MariaCtrl}" />
<Button Content="PanUp" ToolTip="PanUp" 
        Name="PanUpCommand" 
        Command="{Binding PanUpCommand, ElementName=MariaCtrl}" />
<Button Content="PanDown" ToolTip="PanDown" 
        Name="PanDownCommand"
        Command="{Binding PanDownCommand, ElementName=MariaCtrl}" />
```

Add the following navigation properties to your map interface class (*MapViewModel*)

```csharp
public ICommand NavigateBackwardCommand
{
    get { return _mapLayer.NavigateBackwardCommand; }
}
public ICommand NavigateForwardCommand
{
    get { return _mapLayer.NavigateForwardCommand; }
}
```

Then, add pushbuttons (Navigate Backward and Navigate Forward) for stepping between the previous views.

```xml
<Button Content="Navigate Backward" ToolTip="Navigate Backward" 
        Name="NavigateBackwardCommand" 
        Command="{Binding MapViewModel.NavigateBackwardCommand}" />
<Button Content="Navigate Forward" ToolTip="Navigate Forward" 
        Name="NavigateForwardCommand"                     
        Command="{Binding MapViewModel.NavigateForwardCommand}" />
```

You should now be able to navigate the map by selecting your own buttons.<br>
Observe that the forward/backward navigation buttons are disabled until you have panned or zoomed to have views to go to.

{% include image.html file="clientapi/mapinteraction/mapareanavigation.png" caption="Map area navigation" %}

## Changing map source {#mapsourcechange}

When several map templates are available, you can include selection of map to use by adding the following properties to *MapViewModel*

```csharp
public ReadOnlyObservableCollection<string> ActiveMapNames
{
    get { return _mapLayer.ActiveMapNames; }
}
public string ActiveMapName
{
    get { return _mapLayer.ActiveMapName; }
    set
    {
        _mapLayer.ActiveMapName = value;
        MiniMapLayer.ActiveMapName = value;
        
        NotifyPropertyChanged(() => ActiveMapName);
    }
}
```

Then add a combo box to your main window

```xml
 <ComboBox Name="cmbActiveMap" 
           ItemsSource="{Binding MapViewModel.ActiveMapNames}" 
           SelectedItem="{Binding MapViewModel.ActiveMapName}"
           IsSynchronizedWithCurrentItem="true"
           Width="Auto" />
```

You should now be able to switch between available map templates.
Please note that the map templates above are displayed in different scales.

{% include image.html file="clientapi/mapinteraction/changingmapsource.png" caption="Changing map source" %}

##  Selecting map sub layers
The map templates may be defined with several sub-layers, e.g. the map information itself, and additional elevation shading.

You can control the sub layer display through the map layer, ***IMariaMapLayer.MapDataLayers*** property.

To select sub-layers to be displayed, first add a sub layers property to your view model:

```csharp
public ObservableCollection<IRasterLayerData> MapSubLayersDisplay
{
    get { return new ObservableCollection<IRasterLayerData>(_mapLayer.MapDataLayers); }
}
```

Then, add a list box with check items your XAML:

```xml
<ListBox Name="lstSubLayers" Height="Auto" Margin="2" MinWidth="40"
         ItemsSource="{Binding MapViewModel.MapSubLayersDisplay}" >
    <ListBox.ItemTemplate>
        <DataTemplate>
            <CheckBox Width="Auto" 
                      Content="{Binding Path=Name}" 
                      IsChecked="{Binding Path=Visible, Mode=TwoWay}" />
        </DataTemplate>
    </ListBox.ItemTemplate>
</ListBox>
```

To ensure that the sub layer display is updated when a new map source is selected, add the following line to the active map name setter:

```csharp
public string ActiveMapName
{
    . . .
    set
    {
        . . .
        NotifyPropertyChanged(() => MapSubLayersDisplay);
    }
}
```

You should now be able to select between different sub layers in your map templates.

{% include image.html file="clientapi/mapinteraction/changingmapsublayers.png" caption="Selecting map sub layers" %}

##  Adding Web Maps

* For this part you will need to add the ***TPG.GeoFramemwork.WebMaps.WrapperClient*** NuGet package.
* You also need to have a Web-Map Service running, available through your Catalog service.

Add the following code to your Map View Model (MapViewModel.cs):

```csharp
public MapViewModel(IMariaMapLayer mapLayer)
{
    . . .
    var webMapsClientFactory = new WebMapsClientFactory();
    var webMapsClientManager = new WebMapsClientManager(mapLayer.MapServiceManager, mapLayer.MapResources);

    var os = webMapsClientFactory.CreateOpenStreetClient("Open Street");
    webMapsClientManager.AddClient(os);
    os.Update();
    
    var ve = webMapsClientFactory.CreateVirtualEarthClient("Virtual Earth");
    webMapsClientManager.AddClient(ve);
    ve.Update();
}
```

Set ActiveMapName = "No Template" in OnMapLayerInitialized.

“Virtual Earth” and “Open Street” sub-layers should now be available for all map templates -- including no template.

{% include image.html file="clientapi/mapinteraction/webmaplayers.png" caption="Web Map Layers" %}

##  Bookmarks

Bookmarks are shortcuts to specific map sections, specified by map scale and center position. Predefined may be available from the map service for selected map sources. Available bookmarks are listed in the map layer ***Bookmarks*** property, and activated by setting the ***ActiveBookmark*** property.

In the MapLayerViewModel, create properties for bookmark interaction:

```csharp
public Bookmark ActiveBookmark { set { _mapLayer.ActiveBookmark = value; } }
public ObservableCollection<Bookmark> Bookmarks { get { return _mapLayer.Bookmarks ; } }
```

Then, add a list box with bookmark items for display and selection of bookmarks:

```xml
<ListBox Name="lstBookmarks" Height="Auto" Margin="2" 
     ItemsSource="{Binding MapViewModel.Bookmarks}" 
     SelectedItem="{Binding MapViewModel.ActiveBookmark, Mode=OneWayToSource}">
    <ListBox.ItemTemplate>
        <DataTemplate>
            <Label Width="Auto" 
               Content="{Binding Path=Name}" />
        </DataTemplate>
    </ListBox.ItemTemplate>
</ListBox>
```

You should now be able to display different map sections by selecting different bookmarks.

{% include image.html file="clientapi/mapinteraction/bookmarknavigation.png" caption="Bookmark navigation" %}

Note that different map sources may have different bookmarks.

{% include image.html file="clientapi/mapinteraction/bookmarksdifferentsources.png" caption="Bookmarks for different map sources" %}

Bookmarks can be added or removed *locally* for the map client in runtime.

Add buttons for adding and removing bookmarks to your main window xaml:

```xml
<Button Content="Create" Command="{Binding MapViewModel.AddBookmark}"/>
<Button Content="Remove" Command="{Binding MapViewModel.RemoveBookmark}" />
```

Implement the command handlers in the view model:

```csharp
public ICommand RemoveBookmark { get { return new DelegateCommand(x => OnRemoveBookmark()); } }
private void OnRemoveBookmark()
{
    _mapLayer.Bookmarks.Remove(_mapLayer.ActiveBookmark);
}

public ICommand AddBookmark { get { return new DelegateCommand(x => OnAddBookmark()); } }
private void OnAddBookmark()
{
    _mapLayer.Bookmarks.Add(new Bookmark
                {
                    Name = "Bookmark-" + _cnt++,
                    Pos = new Tuple<double, double>(CenterPosition.Lat, CenterPosition.Lon),
                    Scale = CenterScale,
                    MapSig = ActiveMapName
                });
}   
```

You should now be able to add and remove your own bookmarks!

{% include image.html file="clientapi/mapinteraction/bookmarksmanagement.png" caption="Adding and removing bookmarks" %}

{% include links.html %}