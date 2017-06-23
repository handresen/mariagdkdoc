## Maria Windows-Forms Client

{{:maria_gdk:programming:maria_2012_tutorial_html_m37b3adc4.png?direct&150 | Maria Windows-Forms Client}}
The Maria 2012 GDK is design to work with WPF applications, encouraging the MVVM development pattern. \\
If desired, it is also possible to combine with Windows Forms applications, as will be shown in this example.

`<WRAP center round info >`

*  Make sure that all **[prerequisites](maria_gdk/programming/prerequisites)** are met!

*  You will need to include the **//TPG.Maria.MapLayer//** NuGet package as a minimum.
`</WRAP>`

`<WRAP center round tip >`
Sample code for this section is found in the **//MariaWindowsFormsClient//** solution folder of the **//SampleProjects//** solution.
`</WRAP>`

###  Create the Map Window

Add a Windows Form (named *MariaWinFormsMapWindow*) to be launched from your windows forms application. In this example Maria Map windows are launched by pressing a button in the main window.

{{..:maria_2012_tutorial_html_m443f65cf.png?direct|Creating Win Forms Windows}}

From the Toolbox WPF Interoperability tab, add an *ElementHost* component to host the map component (named *_elementHostMap*).

{{..:maria_2012_tutorial_html_6bc53acc.png?direct|Add element host component}}

### Create the main view model

