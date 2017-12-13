---
title: Grid
keywords: 
tags: [grid]
sidebar: clientapi_sidebar
permalink: clientapi_gridlayer.html
summary: This section describes how to add grids to your map application. 
---

## Creating the grid layer

* For this part you will need ***TPG.Maria.GridLayer*** NuGet package as a minimum.
* The grid layer is accessed through the grid layer interface, ***IMariaGridLayer*** (*_gridLayer*).

Create an view model class (*GridViewModel*) for the grid layer, inheriting ViewModelBase, implementing the initialization event handler and a “Grids” field for access to the Grid Layer.

```csharp
public class GridViewModel  : ViewModelBase
{
   private readonly IMariaGridLayer _gridLayer;

   public GridViewModel(IMariaGridLayer gridLayer)
   {
       _gridLayer = gridLayer;
       _gridLayer.LayerInitialized += GridLayerOnLayerInitialized;
   }
   private void GridLayerOnLayerInitialized()
   {
       NotifyPropertyChanged(() => Grids);
   }
   public ObservableCollection`<IGridRenderer>` Grids
   {
      get { return _gridLayer.Grids; }
      set { _gridLayer.Grids = value; }
   }
}

```
Then, create the grid layer and include the GridViewModel in the declarations and constructor of the main view model (*MariaWindowViewModel*).

```csharp
public GridViewModel  GridViewModel  { get; set; }
private readonly IMariaGridLayer _gridLayer;
. . .
_gridLayer = new TPG.Maria.GridLayer.GridLayer();
GridViewModel = new GridViewModel(_gridLayer);
Layers.Add(_gridLayer);
```

## Changing grid display

To change the grid settings, add a pushbutton to your main window -- and bind it to the *GridViewModel.*

```xml
`<Button Content="Switch Grid!" Command="{Binding GridViewModel.SwitchGrid}"/>`
```
 Add command handling to the GridViewModel.

```csharp
private DelegateCommand _switchGrid;
public ICommand SwitchGrid { get { return _switchGrid; } }
 . . .
private int cnt = 0;
private void OnSwitchGrid(object a)
{
	var item = cnt % (Grids.Count + 1);
	for (var i = 0; i < Grids.Count; i++)
	{
		Grids[i].Visible = i == item;
	}
	cnt++;
}
```
… and set the delegate in the constructor.

```csharp
_switchGrid = new DelegateCommand(OnSwitchGrid);
```

You should now be able to step through the different grid types. 

* This is a very simple example. In a real application you would probably use a drop down button or another control allowing multiple selection of grid types.

{% include image.html file="clientapi/interactionclient/mapgrid.png" caption=" Map area with grids" %}

{% include links.html %}