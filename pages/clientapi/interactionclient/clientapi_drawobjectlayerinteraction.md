---
title: Draw Object Interaction
keywords: 
tags: [draw object]
sidebar: clientapi_sidebar
permalink: clientapi_drawobjectlayerinteraction.html
summary: This section describes how to interact with Maria draw objects and draw object layer functionality.
---
Key information:

* The draw object layer is accessed programmatically through the [***IMariaDrawObjectLayer***](http://maria.support2.teleplan.no/MariaGDKDoc/html/376EB49A.htm) and [***IMariaExtendedDrawObjectLayer***](http://maria.support2.teleplan.no/MariaGDKDoc/html/B305C33C.htm).

* Primitives and types for accessing the draw objects are found in  [***ISimpleDrawObject***](http://maria.support2.teleplan.no/MariaGDKDoc/html/5AF7A675.htm) interface.

* For this part you will need to add the ***TPG.Maria.DrawObjectLayer*** NuGet package.

## Using draw object service or client internal draw objects
The draw objects displayed can be internal to each client, or connected to a draw object service.<br>
**In the following example we will use client internal draw objects!**

<div class="alert alert-info" role="alert"><i class="fa fa-exclamation-circle"></i> <b> Note:</b><br> 
When connected to a draw object service, creating, updating and removing draw objects is performed towards the connected service, while selecting draw objects is performed locally on your client.
</div>

Example of draw object layer connected to a draw object service is found in the sample project [Geo Fencing Client](clientapi_geofencing.html)

##  Creating the draw object layer {#createdrawobjectlayer}

Create an view model class, *DrawObjectViewModel*, for the draw object layer, inheriting ViewModelBase, implementing the initialization event handler and a “DrawObjectLayer” field for access to the Draw Object Layer.

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

##  Creating draw objects programmatically {#createdrawobjectsprogramatically}

Use the ***DrawObjectFactory*** property of the draw object layer interface to create your objects; ellipses, poly lines, poly areas (including triangles, rectangles) text objects, tactical objects etc.! The returned objects are subtypes of ***ISimpleDrawObject*** and available in ***SimpleDrawObjectAPI Primitives.***

After updating the object properties, use the ***UpdateStore*** method to add the objects to the draw object layer.

Here are two examples of how to generate objects at random positions within the diaplayed map area (using the *RandomProvider* class described for [track creation](clientapi_tracklayerinteraction.html#createtracks)):

```csharp
private void AddPolyLine()
{
    var rect = _drawObjectLayer.GeoContext.Viewport.GeoRect;
    ISimpleDrawObject obj = _drawObjectLayer.DrawObjectFactory.CreatePolyLine();
    obj.Points = new[]
    {
        RandomProvider.GetRandomGeoPoint(rect),
        RandomProvider.GetRandomGeoPoint(rect),
        RandomProvider.GetRandomGeoPoint(rect)
    };

    obj.DataFields.Name = "PolyLineObject-" + DateTime.UtcNow.ToString("yyyy-MM-dd HH:mm:ss");
    obj.DataFields.DrawDepth = 4;
    obj.DataFields.RotationAngle = 0;

    obj.GenericDataFields.LineColor = Colors.Brown;
    obj.GenericDataFields.LineWidth = 4;
    obj.GenericDataFields.LineDashStyle = null; // DashStyles.Solid
    _drawObjectLayer.UpdateStore(obj);
}

private void AddEllipse()
{
    var rect = _drawObjectLayer.GeoContext.Viewport.GeoRect;
    rect.Center = RandomProvider.GetRandomPosition(rect);
    rect.InflateRelative(0.1);

    var obj = _drawObjectLayer.DrawObjectFactory.CreateEllipse();

    if (obj.GetType().IsSubclassOf(typeof(IEllipse)))
        return;

    var ellipse = (IEllipse)obj;
    ellipse.CentrePoint = RandomProvider.GetRandomGeoPoint(rect);
    ellipse.FirstConjugateDiameterPoint = RandomProvider.GetRandomGeoPoint(rect);
    ellipse.SecondConjugateDiameterPoint = RandomProvider.GetRandomGeoPoint(rect);

    obj.DataFields.Name = "EllipsisObject-" + DateTime.UtcNow.ToString("yyyy-MM-dd HH:mm:ss");
    obj.DataFields.DrawDepth = 4;
    obj.DataFields.RotationAngle = 0;

    obj.GenericDataFields.LineColor = Colors.DarkOrange;
    obj.GenericDataFields.LineWidth = 4;
    obj.GenericDataFields.LineDashStyle = new List<double>();
    obj.GenericDataFields.Fill = true;
    obj.GenericDataFields.FillStyle = FillStyle.Cross;
    obj.GenericDataFields.FillBackgroundColor = Colors.Gold;
    obj.GenericDataFields.FillForegroundColor = Colors.Yellow;

    _drawObjectLayer.UpdateStore(obj);
}
```

In the sample project you will fine additional examples. The objects are created, one at the time, when pressing the *Add Object* button.

{% include image.html file="clientapi/interactionclient/drawobjcreate.png" caption="Programatically created draw objects" %}

##  Draw objects enquiries

The extended draw object layer interface contains various methods and properties for making draw object enquiries.

Examples:

| ***ItemDrawObjectIds*** | Get instance IDs of all objects in the layer.|
| ***GetVisibleObjectCount()*** | Get number of visible objects.|
| ***GetVisibleObjectIds()*** | Get instance IDs of visible objects.|
| ***GetClickedDrawObjects(point)*** | Get instance IDs for pbjects at speciffic point.|
| ***SelectedDrawObjectIds*** | Get instance IDs of all selected objects.|

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

{% include image.html file="clientapi/interactionclient/drawobjselect.png" caption="Selecting draw objects" %}

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
        obj.GenericDataFields.LineDashStyle = new List<double>(); // DashStyles.Solid
        obj.GenericDataFields.Fill = true;
        obj.GenericDataFields.FillStyle = FillStyle.Solid;
        obj.GenericDataFields.FillForegroundColor = Colors.Gold;
    }

    _drawObjectLayer.UpdateStore(obj);
```

Object changes can also be monitored by implementing the ***LayerChanged*** delegate -- also in the extended draw object layer interface.

The sample project contains example code for programmatically updating selected objects (***ModifyDrawObjects***), and monitoring object changes.

{% include image.html file="clientapi/interactionclient/drawobjupdate.png" caption="Updating draw objects" %}

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

{% include image.html file="clientapi/interactionclient/drawobjmapcreate.png" caption="Creating draw objects in map" %}

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

{% include image.html file="clientapi/interactionclient/drawobjedit.png" caption="Edit draw objects" %}

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

{% include image.html file="clientapi/interactionclient/drawobjremove.png" caption=" Removing draw objects" %}

{% include links.html %}