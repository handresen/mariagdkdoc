---
title: Track Visualization
keywords: 
tags: 
sidebar: clientapi_sidebar
permalink: clientapi_trackdisplay.html
summary: This section describes how to alter track visualization
---

## Track filtering

Display of tracks may be limited to tracks satisfying specific filter conditions, through the ***IMariaTrackLayer DisplayFilter*** property.

Add filter properties to your interface class (*TrackViewModel*)

```csharp
public bool FilterActive
{
    get { return (_trackLayer.DisplayFilter != null) && _trackLayer.DisplayFilter.Active; }
    set
    {
        if ((_trackLayer.DisplayFilter != null))
            _trackLayer.DisplayFilter.Active = value;
    }
}
```

To your main window xaml, add a check box for enabling/disabling the filter, and bind it to the filter property above:

```xml
<CheckBox Content="Filter" ToolTip="Filter Active" 
          Name="chkTrackDisplayFilter"                                   
          IsChecked="{Binding TrackViewModel.FilterActive}" 
          Width="60" 
/>
```

Then, create your filter, in this example a minimum speed filter an include it to the track layer when it has been initialized (during the ***OnTrackLayerInitialized*** event).

```csharp
_trackLayer.DisplayFilter.Filter = new SpeedCondition("15kts", FieldOperator.GtEq);
_trackLayer.DisplayFilter.Active = false;
_trackLayer.DisplayFilter.Name = "TrackMinimumSpeed15kts";
NotifyPropertyChanged(() => FilterActive);
```

You should now be switching between showing all the tracks and tracks wit speed > 15 knots only.

{% include image.html file="clientapi/mapinteraction/trackfiltering.png" caption="Track Filtering" %}

## Track style XML

The main mechanisms for controlling track appearance and styling is by providing track style information (track style xml). <br> 
Several aspects of track visualization can be controlled using styling. Conditional styling allows styling based on track attributes, map attributes and external settings. For details on how to create and maintain track style xml, see [Track Styling](core_styling_track.html).

The style xml is accessed through the track layer interface ***StyleXml*** property.

```csharp
public string StyleXml
{
     get { return _trackLayer.StyleXml; }
     set
     {
         _trackLayer.StyleXml = value;
         NotifyPropertyChanged(() => StyleXml);
     }
 }
```

## Track Symbol display

In addition to (combined with) the track style xml, the display is controlled by the following:

*  Symbol Scale <br> Scale factor for drawing of track symbol. Combined with symbol scale factors given by the track style XML, if any. Numeric value (double), normal range 0 -- 10.
*  Color Scheme<br> Enumeration, TPG.GeoFramework.Symbols.Contracts.SymbolColorScheme<br> Values: { Dark, Medium, Light }
*  Simplified Rendering <br> Boolean

The following is an example on how to control track symbol display:

Add display properties to your interface class (*TrackViewModel*)

```csharp
public double SymbolScale
{
    get { return _trackLayer.SymbolScale; }
    set
    {
        // Adjust to one decimal...
        var temp = (int)(value * 10);
        _trackLayer.SymbolScale = temp / 10.0;
        NotifyPropertyChanged(() => SymbolScale);
    }
}
public SymbolColorScheme ColorScheme
{
    get { return _trackLayer.SymbolColorScheme; }
    set
    {
        _trackLayer.SymbolColorScheme = value;        
        NotifyPropertyChanged(() => ColorScheme);
    }
}
public bool SimplifiedRendering
{
    get { return _trackLayer.SimplifiedRendering; }
    set
    {
        _trackLayer.SimplifiedRendering = value;
        NotifyPropertyChanged(() => SimplifiedRendering);
    }
}
```
Add data provider for the color scheme enumeration to your app.xaml:

```xml
<Application.Resources>        
    <ObjectDataProvider x:Key="trackColorSchemeEnum" 
                        MethodName="GetValues" 
                        ObjectType="{x:Type System:Enum}">
        <ObjectDataProvider.MethodParameters>
            <x:Type TypeName="SymbolsContracts:SymbolColorScheme"/>
        </ObjectDataProvider.MethodParameters>
    </ObjectDataProvider>
    . . . 
```
Then, add to your main window (xaml), a combo box for selection of color scheme, a slider and a text box for size specification, and check box for simplified rendering.

```xml
<ComboBox Name="cmbTrackColorScheme" 
          ItemsSource="{Binding Source={StaticResource colorSchemeEnum}}" 
          SelectedItem="{Binding TrackViewModel.ColorScheme}" 
          IsSynchronizedWithCurrentItem="true"
          Width="Auto" />
<Slider Grid.Row="0" 
        Name="sldTrackDisplaySize" Width="100" 
        Minimum="0.1" Maximum="10" 
        Value="{Binding TrackViewModel.SymbolScale}"/>
<TextBox Width="40" 
         TextAlignment="Right" 
         Text="{Binding Path=Value, ElementName=sldTrackDisplaySize, Mode=Default}"/>
<CheckBox Content="Simplified Rendering" ToolTip="Simple renderer" 
          Name="SimplifiedRendering" 
          IsChecked="{Binding TrackViewModel.SimplifiedRendering}" />
```

