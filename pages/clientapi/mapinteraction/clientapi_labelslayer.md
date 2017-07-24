---
title: Labels
keywords: 
tags: 
sidebar: clientapi_sidebar
permalink: clientapi_labelslayer.html
summary: This section describes how to modify label properties.
---

OBSOLETE???? 

The labels layer provides display of dynamic layer information.
###  Labels layer interaction

Create an view model class (//LabelsLayerViewModel.cs//) for interaction with the label layer, inheriting ViewModelBase. 

Implement public property access to the map layer, and add an event handler for the map layer *LayerInitialized*event.

```csharp
public class LabelsLayerViewModel : ViewModelBase
{
    private IMariaLabelsLayer _labelsLayer;

    public LabelsLayerViewModel(IMariaLabelsLayer labelsLayer )
    {
        _labelsLayer = labelsLayer;
        _labelsLayer.LayerInitialized += OnLabelsLayerInitialized;
    }
    private void OnLabelsLayerInitialized()  {  }
}
```

Add properties to access the labels layer visibility and opacity, and refresh the information when the layer has been initialized.

```csharp
private void OnLabelsLayerInitialized()
{
    RefreshProperties();
}

public double Opacity
{
    get { return _labelsLayer.Opacity; }
    set
    {
        _labelsLayer.Opacity = value;
        NotifyPropertyChanged(() => Opacity);
    }
}

public bool LabelsLayerVisible
{
    get { return _labelsLayer.Visible; }
    set
    {
        _labelsLayer.Visible = value;
        RefreshProperties();
    }
}

private void RefreshProperties()
{
    NotifyPropertyChanged(() => LabelsLayerVisible);
    NotifyPropertyChanged(() => Opacity);
}
```

Create the labels layer and include the *LabelsLayerViewModel* in the declarations and constructor of the main view model (//MariaWindowViewModel//).

```csharp
public LabelsLayerViewModel LabelsLayerViewModel { get; set; }
private readonly IMariaLabelsLayer _labelsLayer;
. . .
_labelsLayer = new LabelsLayer(_mapLayer);
LabelsLayerViewModel = new LabelsLayerViewModel(_labelsLayer);
Layers.Add(_labelsLayer);
```

Then, add a checkbox and a slider with bindings to the view model properties:

```xml
<Slider Grid.Row="0" Width="50"
        Name="sldLabelsOpacity" 
        Minimum="0.0" Maximum="1.0" 
        Value="{Binding LabelsLayerViewModel.Opacity }" 
        IsEnabled="{Binding LabelsLayerViewModel.LabelsLayerVisible}"
        />
```

Running your application, You should now be able to view labels when the selected map type supports it.

{% include image.html file="clientapi/mapinteraction/labels.jpg" caption="Labels at different scales and opacity" %}

{% include links.html %}