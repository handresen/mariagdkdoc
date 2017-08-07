---
title: Layer Visibility
keywords: 
tags: 
sidebar: clientapi_sidebar
permalink: clientapi_layervisibility.html
summary: This section describes how to alter layer visibility
---

The visibility of each layer is controlled individually through each layers Visibility property, ***IMariaInternalLayer.Visible***.

To select layers to be displayed, first add a layers property to the main view model (*MariaWindowViewModel*):

```csharp
public ObservableCollection<IMariaLayer> LayersDisplay
{
    get { return new ObservableCollection<IMariaLayer>(Layers);}
}
```

Then, add a list box with check items your XAML:

```xml
<ListBox Name="lstLayers" Height="Auto" Margin="2" 
             ItemsSource="{Binding LayersDisplay}" >
    <ListBox.ItemTemplate>
        <DataTemplate>
            <CheckBox Width="Auto" 
                      Content="{Binding Path=Name}" 
                      IsChecked="{Binding Path=Visible, Mode=TwoWay}" >
            </CheckBox>
        </DataTemplate>
    </ListBox.ItemTemplate>
</ListBox>
```

Running your application, you should be able to turn the different layers on/off !

{% include image.html file="clientapi/interactionclient/layervisibility.png" caption="Layer visibility" %}

{% include links.html %}