You should now be able to switch between different symbol scales, color schemes and normal/simplified rendering.

{% include image.html file="clientapi/mapinteraction/tracksymbscale.png" caption="Track Symbol Scale" %}

{% include image.html file="clientapi/mapinteraction/trackcolorscheme.png" caption="Track Color Scheme" %}

{% include image.html file="clientapi/mapinteraction/tracksimplerendering.png" caption="Simplified rendering" %}

##  Track Clustering

Track clustering is controlled by mode, algorithm and size.

*  Mode <br> Enumeration, TPG.Maria.TrackContracts.ClusterMode <br> Values: { None, Clustering, Bounds }
*  Algorithm <br> Enumeration, TPG.Maria.TrackContracts.ClusterMode.ClusteringType <br> Values: { Combined, Sequential, Grid }
*  Size <br> Numeric value (Integer).

The following is an example on how to control track clustering from your GUI.

Add cluster properties to your interface class (*TrackViewModel*)

```csharp
public bool ClusterActive
{
    get { return ClusterMode != ClusterMode.None; }
}
public ClusterMode ClusterMode
{
    get { return _trackLayer.ClusteringMode; }
    set
    {
        _trackLayer.ClusteringMode = value;
        NotifyPropertyChanged(() => ClusterMode);
        NotifyPropertyChanged(() => ClusterActive);
        NotifyPropertyChanged(() => ClusterType);
    }
}
public ClusterType ClusterType
{
    get { return _trackLayer.ClusteringType; }
    set
    {
        _trackLayer.ClusteringType = value;
        NotifyPropertyChanged(() => ClusterMode);
        NotifyPropertyChanged(() => ClusterActive);
        NotifyPropertyChanged(() => ClusterType);
    }
}
public int ClusterSize
{
    get { return _trackLayer.ClusterSize; }
    set
    {
        _trackLayer.ClusterSize = value;
        NotifyPropertyChanged(() => ClusterSize);
    }
}
```

Add data providers for the enumerations to your app.xaml:

```xml
 <Application.Resources>        
     <ObjectDataProvider x:Key="clusterAlgorithmEnum" 
                         MethodName="GetValues" 
                         ObjectType="{x:Type System:Enum}">
         <ObjectDataProvider.MethodParameters>
             <x:Type TypeName="Contracts:ClusteringType"/>
         </ObjectDataProvider.MethodParameters>
     </ObjectDataProvider>
     <ObjectDataProvider x:Key="clusterModeEnum" 
                         MethodName="GetValues" 
                         ObjectType="{x:Type System:Enum}">
         <ObjectDataProvider.MethodParameters>
             <x:Type TypeName="TrackContracts:ClusterMode"/>
         </ObjectDataProvider.MethodParameters>
     </ObjectDataProvider>
```

Then, add two combo boxes for selection of mode and algorithm and a slider and a text box for size specification, to your main window (xaml) -- binding them to your cluster properties and data providers.

```xml
<TextBox  Text="Clustering: "/>
<ComboBox Name="cmbTrackClusterMode" 
          ItemsSource="{Binding Source={StaticResource clusterModeEnum}}"
          SelectedItem="{Binding TrackViewModel.ClusterMode}" 
          IsSynchronizedWithCurrentItem="true"
          Width="Auto" />
<ComboBox Name="cmbTrackClusterAlgoritm" 
          IsEnabled="{Binding TrackViewModel.ClusterActive}"
          ItemsSource="{Binding Source={StaticResource clusterAlgorithmEnum}}" 
          SelectedItem="{Binding TrackViewModel.ClusteringType}" 
          IsSynchronizedWithCurrentItem="true"
          Width="Auto" />
<Slider Grid.Row="0" 
        Name="sldTrackClusterSize" Width="100" 
        IsEnabled="{Binding TrackViewModel.ClusterActive}"
        Minimum="0" Maximum="1000" 
        Value="{Binding TrackViewModel.ClusterSize}"/>
<TextBox Width="40" TextAlignment="Right" 
         IsEnabled="{Binding TrackViewModel.ClusterActive}"
         Text="{Binding Path=Value, ElementName=sldTrackClusterSize, Mode=Default}"/>
```

You should now be able to switch between different clustering modes, algorithms and sizes.

{% include image.html file="clientapi/mapinteraction/trackmodes.png" caption="Cluster modes and algorithms" %}

{% include image.html file="clientapi/mapinteraction/trackclustersize.png" caption="Cluster sizes" %}

##  Track history

Not yet described… 

{% include links.html %}