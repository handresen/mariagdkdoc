---
title: Search Client
keywords: GDK
tags: [geolocation, geosearch, sampleapplication, wpf]
sidebar: clientapi_sidebar
permalink: clientapi_search.html
summary: This section describes how to create a map client utilizing Maria search layer and location service functionality.
---
{% include image.html file="clientapi/search/mariasearch.png" max-width=400 caption="Search Client" %}

***Maria Search Layer*** provides presentation of geographic serch results from various data providors. 

* In this example place names will be collected from the *Locatopn service* and presented in the *Search Layer. *

* The example is based on a map components corresponding to 
[Maria Basic Map Client](maria_gdk/programming/getting_started/mariabasicmapclient), with 
[web maps](maria_gdk/programming/getting_started/mariamapinteractionclient/maplayerinteraction#adding_web_maps) and  
[tools](maria_gdk/programming/getting_started/mariamapinteractionclient/toolsinteraction).

* You will need to include the following NuGet packages (in addition to ***TPG.Maria.MapLayer***) as a minimum:

  *  ***TPG.Maria.SearchLayer***
  *  ***TPG.GeoFramework.LocationServiceClient***
  *  ***TPG.Maria.MapLayer*** 
  *  ***TPG.Geoframework.WebMaps.WrapperClient*** (optionally) for WEB maps.

* To obtain location info you also need to have a Location Service available.

* Sample code for this section is the ***MariaSearch*** project, project, in ***MariaAdditionalComponents*** 
folder of the ***Sample Projects*** solution.

## Creating the Search Layer.

Create an interface class (*SearchViewModel.cs*) for interaction with the Search layer, inheriting ViewModelBase, and with the search layer as parameters to the constructor.

Create fields for interaction with the search layer, and add an event handler for the search layer LayerInitialized event.

```csharp
private readonly IMariaSearchLayer _searchLayer;

public SearchViewModel(IMariaSearchLayer searchLayer)
{
    _searchLayer = searchLayer;
    _searchLayer.LayerInitialized += SearchLayerOnLayerInitialized;
}
private void SearchLayerOnLayerInitialized()
{
}
```

Then, create the Search Layer and include the SearchViewModel in the declarations and constructor of the main view model (MariaWindowViewModel).

```csharp
public SearchViewModel SearchViewModel { get; set; }
private readonly IMariaSearchLayer _searchLayer;
. . .
public MariaWindowViewModel()
{
    . . .
    _searchLayer = new TPG.Maria.SearchLayer.SearchLayer();
    SearchViewModel = new SearchViewModel(_searchLayer);
    Layers.Add(_searchLayer);
    . . .
```

## Connecting to the Location Service

Create fields for interaction with the location service client and geo location provider, and initialize them in the constructor.

```csharp
private readonly ILocationServiceClient _locationServiecClient;
private readonly LocationProvider _geoLocationProvider;
. . .
public SearchViewModel(IMariaSearchLayer searchLayer)
{
   . . .
    LocationDbInfo = new ObservableCollection<LocationDatabaseInfo>();

    ILocationServiceFactory locationServiceFactory = 
        new LocationServiceFactory(new BindingFactory(), new EndpointAddressFactory());
    _locationServiecClient = locationServiceFactory.New("LocationService");
    _locationServiecClient.Connect(2000);

    _geoLocationProvider = new LocationProvider
                               {
                                   LocationServiceClient = _locationServiecClient
                               };
}
```

And connect to the data provider during at the LayerInitialized event, add the search provider to the search layer, and add and implement a search complete event handler to preesent the result.

```csharp
private void SearchLayerOnLayerInitialized()
{
    var nvc = System.Configuration.ConfigurationManager .GetSection("LocationConfig") 
                       as NameValueCollection;
    if (nvc == null)
    {
        MessageBox.Show("Could not provide Location Service info... ", 
                        "Connection error!",
                        MessageBoxButton.OK, MessageBoxImage.Exclamation);
        return;
    }
    var activeDb = nvc.Get("DefaultDatabase");
    var dbInfo = _geoLocationProvider.GetLocationDatabaseInfo();
    if (dbInfo == null)
    {
        MessageBox.Show("Could not provide Location Database Info... ", 
                        "Database error!", 
                        MessageBoxButton.OK, MessageBoxImage.Exclamation);
        return;
    }
    foreach (var dbi in dbInfo)
    {
        if (dbi.DataSource.Contains(activeDb))
            _geoLocationProvider.ActiveDatabaseInfo = dbi;
    }
    if (_geoLocationProvider.ActiveDatabaseInfo == null)
    {
        MessageBox.Show("Could not provide Active Database Info... ", "Database error!",
                        MessageBoxButton.OK, MessageBoxImage.Exclamation);
    }
   
    _searchLayer.SearchProviders.Add(_geoLocationProvider);
    _searchLayer.SearchCompleted += SearchLayerOnSearchCompleted;
}
private void SearchLayerOnSearchCompleted(object sender, EventArgs args)
{
    NotifyPropertyChanged(() => SearchMatches);
}
```

**Note!**<br>
Remember to add location service configuration to the app.config file, as described in [ ***Service Configuration Setup***](clientapi_serviceconfiguration.html).
      
Set breaks in the view model constructor and the layer initialized delegate before running the application. 
Step through the initialization to see that the connection is OK and the active database is set successfully.

##  Perform simple searches

We will now set up GUI elements to perform a simple search through the default datasource:

*  text box for specification of search text
*  button to activate the search
*  list view to display the search results.

Add properties and command handlers in the view model. 

```csharp
public ICommand OnSearchCmd { get { return new DelegateCommand(x => DoSearch()); } }
private string _searchText;
public string SearchText
{
    get { return _searchText; }
    set 
    {
        _searchText = value;
        NotifyPropertyChanged(() => SearchText);
    }
}
public ObservableCollection<ISearchMatch> SearchMatches
{
    get { return _searchLayer.SearchMatches; }
}
public void DoSearch()
{
    if (String.IsNullOrWhiteSpace(SearchText))     return;
    var query = new SearchQuery
    {
        SearchText = SearchText,
        MaxHitCount = 20, MaxInternalHits = 20,
        CenterPosition = _searchLayer.GeoContext.CenterPosition
    };
    _searchLayer.Search(query);
}
```

and bind to components in the xaml:

```csharp

<TextBox Margin="2" 
         Text="{Binding  SearchViewModel.SearchText}"/>
<Button Margin="2" 
        Content="Search" 
        Command="{Binding SearchViewModel.OnSearchCmd}" />
<ListView Name="listSearchMatches" MinWidth="50" 
          ItemsSource="{Binding SearchViewModel.SearchMatches}" >
    <ListView.ItemTemplate>
        <DataTemplate>
            <Label Content="{Binding Path=Name}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```


Running the application, you should now be able to search for locations. Observe that the locations are marked in the map.

{% include image.html file="clientapi/search/simplesearch.png" caption="Simple location search results" %}

##  Interacting with the location search results

Add two check boxes to the GUI, and bind them to properties for display of name and mark for the search match marks. 
In the view model:

```csharp
public bool ShowSearchMatchMark
{
    get { return _searchLayer.ShowSearchMatchMark; }
    set 
    {
        _searchLayer.ShowSearchMatchMark = value;
        NotifyPropertyChanged(() => ShowSearchMatchMark);
    }
}
public bool ShowSearchMatchName 
{
    get { return _searchLayer.ShowSearchMatchName; }
    set 
    {
        _searchLayer.ShowSearchMatchName = value;
        NotifyPropertyChanged(() => ShowSearchMatchName);
    }
}
```

And in the Xaml:

```xml
<CheckBox Width="Auto"  Margin="2"
          Content="Show Name" 
          IsChecked="{Binding SearchViewModel.ShowSearchMatchName, Mode=TwoWay}" />
<CheckBox Width="Auto" Margin="2"
              Content="Show Mark" 
              IsChecked="{Binding SearchViewModel.ShowSearchMatchMark, Mode=TwoWay}" />
```
              
Observe that name and mark for the match marks are displayed according to the check boxes.

{% include image.html file="clientapi/search/shownameandmark.png" max-width=500 caption="Show name and mark" %}

Now, we will synchronize selection of marks in the map with selection of corresponding item in the search match list, and move to the selected item. This is done through the search match view. 

In the view model, add a property to handle item selection, and a field to hold the view. Initialize the field in the layer initialized handler. 

Also add and implement selection event handlers for the search layer and the search match view.

```csharp
private CollectionView _searchMatchView;
public ISearchMatch SelectedSearchMatch
{
    get { return _searchLayer.SelectedSearchMatch; }
    set
    {
        if (value != null )
            _searchLayer.GeoNavigator.CenterPosition = value.Position;
        _searchLayer.SelectedSearchMatch = value;
    }
}
. . .
private void SearchLayerOnLayerInitialized()
{
    . . . 
    _searchMatchView = (CollectionView)CollectionViewSource.GetDefaultView(SearchMatches);
    _searchLayer.SearchMatchSelectionChanged += OnSearchMatchSelectionChanged;
    _searchMatchView.CurrentChanged += OnCurrentChanged;
}
void OnCurrentChanged(object sender, EventArgs e)
{
    if (_searchMatchView.CurrentItem == null)    return;

    SelectedSearchMatch = _searchMatchView.CurrentItem as ISearchMatch;
    NotifyPropertyChanged(() => SelectedSearchMatch);
}
void OnSearchMatchSelectionChanged(object sender, 
SearchMatchSelectionChangedEventArgs args)
{
    _searchMatchView.MoveCurrentTo(args.SelectedSearchMatch);
    NotifyPropertyChanged(() => SelectedSearchMatch);
}
```

Bind the list view “SelectedItem” to the new propery:

```csharp
<ListView Name="listSearchMatches" MinWidth="50" 
      ItemsSource="{Binding SearchViewModel.SearchMatches}" 
      SelectedItem="{Binding SearchViewModel.SelectedSearchMatch}" >
    <ListView.ItemTemplate>
    . . .
</ListView>
```

Observe that the map view in sentered to the selected search matc item.

{% include image.html file="clientapi/search/searchclient.png" caption="Center to selected search match" %}

##  Working with search facets

The returned search information is categorized in facets. The returned resuls amy include these fasets – and the search may also be limited to specific facets, or exclude facets. The information is returned as ISearcFacet objets.
To show this, we will add two list views, one for returned with the searc result, and one for search limitation. 
Create a class extending the SearchFacet object to use in the lists.

```csharp
public class ExtendedSearchFacet 
{
    public ISearchFacet Facet { get; private set; }
    public ExtendedSearchFacet( ISearchFacet facet) 
    {
        Facet = facet;
    }
    public bool IsExcluded
    {
        get { return Facet.ExcludeFacet; }
        set { Facet.ExcludeFacet = value; }
    }
    public override string ToString()
    {
        return Facet.DisplayName + "<" + GetGroup(Facet.Name) + ">" + Facet.Count;
    }
    public string Info
    {
        get { return ToString(); }
    }
    private string GetGroup(string name)
    {
        if (name == "cc")
                 return "Country";
        if (name == "fcode")
                 return "Feature Code";
        if (name == "fclass")
                 return "Feature Class";
        return name;
    }
}
```

Add the following to your view model:

```csharp
public ICommand OnClearFacetsCmd 
{ get { return new DelegateCommand(x => DoClearFacet()); } }

public ObservableCollection<ExtendedSearchFacet> Facets { get; private set; }
public ObservableCollection<ExtendedSearchFacet> SelectedFacets { get; private set; }
public ExtendedSearchFacet SelectedFacet
{
    set
    {
        if (value != null)
            _searchLayer.SelectedFacets.Add(value.Facet);
        else
             _searchLayer.SelectedFacets.Clear();
    }
}
public int MinimumFacetOccurenceFilter
{
    get { return _searchLayer.MinimumFacetOccurenceFilter; }
    set
    {
        _searchLayer.MinimumFacetOccurenceFilter = value;
        NotifyPropertyChanged(() => MinimumFacetOccurenceFilter);
    }
}
. . .
private void SearchLayerOnLayerInitialized()
{
    . . .
     Facets = new ObservableCollection<ExtendedSearchFacet>();
     SelectedFacets = new ObservableCollection<ExtendedSearchFacet>();

     _searchLayer.Facets.CollectionChanged += OnFacetsCollectionChanged;
     _searchLayer.SelectedFacets.CollectionChanged += OnSelectedFacetsCollectionChanged;
 }
private void OnSelectedFacetsCollectionChanged(object sender, 
NotifyCollectionChangedEventArgs e)
{
    HandleFacetCollectionChanged(e, SelectedFacets);
    NotifyPropertyChanged(() => SelectedFacets);
}
private void OnFacetsCollectionChanged(object sender, NotifyCollectionChangedEventArgs e)
{
    HandleFacetCollectionChanged(e, Facets);
    NotifyPropertyChanged(() => Facets);
}
. . .
private void HandleFacetCollectionChanged(NotifyCollectionChangedEventArgs e, 
    ObservableCollection<ExtendedSearchFacet> facets)
{
    if (e.Action == NotifyCollectionChangedAction.Reset)
        facets.Clear();
    else if (e.Action == NotifyCollectionChangedAction.Add)
    {
        foreach (var obj in e.NewItems)
        {
            var facet = (ISearchFacet) obj;
            if (facet == null)
                continue;

            var extended = new ExtendedSearchFacet(facet);
            facets.Add(extended);
        }
    }
}
private void DoClearFacet()
{
    SelectedFacet = null;
    SelectedFacets.Clear();

    DoSearch();
}
```

Remember to update the search query to include facets:

```csharp
public void DoSearch()
{
    . . .
    var query = new SearchQuery
                    {   . . .
                        ExtractFacets = true,
                        Facets = ConvertList(SelectedFacets)
                    };
    . . .
}
private List<ISearchFacet> ConvertList(IEnumerable<ExtendedSearchFacet> extendedList)
{
    var list = new List<ISearchFacet>();
    foreach (var extFacet in extendedList)
    {
        list.Add(extFacet.Facet);
    }
    return list;
}
```

And to the main window xaml:

```xml
<Button Margin="2" 
        Content="Reset Facets"
        Command="{Binding SearchViewModel.OnClearFacetsCmd}" />
<ListView Name="listFacets" MinWidth="50" 
          ItemsSource="{Binding SearchViewModel.SelectedFacets}" >
    <ListView.ItemTemplate>
        <DataTemplate>
            <CheckBox Content="{Binding Path=Info}"  Margin="2"
                      IsChecked="{Binding Path=IsExcluded}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
<ListView Name="listSelectedFacets" MinWidth="50"
          ItemsSource="{Binding SearchViewModel.Facets}"             
          SelectedItem="{Binding SearchViewModel.SelectedFacet}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <Label Content="{Binding Path=Info}" />                  
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

When search is activated, a list of facets for the search results is displayed. Selecting some of the facets and reactivating the searc will return matches within the selected facets only. Note that checked “Selected Factes” items are excuded form the search.

{% include image.html file="clientapi/search/searchwithfacets.png" caption="Location search with facets" %}

##  Selecting location data sources

Add an property to hold the available location database entries, and one for the selected entry.

```csharp
public ObservableCollection<LocationDatabaseInfo> LocationDbInfo { get; private set; }
public LocationDatabaseInfo ActiveLocationDbInfo
{
    get { return _geoLocationProvider.ActiveDatabaseInfo; }
    set
    {
        _geoLocationProvider.ActiveDatabaseInfo = value;
        NotifyPropertyChanged(() => ActiveLocationDbInfo);
        DoSearch();
    }
}
Modify the data source initialization
private void SearchLayerOnLayerInitialized()
{
    . . .
    LocationDbInfo = new ObservableCollection<LocationDatabaseInfo>();
    foreach (var dbi in dbInfo)
    {
        LocationDbInfo.Add(dbi);
        if (dbi.DataSource.Contains(activeDb))
            ActiveLocationDbInfo = dbi;
    }
    NotifyPropertyChanged(() => LocationDbInfo);
     . . .
}
```

```xml
Add list view to the main window xaml, and bind to the properties.
<ListView MinWidth="50" 
          ItemsSource="{Binding SearchViewModel.LocationDbInfo}" 
          SelectedItem="{Binding SearchViewModel.ActiveLocationDbInfo}"  >
    <ListView.ItemTemplate>
        <DataTemplate>
            <Label Content="{Binding Path=DataSource}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Running the application, you will observe that the seach results differs between the selectable data sources.

{% include image.html file="clientapi/search/searchdatasource.png" caption="Selecting Location data source" %}

{% include links.html %}