Create an interface class (//MariaMapComponentViewModel//) for communication with the Maria component.

```csharp
public class MariaMapComponentViewModel
{
    public MariaMapComponentViewModel()
    {
    }
}
```

###  Create the map control

 Add a WPF User control item (//MariaMapControl//), and add the MariaUserControl to the xaml of the control, optionally including properties for the integrated map controls.

```xml
<mariaUserControl:MariaUserControl  
    Name="MariaCtrl"
    IsPanNavigationVisible="True" 
    IsScaleBarVisible="True" 
    IsRulerVisible="True" 
    />
```

In the code behind (//MariaMapControl.xaml.cs//), have the class inheriting the IDisposable interface .
Set the data context of your component to be the main interface class (//MariaMapComponentViewModel//), and implement the Dispose method.

```csharp
public partial class MariaMapControl : UserControl, IDisposable
{
    public MariaMapControl()
    {
        InitializeComponent();
        DataContext = new MariaMapComponentViewModel();
    }
    public void Dispose()
    {
        MariaCtrl.Dispose();
        MariaCtrl = null;
        DataContext = null;
    }
}
```
###  Including MariaMapControl to your map window

Now, we will connect the map control (//MariaMapControl//) to the WinForms window.  

*  Add a map control field to your map window, 

*  instantiate it and hand it to the _//**elementHost**// in the constructor.

```csharp
public partial class MariaWinFormsMapWindow : Form
{
    public MariaMapComponentViewModel MainViewModel { get; private set; }
    
    public MariaMapControl MapControl { get; private set; }

    public MariaWinFormsMapWindow()
    {
        InitializeComponent();
        
        MapControl = new MariaMapControl();
        _elementHostMap.Child = MapControl;
    }
}
```


###  Handle the WindowClosing event

Implement the ***FormClosing*** event of your map window:

{{..:maria_2012_tutorial_html_79a22b83.png?direct|Implement Form Closing event}}

```csharp
private void OnFormClosing(object sender, FormClosingEventArgs e)
{
    _elementHostMap.Child = null;
   
    MapControl.Dispose();
    MapControl = null;
}
```

###  Preparing for creation of Maria layers

In the main interface class, *MariaMapComponentViewModel*, create an auto property to hold a list of Maria layers (//Layers//), and initialize the list in the constructor.

```csharp
public class MariaMapComponentViewModel
{
    public ObservableCollection`<IMariaLayer>` Layers { get; set; }

    public MariaMapComponentViewModel()
    {
        Layers = new ObservableCollection`<IMariaLayer>`();
    }
}
```

Then, bind the *Layers* property of the MariaUserControl to this list.

```xml
<mariaUserControl:MariaUserControl  
    Name="MariaCtrl"
    Layers="{Binding Layers}"
    IsPanNavigationVisible="True" 
    IsScaleBarVisible="True" 
    IsRulerVisible="True" 
    /> 
```

###  Service Configuration.

`<WRAP center round todo 60%>`
Replace with common Service Configuration item!
`</WRAP>`

Add necessary service connection configuration for connection to the map catalog service, e.g:

```xml
`<system.serviceModel>`
  `<bindings>`
    `<basicHttpBinding>`
      <binding name="myHttpBinding"
               maxBufferSize="2147483647"
               maxReceivedMessageSize="2147483647"
               closeTimeout="00:10:00"
               openTimeout="00:10:00"
               receiveTimeout="00:10:00"
               sendTimeout="00:10:00">
        `<readerQuotas maxArrayLength="2147483647"/>`
      `</binding>`
    `</basicHttpBinding>`
  `</bindings>`
  `<client>`
    <endpoint name="MapCatalogService" 
              address="http://localhost:9008" 
              contract="TPG.GeoFramework.MapServiceInterfaces.IMapCatalogService" 
              binding="basicHttpBinding" 
              bindingConfiguration="myHttpBinding" />
  `</client>`
  `<behaviors>`
    `<endpointBehaviors>`
      `<behavior name="myEndpointBehavior">`
        `<dataContractSerializer maxItemsInObjectGraph="2147483647"/>`
      `</behavior>`
    `</endpointBehaviors>`
  `</behaviors>`
  `<extensions>`
  `</extensions>`
`</system.serviceModel>`
```

###  Creating the map layer

Create an interface class (//MapViewModel//) for the map layer, and add the following variables and properties:

```csharp
private readonly IMariaMapLayer _mapLayer;
public IMariaMapLayer MiniMapLayer { get; set; }

public GeoPos CenterPosition { get; set; }
public double Scale { get; set; }
```

Add the *LayerInitialized* event handlers for the map, and optionally also for mini map:

```csharp
private string PreferredMap()
{
    const string preferred = "NorwayMap";
    if (_mapLayer.ActiveMapNames.Contains(preferred))
    {
        return preferred;
    }
    return _mapLayer.ActiveMapNames.Any() ? _mapLayer.ActiveMapNames.First() : "-";
}
private void OnMapLayerInitialized()
{
    Scale = 50000;
    CenterPosition = new GeoPos(60, 10);

    _mapLayer.ActiveMapName = PreferredMap();
}
private void OnMiniMapLayerInitialized()
{
    MiniMapLayer.ActiveMapName = PreferredMap();
}
```

Initializing the event handling in the constructor:

```csharp
public MapViewModel(IMariaMapLayer mapLayer)
{
  _mapLayer = mapLayer;
  _mapLayer.LayerInitialized += OnMapLayerInitialized;

  MiniMapLayer = new MapLayer();
  MiniMapLayer.LayerInitialized += OnMiniMapLayerInitialized;
}
```

Then, include the MapViewModel in the main interface (MariaMapComponentViewModel).

Declarations:

```csharp
public MapViewModel MapViewModel { get; set; }
private readonly IMariaMapLayer _mapLayer;
```

and in the constructor

```csharp
_mapLayer = new TPG.Maria.MapLayer.MapLayer();
MapViewModel = new MapViewModel(_mapLayer);
Layers.Add(_mapLayer);
```

Update MariaUserControl binding

```xml
<MariaUserControl:MariaUserControl Name="MariaCtrl"
                                   Layers="{Binding Layers}"
                                   IsPanNavigationVisible="True" 
                                   IsScaleBarVisible="True" 
                                   IsRulerVisible="True"
                                   IsMiniMapVisible="True"
                                   CenterScale="{Binding MapViewModel.Scale}" 
                                    CenterPosition="{Binding MapViewModel.CenterPosition}" 
                                    MiniMapLayer="{Binding MapViewModel.MiniMapLayer}"
/>
```

`<WRAP center round box>`
Running your application, the window area should now include map information and mini map -- and you should be able to navigate the map!
`</WRAP>`

{{..:maria_2012_tutorial_html_m14162176.png?direct&|Maria map in Win Forms Window}}
###  Map interaction.

Add a combo box, named ***_comboBoxMapTemplates***, a label field with the content “Cursor Position” and a read only text box named **_textCursorPos** to your map window.

Extend the Map view model with the event handling and properties for map templates and cursor position changes:

```csharp
public delegate void MapLoadedEvent(object sender);
public event MapLoadedEvent MapLoaded = delegate { };

public delegate void MouseCursorPosChangedEvent(object sender, GeoPos mousePosition);
public event MouseCursorPosChangedEvent MouseCursorChanged = delegate { };

private readonly string _cursorPositionPropertyName;
. . .
public ReadOnlyObservableCollection`<string>` ActiveMapNames
{
    get { return _mapLayer.ActiveMapNames; }
}

public string ActiveMapName
{
    get { return _mapLayer.ActiveMapName; }
    set { _mapLayer.ActiveMapName = value; }
}
. . .
public MapViewModel(IMariaMapLayer mapLayer)
{
   . . .
   // By using reflection to get the property name we avoid possible typos and
   // we will have a compile error in case the property changes name in the future.
   _cursorPositionPropertyName = GetPropertyName%%((%%) => _mapLayer.GeoContext.CursorPosition);
}

public static string GetPropertyName`<T>`(Expression`<Func<T>`> propertyExpression)
{
    var memberExpression = propertyExpression.Body as MemberExpression;

    if (memberExpression != null)
        return memberExpression.Member.Name;
        
    return "-";
}
. . .
private void OnMapLayerInitialized()
{ . . .
  MapLoaded(this);
  _mapLayer.GeoContext.PropertyChanged += HandlePropertyChanged;
}

private void HandlePropertyChanged(object sender, PropertyChangedEventArgs e)
{
    if (e.PropertyName == _cursorPositionPropertyName)
    {
        MouseCursorChanged(this, _mapLayer.GeoContext.CursorPosition);
    }
}
```

Then connect the GUI components with the view model, in *MariaWinFormsMapWindow.cs:*

```csharp
private MariaMapComponentViewModel _mainViewModel;
private MapViewModel _mapViewModel;
. . .

public MariaWinFormsMapWindow()
{
    . . .
    _mainViewModel = _mapControl.DataContext as MariaMapComponentViewModel;

    if (_mainViewModel == null)
    {
       throw new ArgumentException(string.Format("Expected {0} as view model interface, got {1}",
                                   typeof (MariaMapComponentViewModel).FullName,
                                   _mapControl.DataContext.GetType().FullName));
    }
    
    _mapViewModel = _mainViewModel.MapViewModel;
    _mapViewModel.MouseCursorChanged += HandleMouseCursorPosChanged;
    _mapViewModel.MapLoaded += HandleMapLoaded;
}

. . .

private void HandleMapLoaded(object sender)
{
    string currentTemplate = _mapViewModel.ActiveMapName;
    object selectedItem = null;
    
    foreach (string activeMapName in _mapViewModel.ActiveMapNames)
    {
        _comboBoxMapTemplates.Items.Add(activeMapName);
        
        if (activeMapName == currentTemplate)
           selectedItem = activeMapName;
    }
    
    _comboBoxMapTemplates.SelectedItem = selectedItem;
    _comboBoxMapTemplates.SelectedValueChanged += HandleMapTemplateChange;
}

private void HandleMapTemplateChange(object sender, EventArgs e)
{
    var cb = sender as ComboBox;

    if (cb == null)
        return;

    _mapViewModel.ActiveMapName = cb.SelectedItem.ToString();
}

private void HandleMouseCursorPosChanged(object sender, GeoPos mouseposition)
{
    _textCursorPos.Text = mouseposition.ToString();
}

private void OnFormClosing(object sender, FormClosingEventArgs e)//
{
    . . .

    _mainViewModel = null;
    _mapViewModel = null;
}
```

`<WRAP round box >`
Running your application, he map window will look like this, and you should be able to change map templates and see the cursor position change.
`</WRAP>`


{{..:maria_2012_tutorial_html_m37b3adc4.png?direct|Map interaction}}
