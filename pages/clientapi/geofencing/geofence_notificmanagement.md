---
title: Geo Fence, Notification Management
keywords: 
tags: 
sidebar: clientapi_sidebar
permalink: geofence_notificmanagement.html
summary: This section describes how to ...
---

Geo Fence Nottifications are accessed through 
[INotificationHandlingService Interface] of the Geo Fencing Service.

For additional information, see [Notifications](xxx/notifications.html)

## Connect to service
 
Create a view model class (*GeoFenceNotificationsViewModel*) for notification management, inheriting ViewModelBase.

```csharp
public class GeoFenceNotificationsViewModel : ViewModelBase
{
}
```

Create a constructor, and connect to the NotificationHandling part of the GeoFencingService. The code will look like this: 

```csharp
private IBindingFactory _bindingFactory = new BindingFactory();
private IEndpointAddressFactory _endpointAddressFactory = new EndpointAddressFactory();
private NotificationHandlingServiceClient _clientNotification;
. . .
public GeoFenceNotificationsViewModel()
{
    _clientNotification =
        new NotificationHandlingServiceClient(
            _bindingFactory.NewFromConfigurationFile(GeoFenceDefs.DefaultGeoFenceNotificationName),
            _endpointAddressFactory.NewFromConfigurationFile(GeoFenceDefs.DefaultGeoFenceNotificationName));

    _clientNotification.ServiceConnected += OnNotificationClientServiceConnected;
    _clientNotification.Connect();
}

private void OnNotificationClientServiceConnected(object sender, EventArgs args)
{
}
```

Make sure that your App.config file contains an end point specification for the GeoFencingService/NotificationHandling. See [Service Configuration](clientapi_serviceconfiguration.html).

Then, define and instansiate the GeoFenceNotificationsViewModel in the declarations and constructor of the main view model (MariaWindowViewModel).

```csharp
public GeoFenceNotificationsViewModel GeoFenceNotificationsViewModel { get; set; }
. . .
public MariaWindowViewModel()
{
    . . .
    GeoFenceNotificationsViewModel = new GeoFenceNotificationsViewModel();
    . . .
}
```

## Collect Notifications

Our first step is to collect current notifications from the connected service, and display the result.\\
For this, we need to add a list property and a command handler to the view model.

```csharp
private NotificationQueryResult _notifications;

public ObservableCollection<string> DisplayListItems { get; private set; }

public ICommand PollCmnd { get { return new DelegateCommand(x => PollNotifications()); } }

public GeoFenceNotificationsViewModel()
{
    DisplayListItems = new ObservableCollection<string>();
    . . .
}
. . .
private void PollNotifications()
{
    if (_clientNotification == null || !_clientNotification.Connected)
        return;

    _notifications = _clientNotification.GetNotifications("", -1);

    DisplayListItems.Clear();
    if (_notifications != null)
    {
        foreach (var item in _notifications.Notifications)
        {
            DisplayListItems.Add(item.Heading);
        }
    }
    NotifyPropertyChanged(() => DisplayListItems); 
}
```

Then add a button and a list box to the GUI, and bind them to the new view model properties.

```xml
<Button  MinWidth="60" HorizontalAlignment="Left"  Margin="0,0,5,0"
         Content="Poll"
         Command="{Binding GeoFenceNotificationsViewModel.PollCmnd}" />

<GroupBox Header="Notifications:">
    <ListBox Height="Auto" VerticalAlignment="Top" MinWidth="40"
             ItemsSource="{Binding GeoFenceNotificationsViewModel.DisplayListItems}"
             />
</GroupBox>
```


All current notifications from the service are now displayed. The number of items may be large!
It will look like this:

{% include image.html file="clientapi/geofencing/notificationcollection.png" caption="Notification Collection" %}

## Reduce amount of Notifications

Notifications are collected by request, and by default, all notifications are collected. As we normally are interested in a limited set of information, there are two different ways to reduce the information.


*  Retreive by "Generation"

*  Specify filter conditions

The reduction methods can be used saparately, or in combination.
 
### Notification generations

The Geo Fencing Service, Notification Handling maintains a "generation" counter. The counter value is stepped up every time new geo fence events are detected. The current generation is available in the returned notification result.

By specifying the generation counter of the last read information, you will have a mechanism that retrieves new notifications only.

```csharp
public class GeoFenceNotificationsViewModel : ViewModelBase
{
    . . .
    private void PollNotifications()
    {
        . . .
        _notificationResult = 
          _clientNotification.GetNotifications("", 
                                               (_notificationResult != null) ?
                                               _notificationResult.Generation :
                                               -1);
        . . .
    }
```

