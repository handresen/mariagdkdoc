#  Draw Object Layer

We will now perform interactions towards the draw object layer.

`<WRAP center round info>`
For this part you will need to add the **//TPG.Maria.DrawObjectLayer//** package.
`</WRAP>`

The draw layer can be internal to each client, or connected to a draw object service. **In this example we will use internal draw layer!**

`<WRAP center round important>`
Please note that when connected to a draw object service, creating, updating and removing draw objects is performed towards the connected service, while selecting draw objects is performed locally on your client.
`</WRAP>`

Example of draw object layer connected to a draw object service, see sample project 
[Maria Geo Fencing](maria_gdk/programming/getting_started/mariageofencing)

Primitives and types for draw accessing the draw object layer are found in ***TPG.DrawObjects.Contracts.SimpleDrawObjectAPI.***

The layer is accessed programmatically through the draw object layer interfaces:
 | Interface                                                                          | Accesed through | 
 | ---------                                                                          | --------------- | 
 | *IMariaDrawObjectLayer*     | *_drawObjectLayer.DrawObjectLayer* |            
 | *IMariaExtendedDrawObjectLayer* | *_drawObjectLayer.ExtendedDrawObjectLayer* |



For details on how to administer the connection to the draw object service, please see [Utilizing global draw object information](maria_gdk/programming&#utilizing_global_draw_object_information).
###  Creating the draw object layer

Create an interface class (//DrawObjectViewModel//) for the draw object layer, inheriting ViewModelBase, implementing the initialization event handler and a “DrawObjectLayer” field for access to the Draw Object Layer.

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

 Then, create the draw object layer and include the DrawObjectViewModel in the declarations and constructor of the main view model (//MariaWindowViewModel//).

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

###  Create objects programmatically

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

The sample project contains example code for programmatically adding objects (//**AddSimpleDrawObjects**//) in the startup sequence, as soon as the draw layer is ready for interaction (from *DrawObjectLayerOnLayerInitialized*).

{{maria_gdk:programming:maria_2012_tutorial_html_m2a6781c2.png|Create draw objects programatically}}\\ Figure: Create draw objects programatically.

###  Draw Layer Objects enquiries

The extended draw object layer interface contains various methods and properties for making draw object enquiries.

Example:

*  ***ItemDrawObjectIds***\\ Get instance IDs of all objects in the layer.

*  ***GetVisibleObjectCount()***\\ Get number of visible objects.

*  ***GetVisibleObjectIds()***\\ Get instance IDs of visible objects.

*  ***GetClickedDrawObjects(point);***\\ Get instance IDs at clicked point.

*  ***SelectedDrawObjectIds*** \\ Get instance IDs of all selected objects.

The sample project contains examples on how these are used.
###  Draw layer object selection

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

{{maria_gdk:programming:maria_2012_tutorial_html_6382f737.png|Selecting draw objects programmatically}} \\ Figure: Selecting draw objects programmatically.

###  Updating objects

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

The sample project contains example code for programmatically updating selected objects (//**ModifyDrawObjects**//), and monitoring object changes.

{{maria_gdk:programming:maria_2012_tutorial_html_m5a1c4395.png|Updating objects}}\\ Figure: Updating objects.

###  User created objects in map, draw object tools

While default tool (see [Tools](maria_gdk/programming/getting_started/client_interaction&#tools)) is active, Draw object tools (Creation Workflows) can be activated through the ***ActivateCreationWorkflow*** method of the extended layer interface.

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

This is the appropriate time to update the object attributes to add styling and add custom values. See: .
###  Updating objects

The sample project contains code to activate user created objects of different types.

{{maria_gdk:programming:maria_2012_tutorial_html_m68f5c2ae.png|Create objects in map area}} \\ Figure: Create objects in map area.
###  Edit points mode

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

{{maria_gdk:programming:maria_2012_tutorial_html_m72b45e06.png|Edit object points}} \\ Figure: Edit object points.

###  Remove objects

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

{{maria_gdk:programming:maria_2012_tutorial_html_m53dddec6.png|Removing objects}} \\ Figure: Removing objects.
