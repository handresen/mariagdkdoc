---
title: Draw Object Interaction
keywords: 
tags: [draw object]
sidebar: clientapi_sidebar
permalink: clientapi_drawobjectlayerinteraction.html
summary: This section describes how to interact with Maria draw object layer functionality.
---
##  Draw Object Layer preparations

We will now perform interactions towards the draw object layer.

* For this part you will need to add the ***TPG.Maria.DrawObjectLayer*** NuGet package.
* The draw layer can be internal to each client, or connected to a draw object service.<br>
  **In this example we will use internal draw layer!**

Please note that when connected to a draw object service, creating, updating and removing draw objects is performed towards the connected service, while selecting draw objects is performed locally on your client.

Example of draw object layer connected to a draw object service, see sample project 
[Geo Fencing Client](clientapi_geofencing.html)

Primitives and types for draw accessing the draw object layer are found in ***TPG.DrawObjects.Contracts.SimpleDrawObjectAPI***.

The layer is accessed programmatically through the draw object layer interfaces:

 | Interface                                                                          | Accesed through | 
 | ---------                                                                          | --------------- | 
 | *IMariaDrawObjectLayer*     | *_drawObjectLayer.DrawObjectLayer* |            
 | *IMariaExtendedDrawObjectLayer* | *_drawObjectLayer.ExtendedDrawObjectLayer* |

###  Create draw object layer {#createdrawobjectlayer}

Create an view model class (*DrawObjectViewModel*) for the draw object layer, inheriting ViewModelBase, implementing the initialization event handler and a “DrawObjectLayer” field for access to the Draw Object Layer.

```csharp
public class DrawObjectViewModel
{
   private readonly IMariaDrawObjectLayer _drawObjectLayer;

   public DrawObjectViewModel(IMariaDrawObjectLayer drawObjectLayer)
   {
            _drawObjectLayer = drawObjectLayer;
            _drawObjectLayer.LayerInitialized += DrawObjectLayerOnLayerInitialized;
   }
   private void DrawObjectLayerOnLayerInitialized()
   {
        // Draw layer initialization goes here...
   }
}
```

 Then, create the draw object layer and include the DrawObjectViewModel in the declarations and constructor of the main view model (*MariaWindowViewModel*).

```csharp
public DrawObjectViewModel DrawObjectViewModel { get; set; }
        private readonly IMariaDrawObjectLayer _drawObjectLayer;
. . .
_drawObjectLayer = new DrawObjectLayer(new DefaultDrawObjectTypeDefinitionProvider(),
                                       useDrawObjectService: false)
{
    InitializeCreationWorkflows = true
};

DrawObjectViewModel = new DrawObjectViewModel(_drawObjectLayer);
Layers.Add(_drawObjectLayer);
```

##  Create draw objects programmatically {#createdrawobjectsprogramatically}

Use the ***DrawObjectFactory*** property of the draw object layer interface to create your objects; ellipses, poly lines, poly areas (including triangles, rectangles) text objects, tactical objects etc.! The returned objects are subtypes of ***ISimpleDrawObject*** and available in ***SimpleDrawObjectAPI Primitives.***

After updating the object properties, use the ***UpdateStore*** method to add the objects to the draw object layer.

Example:

```csharp
var obj = _drawObjectLayer.DrawObjectFactory.CreateFanArea();

if (obj.GetType().IsSubclassOf(typeof(IFanArea)))
    return;

var fan = (IFanArea)obj;
fan.VertexPoint = new GeoPoint { Latitude = 60.0, Longitude = 11.0 };
fan.MaximumRange = 20000;
fan.MinimumRange = 5000;

obj.DataFields.Name = "Fan Object";
obj.DataFields.DrawDepth = 4;
obj.DataFields.RotationAngle = 0;
obj.DataFields.ExtraFields.Add("MyTag", "MyTagValue");

obj.GenericDataFields.LineColor = Colors.Green;
obj.GenericDataFields.LineWidth = 4;
obj.GenericDataFields.LineDashStyle = new List`<double>`(); // DashStyles.Solid

obj.GenericDataFields.Fill = true;
obj.GenericDataFields.FillStyle = FillStyle.Solid;
obj.GenericDataFields.FillForegroundColor = Colors.LawnGreen;

_drawObjectLayer.UpdateStore(obj);
```

The sample project contains example code for programmatically adding objects (***AddSimpleDrawObjects***) in the startup sequence, as soon as the draw layer is ready for interaction (from *DrawObjectLayerOnLayerInitialized*).

{% include image.html file="clientapi/mapinteraction/drawobjcreate.png" caption="Programatically created draw objects" %}

##  Draw objects enquiries

The extended draw object layer interface contains various methods and properties for making draw object enquiries.

Example:

*  ***ItemDrawObjectIds***\\ Get instance IDs of all objects in the layer.

*  ***GetVisibleObjectCount()***\\ Get number of visible objects.

*  ***GetVisibleObjectIds()***\\ Get instance IDs of visible objects.

*  ***GetClickedDrawObjects(point);***\\ Get instance IDs at clicked point.

*  ***SelectedDrawObjectIds*** \\ Get instance IDs of all selected objects.

The sample project contains examples on how these are used.

##  Draw layer object selection

The Maria Control supports selection of draw layer objects by user action in the map area.

In addition, setting and getting selection state of objects can be done programmatically through the extended draw object layer interface.