The result will look like this:

{% include image.html file="clientapi/geofencing/notificationsbygenerations.png" caption="Notification by Generation" %}

### Filtering notifications

The notification filter is an XML string, specifying one or or several conditions. Here we will include a simple filter supressing low level notifications, and a composite filter supressing low level notifcations older than 60 seconds. We'll also add automatic poll whenever the filter conditions change.

In the view model, add the following properties:

```csharp
private string _conditionXml;
public string ConditionXml
{
    get { return _conditionXml; }
    set
    {
        _conditionXml = value;
        NotifyPropertyChanged(() => ConditionXml);
    }
}

private bool _supressLowLevel;
public bool SupressLowLevel
{
    get { return _supressLowLevel; }
    set
    {
        _supressLowLevel = value;
       
        NotifyPropertyChanged(() => SupressLowLevel);
        PollNotifications();
    }
}

private bool _includeAllNew;
public bool IncludeAllNew
{
    get { return _includeAllNew; }
    set
    {
        _includeAllNew = value;

        NotifyPropertyChanged(() => IncludeAllNew);
        PollNotifications();
    }
}

```

Update the polling mechanism:

```csharp
. . .
private void PollNotifications()
{
    . . .    
    // By filter conditions
    UpdateConditionXml();
    _notificationResult = _clientNotification.GetNotifications(ConditionXml, -1);
    . . .    
}
  
private void UpdateConditionXml()
{
    var conditionXml = "";
    var compMainCondition = new CompositeCondition { Operator = GroupOperator.And };

    if (SupressLowLevel)
    {
        var compSubCondition = new CompositeCondition { Operator = GroupOperator.Or };
        compSubCondition.Children.Add(new FieldCondition("Level", 
                                                         NotificationLevel.Low.ToString(), 
                                                         FieldOperator.NEq));

        if (IncludeAllNew)
        {
            compSubCondition.Children.Add(new AgeCondition { Operator = FieldOperator.Lt, 
                                                            Age = 60.0 });
        }
        conditionXml = GenericFielsConditionFilter(compSubCondition);
        compMainCondition.Children.Add(compSubCondition);
    }

    ConditionXml = conditionXml;
}

private string GenericFielsConditionFilter(ICondition condition)
{
    var filterXml = @"<GeoFencingFilters>"
                    + (new ConditionXmlParser(new ConditionFactory())).WriteCondition(condition)
                    + @"</GeoFencingFilters>";

    var fsp = new GeoFencingXmlParserUtils();
    var result = fsp.ParseGeoFenceFilters(filterXml);

    return result;
}
```

Then add components and bindings to the GUI:

```xml
<GroupBox Header="Filters" >
    <StackPanel Orientation="Vertical">
        <CheckBox Content="Level - Suppress 'Low'"
                  IsChecked="{Binding GeoFenceNotificationsViewModel.SupressLowLevel}" />
        <CheckBox Content="Age - Include all new!"
                  IsChecked="{Binding GeoFenceNotificationsViewModel.IncludeAllNew}" 
                  IsEnabled="{Binding GeoFenceNotificationsViewModel.SupressLowLevel}"/>        
        <GroupBox>
            <Label Content="{Binding GeoFenceNotificationsViewModel.ConditionXml}" />
        </GroupBox>
    </StackPanel>
</GroupBox>
```

After creating some notifications of different levels, the result with different filters would be something like this:


{% include image.html file="clientapi/geofencing/notificationfilters.png" caption="Filtered Notifications" %}

Examples of filtering according to specific tracks and drawobjects can be found in [Visualization](maria_gdk/programming/getting_started/mariageofencing/4-visualization)

### Add user data to notification.

It is not possible to modify the notification information, however, it is possible to add user data, like processing information.  
In the sample application, you can find an example of adding acknowledged tag to selected notification item, with corresponding "Acknowledged" filter. Setting the acknowledge tag like this:

```csharp
_clientNotification.SetNotificationUserData(
  _notificationResult.Notifications[SelectedNotificationItemIndex].Id,
  new UserDataDefinition 
  {
    Owner = "MariaGeoFenceTestClient", 
    Key = "userdata/*/state", 
    Value = "acknowledged" 
  });

PollNotifications();
```

Details for items selction and filter setting is found in the sample code.

The result will be like this:

{% include image.html file="clientapi/geofencing/notificationacknowledge.png" caption="Notification Acknowledge" %}

{% include links.html %}

