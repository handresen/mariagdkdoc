## Geo Shape Information

The draw objects we are working with should belong to the **//MariaGeoFence.Sample//** draw object store, from a local draw object service.

### Draw Object preparation

Create draw layer and *DrawObjectViewModel* for client internal draw object layer, as described in [Draw Object Layer](maria_gdk/programming/getting_started/mariamapinteractionclient/drawobjectlayerinteraction).

Then, in *DrawObjectViewModel*, implement an event handler for the ServiceConnected event in addition to the LayerInitialized event. Add service connection when the layer is initialized, and creation/selection of draw object store when the service is connected. 

The constructor and event handlers will be looking something like this:

```csharp
public DrawObjectViewModel(IMariaDrawObjectLayer drawObjectLayer, MariaWindowViewModel parent)
{
    _parent = parent;

    _drawObjectLayer = drawObjectLayer;
    _drawObjectLayer.LayerInitialized += DrawObjectLayerOnLayerInitialized;
    _drawObjectLayer.ServiceConnected += OnDrawObjectLayerServiceConnected;

    _drawObjectLayer.ExtendedDrawObjectLayer.ActiveCreationWorkflowCompleted += 
      OnExtendedDrawObjectLayerActiveCreationWorkflowCompleted;
}
private void DrawObjectLayerOnLayerInitialized()
{
    _drawObjectLayer.DefaultStyleXml = "`<styleset>``<stylecategory name=\"DrawObjects\"/>``</styleset>`";

    DrawObjectServices = new ObservableCollection`<IMariaService>`
    {
        new MariaService("DrawObjectService")
    };

    _parent.DisplayFilters.Add(_drawObjectLayer.DisplayFilter);

    if (_drawObjectLayer != null &&
        _drawObjectLayer.GenericCreationWorkflows != null &&
        _drawObjectLayer.GenericCreationWorkflows.Count > 0)
    {
        foreach (var gwf in _drawObjectLayer.GenericCreationWorkflows.Reverse())
            _drawObjectLayer.CreationWorkflows.Insert(0, gwf);
    }
}

private void OnDrawObjectLayerServiceConnected(object sender, MariaServiceEventArgs args)
{
    if (DrawObjectServices.Count > 0)
    {
        var activeName = _parent.StoreId;
        ActiveDrawObjectService = DrawObjectServices[0];

        DrawObjectServiceStores =
            new ObservableCollection`<string>`(_drawObjectLayer.GetDrawObjectServiceStores());


        if (!DrawObjectServiceStores.Contains(activeName))
        {
            DrawObjectServiceStores.Add(activeName);
        }

        ActiveDrawObjectServiceStore = activeName;
    }
}
```

Make sure that your *App.config* file contains an end point specification for the Draw Object Service. See [Service Configuration.](maria_gdk/programming/getting_started/mariabasicmapclient/service_configuration)

Then, define and instansiate the DrawObjectViewModel in the declarations and constructor of the main view model (MariaWindowViewModel).

```csharp
public DrawObjectViewModel DrawObjectViewModel { get; set; }
. . .
public MariaWindowViewModel()
{
    . . .
    DrawObjectViewModel = new DrawObjectViewModel();
    . . .
}
```


### Draw Object Management

Add the following to your Application window:

 | Function                                                                                                                                                                                                                                                                                                                    | GUI element | Description | 
 | --------                                                                                                                                                                                                                                                                                                                    | ----------- | ----------- | 
 | Add | Button | Add draw object, e.g. at random position within the screen area ((See [Draw Object Layer->Create Objects Programmatically](maria_gdk/programming/getting_started/mariamapinteractionclient/drawobjectlayerinteraction#create_objects_programmatically), or  MariaGeoFencing sample project for details.)). |
 | Pt.Edit | Button | Point edit mode for selected draw object((See [Draw Object Layer->Edit Points Mode](maria_gdk/programming/getting_started/mariamapinteractionclient/drawobjectlayerinteraction#edit_points_mode), or  MariaGeoFencing sample project for details.)). |                                                  
 | Remove | Button | Remove selected draw objects((See [Draw Object Layer->Remove Objects](maria_gdk/programming/getting_started/mariamapinteractionclient/drawobjectlayerinteraction#remove_objects), or  MariaGeoFencing sample project for details.)). |                                                                   

{{:maria_gdk:programming:getting_started:mariageofencing:geofencedrawobjectmngt.png?direct&300|Draw Object Management}}