Example:

```csharp
if (_drawObjectLayer.ExtendedDrawObjectLayer.IsSelected(objId))
    _drawObjectLayer.ExtendedDrawObjectLayer.DeSelect(objId);
else
    _drawObjectLayer.ExtendedDrawObjectLayer.Select(objId);
```

 Selections can also be monitored by implementing the ***LayerSelectionChanged*** delegate -- also in the extended draw object layer interface.

The sample project contains selection example code, toggling selection status for all visible objects, and monitoring selection status changes.

{% include image.html file="clientapi/mapinteraction/drawobjselect.png" caption="Selecting draw objects" %}

##  Updating draw objects

Draw objects information can be obtained with the draw object layer interface ***GetDrawObjectFromStore*** method, and updated by the ***UpdateStore*** method.

Example:

```csharp
var obj = _drawObjectLayer.GetDrawObjectFromStore(id);

_drawObjectLayer.UpdateStore(obj);

obj.GenericDataFields.DrawOutline = true;

if (obj.GenericDataFields.Fill)
    {
        obj.GenericDataFields.Fill = false;
        obj.GenericDataFields.LineColor = Colors.Magenta;
        obj.GenericDataFields.LineWidth = 6;
    }
    else
    {
        obj.GenericDataFields.Fill = true;
        obj.GenericDataFields.LineColor = Colors.DarkGoldenrod;
        obj.GenericDataFields.LineWidth = 3;
        obj.GenericDataFields.LineDashStyle = new List`<double>`(); // DashStyles.Solid
        obj.GenericDataFields.Fill = true;
        obj.GenericDataFields.FillStyle = FillStyle.Solid;
        obj.GenericDataFields.FillForegroundColor = Colors.Gold;
    }

    _drawObjectLayer.UpdateStore(obj);
```

Object changes can also be monitored by implementing the ***LayerChanged*** delegate -- also in the extended draw object layer interface.

The sample project contains example code for programmatically updating selected objects (***ModifyDrawObjects***), and monitoring object changes.

{% include image.html file="clientapi/mapinteraction/drawobjupdate.png" caption="Updating draw objects" %}

##  User created draw objects in map (draw object tools) {#drawobjecttools}

While the default tool (see [Tools](clientapi_toolsinteraction.html)) is active, Draw object tools (Creation Workflows) can be activated through the ***ActivateCreationWorkflow*** method of the extended layer interface.

Example:

```csharp
_drawObjectLayer.ExtendedDrawObjectLayer.ActivateCreationWorkflow(
                       GenericObjectTypeIds.FanArea);
```

After the creation, the objects will have default styling. Completion of the creation workflow can be detected by implementing the ***ActiveCreationWorkflowCompleted*** delegate of the extended draw object layer.

Example:

```csharp
public DrawObjectViewModel(IMariaDrawObjectLayer drawObjectLayer)
{
    . . .    
    _drawObjectLayer.ExtendedDrawObjectLayer.ActiveCreationWorkflowCompleted 
        += OnActiveCreationWorkflowCompleted;
}
. . .
private void OnActiveCreationWorkflowCompleted(object sender, 
                                            ActiveWorkflowCompletedEventArgs args)
{    
    var extendedDrawObjectLayer = (IMariaExtendedDrawObjectLayer) sender;
    foreach (var id in extendedDrawObjectLayer.SelectedDrawObjectIds)
    {
        if (args.DrawObjectTypeId == GenericObjectTypeIds.FanArea)
        { . . . }
        . . . 
    }
}
```

##  Updating draw objects

The sample project contains code to activate user created draw objects of different types.

{% include image.html file="clientapi/mapinteraction/drawobjmapcreate.png" caption="Creating draw objects in map" %}

##  Edit points mode {#editpoints}

Edit point mode can be set/reset by activating ***EditPointsCommand*** (applies to single selected draw layer object) in the draw object layer interface -- or by use of the ***EditPoints***and ***EndEditPoints*** methods in the extended draw object layer interface.

Examples:

```csharp
if (string.IsNullOrEmpty(_drawObjectLayer.ExtendedDrawObjectLayer.EditPointsInstanceId))
{
     _drawObjectLayer.ExtendedDrawObjectLayer.EditPoints(id);
}
else
{
    var id = _drawObjectLayer.ExtendedDrawObjectLayer.EditPointsInstanceId;

    _drawObjectLayer.ExtendedDrawObjectLayer.EndEditPoints();
    _drawObjectLayer.ExtendedDrawObjectLayer.Select(id);
}
```

The sample project contains code to activate draw object “edit points mode” by edit points command and EditPoints/EndEditPoint for specific draw objects.

{% include image.html file="clientapi/mapinteraction/drawobjedit.png" caption="Edit draw objects" %}

##  Remove draw objects {#removeobject}

Draw layer objects can be removed by activating ***DeleteDrawObjectCommand*** in thedraw object layer interface -- or by use of the ***Delete*** and ***DeleteSelected***methods in the extended draw object layer interface.

Examples:

```csharp
_drawObjectLayer.Delete(ids);
```

Alternatively:

```csharp
_drawObjectLayer.ExtendedDrawObjectLayer.DeleteSelected();
```

The sample project contains code to activate removal of objects by delete command and removing specific draw objects.

{% include image.html file="clientapi/mapinteraction/drawobjremove.png" caption=" Removing draw objects" %}

{% include links.html %}