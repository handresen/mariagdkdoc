---
title: Elevation Analysis Client
keywords: GDK
tags: [elevationanalysis, sampleapplication, wpf]
sidebar: clientapi_sidebar
permalink: clientapi_elevationanalysis.html
summary: This section describes how to create a map client utilizing Maria elevation analysis functionality.
---
{% include image.html file="clientapi/elevationanalysis/elevationanalysisclient.png" max-width=500 caption="Elevation Profile Client" %}

Maria provides graphical display of elevation information, by utilizing ***Maria Elevation Analysis layer*** with ***elevation observer*** tools.\\

This example describes how to include use of the elevation analysis tools in your map application.

* The example is based on a map component corresponding to
[Maria Basic Map Client](clientapi_basicmapclient.html), with 
[additional map selection](clientapi_maplayerinteraction.html#mapsourcechange) and 
[tools](clientapi_toolsinteraction.html).

* You will need to include the ***TPG.Maria.ElevationAnalysis*** NuGet package (in addition to TPG.Maria.MapLayer) as a minimum.

* Sample code for this example is found in the ***MariaElevationAnalysis*** project, in ***MariaAdditionalComponents*** 
folder of the ***Sample Projects*** solution. 

##  Creating Elevation Observers

We will now perform interactions towards the elevation analysis layer 
through the elevation analysis layer interface, ***IMariaElevationAnalysisLayer***.

Create a view model class (*ElevationAnalysisViewModel.cs*) for interaction with the elevation analysis layer, 
inheriting ViewModelBase, and with the elevation analysis layer as parameters to the constructor.

Add an event handler for the elevation analysis layer *LayerInitialized* event, populating the 
layer with a “Line Of Sight Observer” and a “Max/Min Elevation Observer”.

```csharp
private IMariaElevationAnalysisLayer _elevationAnalysisLayer;

public ElevationAnalysisViewModel(IMariaElevationAnalysisLayer elevationAnalysisLayer)
{
    _elevationAnalysisLayer = elevationAnalysisLayer;
    _elevationAnalysisLayer.LayerInitialized += OnElevationAnalysisLayerInitialized;
}
private void OnElevationAnalysisLayerInitialized()
{
    var LineOfSightObserverData = new LineOfSightObserverData(Guid.NewGuid().ToString())
                                      {
                                          Position = new GeoPos(60, 9.2),
                                          Range = 20000
                                      };
    _elevationAnalysisLayer.Observers.Add(LineOfSightObserverData);

    var MaxMinElevationObserverData = new MaxMinElevationObserverData(Guid.NewGuid().ToString())
                                          {
                                              Position = new GeoPos(60, 10.2),
                                              Range = 30000
                                          };
    _elevationAnalysisLayer.Observers.Add(MaxMinElevationObserverData);
}
```

Then, create the Elevation Analysis Layerand include the *ElevationAnalysisViewModel* in the declarations 
and constructor of the main view model (*MariaWindowViewModel*).

```csharp
public ElevationAnalysisViewModel ElevationAnalysisViewModel { get; private set; }
private readonly IMariaElevationAnalysisLayer _elevationAnalysisLayer;
. . .
_elevationAnalysisLayer = new MariaElevationAnalysisLayer(_mapLayer.MapResources);
ElevationAnalysisViewModel = new ElevationAnalysisViewModel(_elevationAnalysisLayer);
Layers.Add(_elevationAnalysisLayer);
```

Running your application, you will have a map looking something like this, 
with one free sight observer and one min/max elevation observer.

{% include image.html file="clientapi/elevationanalysis/elevationobservers.png" caption="Elevation Observers" %}

**Note!**<br>
Observe that you can move the observers around by dragging the center point -- and alter the range by dragging the handle on the right side of the observer.

##  Elevation Observer tools

Add buttons for adding observer by position and by tool to your main window xaml:

```xml
<Button Name="AddFreesight" Margin="2"
    Content="Add" ToolTip="Add Freesight Observer"
    Command="{Binding ElevationAnalysisViewModel.AddFreesightObserverCommand}"
    />
<Button Name="AddFreesightTool" Margin="2"
    Content="Add tool" ToolTip="Add Freesight Observer by tool"
    Command="{Binding ElevationAnalysisViewModel.AddFreesightObserverToolCommand}"
    />
```

Add command handlers to the buttons in the elevation analysis view model:

```csharp
public ICommand AddFreesightObserverCommand
{
    get { return new DelegateCommand(x => AddLinOfSightObserver()); }
}
public ICommand AddFreesightObserverToolCommand
{
    get
    {
        return
            new DelegateCommand(x => 
               _elevationAnalysisLayer.ActivateCreationWorkflow(ObserverType.LineOfSight));
    }
}
private void AddLinOfSightObserver()
{
    var data = new LineOfSightObserverData(Guid.NewGuid().ToString())
    {
        Position = RandomProvider .GetRandomPosition(
                     _elevationAnalysisLayer.GeoContext.Viewport.GeoRect),
        Range = 20000
    };
    _elevationAnalysisLayer.Observers.Add(data);
}
```

Do the same for the Max/Min Elevation observer, and remove the creation of objects from the 
initialization method.

You will now be able to add observers at specific & random positions.

{% include image.html file="clientapi/elevationanalysis/elevationobservertool.jpg" caption="Adding observers by tool and position" %}

**Note!**<br>
The range of observers added by the observer tool will be adjusted according to the displayed map area.

##  Managing Elevation Observers

We shall now manage all the observers commonly, adding “Remove”, “Clear” and buttons, and a list view displaying all observers.

Add the buttons and a list view to your main window xaml:

```xml
<Button Name="RemoveObserverCmd" Margin="2"
        Content="Remove" ToolTip="Remove Selected Observer"
        Command="{Binding ElevationAnalysisViewModel.RemoveObserverCommand}"
        />
<Button Name="ClearObserversCmd" Margin="2"
        Content="Clear" ToolTip="Clear Observers"
        Command="{Binding ElevationAnalysisViewModel.ClearObserversCommand}"
        />
<Button Name="DeactivateCmd" Margin="2"
        Content="Deactivate" ToolTip="Deactivate Observer Tool"
        Command="{Binding ElevationAnalysisViewModel.DeactivateCommand}"
        />
. . .
<ListView Grid.Column="1" Margin="5, 5, 0, 5"  MaxHeight="150"
          Name="ObserversView" 
          ItemsSource="{Binding ElevationAnalysisViewModel.Observers}" 
          SelectedItem="{Binding ElevationAnalysisViewModel.SelectedObserver}"
          SelectionMode="Single">
    <ListView.View>
        <GridView>
            <GridViewColumn Header="Type" 
                DisplayMemberBinding="{Binding ObserverType}"/>
            <GridViewColumn Header="Pos" 
                DisplayMemberBinding="{Binding Position}"/>
            <GridViewColumn Header="Range" 
                DisplayMemberBinding="{Binding Range}"/>
        </GridView>
    </ListView.View>
</ListView>
```

Then implement command handlers and other necessary properties in the view model:

```csharp
public ICommand RemoveObserverCommand
        {
            get { return new DelegateCommand(x => OnRemoveObserverCmd()); }
        }
private void OnRemoveObserverCmd()
{
    if (IsObserverSelected)
    {
        _elevationAnalysisLayer.Observers.Remove(_selectedObserver);
    }
}
public ICommand ClearObserversCommand
        {
            get { return new DelegateCommand(x => OnClearObserversCmd()); }
        }
private void OnClearObserversCmd()
        {
            _elevationAnalysisLayer.Observers.Clear();
        }
public ObservableCollection<IObserverData> Observers
{
    get
    {
        return _elevationAnalysisLayer.Observers;
    }
}
public ICommand DeactivateCommand
{
    get { return new DelegateCommand(x => OnDeactivateCmd()); }
}
private void OnDeactivateCmd()
{
     _elevationAnalysisLayer.DeactivateCreationWorkflow();
}
private IObserverData _selectedObserver;
public IObserverData SelectedObserver
{
    get
    {
        return _selectedObserver;
    }
    set
    {
        _selectedObserver = value;

        NotifyPropertyChanged(() => SelectedObserver);
        NotifyPropertyChanged(() => IsObserverSelected);
    }
}
public bool IsObserverSelected
{
    get
    {
        return _selectedObserver != null;
    }
}
```

In the LayerInitialized event handler, add an event handler for the elevation analysis layers IMariaElevationAnalysisLayer.Observers.CollectionChanged event.

```csharp
private void OnElevationAnalysisLayerInitialized()
{
    _elevationAnalysisLayer.Observers.CollectionChanged 
                                      += OnObserversCollectionChanged;
    . . .
}
private void OnObserversCollectionChanged(object sender, 
                                          NotifyCollectionChangedEventArgs e)
{
    NotifyPropertyChanged(() => Observers);
}
```

*  You should now be able to examine your observers in the list view, remove selected observer or clear all. 
*  You should also be able to deactivate ongoing “Add tool” commands.

{% include image.html file="clientapi/elevationanalysis/elevationanalysisclient.png" caption="Observer management" %}

##  Observer properties alterations

The generic observer properties are reached through ***IObserverData*** -- Free sight observers through ***ILineOfSightObserverData***, and max/min observers through ***IMaxMinElevationObserverData***.<br>
We will work with the currently selected object.

### Generic properties

Add the following properties and command handlers to the view model:

```csharp
public ICommand StepNorth { get { return new DelegateCommand(x => Move(0.1, 0)); }}
public ICommand StepSouth { get { return new DelegateCommand(x => Move(-0.1,0)); }}
public ICommand StepEast  { get { return new DelegateCommand(x => Move(0,0.1)); }}
public ICommand StepWest  { get { return new DelegateCommand(x => Move(0,-0.1)); }}
private void Move(double lat, double lon)
{
   if (_selectedObserver != null)
   {
       _selectedObserver.Position = new GeoPos(_selectedObserver.Position.Lat + lat,
                                               _selectedObserver.Position.Lon + lon);
       _elevationAnalysisLayer.UpdateObserver(_selectedObserver);
   }
}
public double SelectedHeightAboveGround
{
    get { return  _selectedObserver != null ? _selectedObserver.HeightAboveGround : 0; }
    set
    {
        if (_selectedObserver != null)
        {
            _selectedObserver.HeightAboveGround = value;
            _elevationAnalysisLayer.UpdateObserver(_selectedObserver);
        }
    }
}       
public int SelectedRange
{
    get { return _selectedObserver != null ? _selectedObserver.Range : 0; }
    set
    {
        if (_selectedObserver != null) 
        {
            _selectedObserver.Range = value;
            _elevationAnalysisLayer.UpdateObserver(_selectedObserver);
        }
    }
}
public bool EnableSelectedInteractiveMove
{
    get { return _selectedObserver != null && _selectedObserver.EnableInteractiveMove; }
    set { if (_selectedObserver != null) _selectedObserver.EnableInteractiveMove = value; }
}
public bool EnableSelectedInteractiveRadius
{
    get { return _selectedObserver != null && _selectedObserver.EnableInteractiveRadius; }
    set { if (_selectedObserver != null)  _selectedObserver.EnableInteractiveRadius = value; }
}
```

Then, add the following to your window xaml, and bind to the view model properties:

```xml
<StackPanel Orientation="Horizontal">
    <Button Content="N" Margin="2" Command="{Binding ElevationAnalysisViewModel.StepNorth}"/>
    <Button Content="S" Margin="2" Command="{Binding ElevationAnalysisViewModel.StepSouth}"/>
    <Button Content="E" Margin="2" Command="{Binding ElevationAnalysisViewModel.StepEast}"/>
    <Button Content="W" Margin="2" Command="{Binding ElevationAnalysisViewModel.StepWest}"/>
</StackPanel>
<StackPanel Orientation="Horizontal">
    <Label Content="H: " />                            
    <TextBox Name="FreesightHeigt" Width="40" TextAlignment="Right" Margin="2"
             Text="{Binding ElevationAnalysisViewModel.SelectedHeightAboveGround}" 
             />
    <Label Content="R: " />                           
    <TextBox Width="40" TextAlignment="Right" Margin="2"
             Text="{Binding ElevationAnalysisViewModel.SelectedRange}" 
             />
</StackPanel> 
<CheckBox Name="ChkEnableInteractiveMove" Margin="2" 
          Content="Enable Move" ToolTip="Enable Interactive Move" 
          IsChecked="{Binding ElevationAnalysisViewModel.EnableSelectedInteractiveMove}" />
<CheckBox Name="ChkEnableInteractiveRadius" Margin="2" 
          Content="Range" ToolTip="Enable Interactive Radius" 
          IsChecked="{Binding ElevationAnalysisViewModel.EnableSelectedInteractiveRadius}" />
```

You should now be able to change the range, position and height level of the selected observer, and disable change by dragging center points and range handles.

{% include image.html file="clientapi/elevationanalysis/elevationobservergenalteration.jpg" caption="Generic properties alteration" %}

**Note!**<br>
Observe that you still can change size and position from your GUI.

### Free sight observers

Free sight observers are reached through ***ILineOfSightObserverData***, with two additional properties, *HiddenColor* and *VisibleColor*.

Add a button to initiate change of color . . .

```xml
<Button Content="Toggle Colors" ToolTip="Toggle Selected Observer Colors" Margin="2"
        Command="{Binding ElevationAnalysisViewModel.ToggleSelectedColorsCommand}"
        />       
```

. . . with command handler:

```csharp
public ICommand ToggleSelectedColorsCommand
{
    get { return new DelegateCommand(x => OnToggleSelectedColorsCommand()); }
}
private void OnToggleSelectedColorsCommand()
{
    if (_selectedObserver.ObserverType == ObserverType.LineOfSight)
    {
        var freesightObserver = (ILineOfSightObserverData) _selectedObserver;

        if (freesightObserver.HiddenColor == Colors.Pink)
        {
            freesightObserver.HiddenColor = Colors.LightGray;
            freesightObserver.VisibleColor = Colors.DimGray;
        }
        else if (freesightObserver.HiddenColor == Colors.LightGray)
        {
            var temp = new LineOfSightObserverData("x");
            freesightObserver.HiddenColor = temp.HiddenColor;
            freesightObserver.VisibleColor = temp.VisibleColor;
        }                                
        else
        { 
            freesightObserver.HiddenColor = Colors.Pink;
            freesightObserver.VisibleColor = Colors.PowderBlue;
        }
    }
    
    _elevationAnalysisLayer.UpdateObserver(_selectedObserver);
}
```

By pressing the button, the hidden/visible will change between the two sets of colors (and the default colors (Pink/PowderBlue and LightGray/DimGray) and the default colors.

{% include image.html file="clientapi/elevationanalysis/elevationobserverfslteration.png" caption="Changing free sight properties" %}

### Max/min observers

Max/min observers are reached through ***IMaxMinElevationObserverData***, with two additional properties, *Legend* and *IsElevationColoringEnabled*.

Add a checkbox to manage enable/disable elevation coloring. We will use the button from the previous section to change legend color sets:

```xml
<CheckBox Name="ChkElevationColoringEnabled" Margin="2" 
          Content="Coloring" ToolTip="Enable Elevation Coloring" 
          IsChecked="{Binding ElevationAnalysisViewModel.IsElevationColoringEnabled}" 
          Visibility="{Binding ElevationAnalysisViewModel.ColoringVisibility}" />
```

Add properties to handle elevation coloring, and extend the button command handler to handle legend update:

```csharp
private void OnToggleSelectedColorsCommand()
{
    var maxMinObserver = (IMaxMinElevationObserverData) _selectedObserver;

    if (_selectedObserver.ObserverType == ObserverType.LineOfSight)
   . . .
    else if (_selectedObserver.ObserverType == ObserverType.MaxMinElevation)
    {
        var legend = new ElevationLegend();

        if (maxMinObserver.Legend.Items.Count == legend.Items.Count)
        {
            legend.Items = new ObservableCollection<IElevationLegendItem>
                               {
                                   new ElevationLegendItem(-1000, Colors.Transparent, 1, Colors.Transparent),
                                   new ElevationLegendItem(1, Colors.LightGreen, 200, Colors.LightGreen),
                                   new ElevationLegendItem(200, Colors.LightYellow, 500, Colors.LightYellow),
                                   new ElevationLegendItem(500, Colors.LightPink, 1000, Colors.LightPink),
                                   new ElevationLegendItem(1000, Colors.RosyBrown, 800, Colors.RosyBrown)
                               };
        }
        maxMinObserver.Legend = legend;
    }
    _elevationAnalysisLayer.UpdateObserver(_selectedObserver);
}
. . .
public bool IsElevationColoringEnabled
{
    get 
    {
        if (_selectedObserver != null && _selectedObserver.ObserverType == ObserverType.MaxMinElevation)
            return ((IMaxMinElevationObserverData)_selectedObserver).IsElevationColoringEnabled;
        return true;
    }
    set
    {
        if (_selectedObserver != null && _selectedObserver.ObserverType == ObserverType.MaxMinElevation)
        {
            ((IMaxMinElevationObserverData)_selectedObserver).IsElevationColoringEnabled = value;
            _elevationAnalysisLayer.UpdateObserver(_selectedObserver);
        }
    }
}
public Visibility ColoringVisibility { get { return SelectedIsMaxMinObserver ? Visibility.Visible : Visibility.Collapsed; } }
```

By pressing the button change between the and the default legend and the alternative legend. Unchecking the check box removes coloring.

{% include image.html file="clientapi/elevationanalysis/elevationobserverminmaxlteration.png" caption="Changing max/min properties" %}

**Note!**<br>
The default legend has convergent coloring, while the alternative legend has uniform coloring for each elevation range.

{% include links.html %}