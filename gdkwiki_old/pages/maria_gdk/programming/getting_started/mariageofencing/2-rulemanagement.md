## Fence Rule Management

Geo Fence Rules are accessed programmatically through the
[IGeoFencingRuleService interface](http://support.teleplanglobe.com/MariaGDKDoc/html/42C43787.htm) of the Geo Fencing Service.

`<WRAP center round important>`
Please note that defined rules will have effect for all clients connected to the  service. \\
In a multi-client environment, Rule Management will typically be a task performed by administrators.
`</WRAP>`

For additional information, see [GeoFencing, Rules](maria_gdk/programming/functionality/geofencing/rules).

### Connect services

 
Create an interface class (//GeoFenceRuleViewModel//) for rule management, inheriting ViewModelBase.

```csharp
 public class GeoFenceRuleViewModel: ViewModelBase
 {
 }
```

Create a constructor, and connect to the FenceRule part of the GeoFencingService from the constructor. The code will look like this:

```csharp
private IBindingFactory _bindingFactory = new BindingFactory();
private IEndpointAddressFactory _endpointAddressFactory = new EndpointAddressFactory();
private FenceRuleServiceClient _clientFenceRule;
. . .
public GeoFenceRuleViewModel()
{
    _clientFenceRule =
        new FenceRuleServiceClient(
            _bindingFactory.NewFromConfigurationFile("GeoFencingService/FenceRule"),
            _endpointAddressFactory.NewFromConfigurationFile("GeoFencingService/FenceRule"));
    _clientFenceRule.ServiceConnected += OnFenceRuleClientServiceConnected;
    _clientFenceRule.Connect();
}
private void OnFenceRuleClientServiceConnected(object sender, EventArgs args)
{
}
```

Make sure that your App.config file contains an end point specification for the GeoFencingService/FenceRule. See [Service Configuration](maria_gdk/programming/getting_started/mariabasicmapclient/service_configuration).

Then, define and instansiate the GeoFenceRuleViewModel in the declarations and constructor of the main view model (//MariaWindowViewModel//).

```csharp
public GeoFenceRuleViewModel GeoFenceRuleViewModel { get; set; }
. . .
public MariaWindowViewModel()
{
    . . .
    GeoFenceRuleViewModel = new GeoFenceRuleViewModel();
    . . .
}
```

### Add generic rules

When the service connection is established, rules can be added. \\
As a start, we will add some generic rules. 

```csharp
private void OnFenceRuleClientServiceConnected(object sender, EventArgs args)
{
  // Example Guids!
  Guid SimpleLeavingRuleId =  new Guid("{4E4FD902-940B-4CBF-BBBB-BB48A8C07EF9}");
  Guid SimpleEnterRuleId = new Guid( "{5359804C-624F-4A44-973E-E1E6E0BF2912}");
  Guid SimpleCrossingRuleId = new Guid( "{60A70116-379F-4245-B2F1-68B4542FA8F8}");

  _clientFenceRule.AddRule(SimpleRule(SimpleLeavingRuleId,
                                      GeoFenceInteractions.Entering, 
                                      NotificationLevel.High));
  _clientFenceRule.AddRule(SimpleRule(SimpleEnterRuleId,
                                      GeoFenceInteractions.Leaving,
                                      NotificationLevel.Low));
  _clientFenceRule.AddRule(SimpleRule(SimpleCrossingRuleId,
                                      GeoFenceInteractions.Crossing,
                                      NotificationLevel.Medium));  
}

private GeoFenceRuleDef SimpleRule(Guid ruleId,
                                   GeoFenceInteractions interaction, 
                                   NotificationLevel level)
{
  var ruleDef = new GeoFenceRuleDef
  {
    Id = ruleId,
    Interactions = interaction,
    NotificationTemplate = CreateTemplate(level)
  };
  
  return ruleDef;
}

private NotificationTemplateDef CreateTemplate(NotificationLevel level)
{
    var template = new NotificationTemplateDef
    {
        NotificationLevel = level,
        Heading = "[level], [track.name]([track.identity]) [interaction] [shape.Name], " +
                  "Crs:[track.course]° Spd:[track.speed], [datetime]"
    };

    template.TrackFields.AddRange(new[] {"name", "identity"});
    template.GeoShapeFields.AddRange(new[] {"Name"});
    
    return template;
}

```

`<WRAP  round box >`
Adding some tracks and geo shapes (see [Data Preparation](maria_gdk/programming/getting_started/mariageofencing/0-preparesampledata)), you shoud now be able to generate notifications when moving tracks in and out of geo areas, and across lines.
`</WRAP>`
How to display notifications is described in [Notification Management](maria_gdk/programming/getting_started/mariageofencing/3-notificationmanagement)

### Get rules from the service

We will now ask the service for currently defined rules, and display the result. For this, we need to add a list property and a command handler to the view model.

```csharp
public ICommand GetRulesCmnd { get { return new DelegateCommand(x => GetRules()); } }
private void GetRules()
{
    NotifyPropertyChanged(() => RulesCollection);
}

private List`<GeoFenceRuleDef>` _rulesList;
public ObservableCollection`<string>` RulesCollection
{
    get
    {
        var result = new ObservableCollection`<string>`();

        if (_clientFenceRule == null)
            result.Add("--");
        else
        {
            _rulesList = _clientFenceRule.GetRules();
            if (_rulesList == null || !_rulesList.Any())
                result.Add("No rules...");
            else
                foreach (var rule in _rulesList)
                {
                    result.Add(
                      "=> " + rule.Interactions + ", " + 
                      rule.TrackConditionXml + rule.NotificationTemplate.NotificationLevel +
                      " GFCond: " + (string.IsNullOrWhiteSpace(rule.GeoFenceConditionXml) ? "No" : "Yes") +
                      " TCond: " + (string.IsNullOrWhiteSpace(rule.TrackConditionXml) ? "No" : "Yes"));
                }
        }
        return result;
    }
}

private void OnFenceRuleClientServiceConnected(object sender, EventArgs args)
{
    . . .
    NotifyPropertyChanged(() => RulesCollection);
}
```

Then add a button and a list box to the GUI, and bind them to the new view model properties.

```xml
  <Button Content="Get Rules" 
          Command="{Binding GeoFenceRuleViewModel.GetRulesCmnd}" />
  <ListBox MinWidth="60"
           ItemsSource="{Binding GeoFenceRuleViewModel.RulesCollection}" />
```

`<WRAP round box >`
All rules defined in the service are now displayed like this:
`</WRAP>`

{{ :maria_gdk:programming:getting_started:mariageofencing:fencerule.png?direct |}} \\

### Remove Rules from the service

Our next step is to add mechanisms to remove specified rules from the service. \\
Add add a command handler and list box selection properties to the view model.

```csharp
public ICommand RemoveRuleCmnd { get { return new DelegateCommand(x => RemoveRule()); }}
private void RemoveRule()
{
    var element = _rulesList[SelectedRuleItemIndex];
    _clientFenceRule.DeleteRules(new List`<Guid>` {element.Id});

    NotifyPropertyChanged(() => RulesCollection);
}

public bool IsRuleItemSelected { get { return _selectedRuleItemIndex >= 0 && _rulesList != null; } }
private int _selectedRuleItemIndex;
public int SelectedRuleItemIndex
{
    get { return _selectedRuleItemIndex; }
    set
    {
        _selectedRuleItemIndex = value;
        NotifyPropertyChanged(() => IsRuleItemSelected);
    }
}
```

With corresponding GUI:

```xml
  <ListBox MinWidth="60"
         ItemsSource="{Binding GeoFenceRuleViewModel.RulesCollection}"
         SelectedIndex="{Binding GeoFenceRuleViewModel.SelectedRuleItemIndex}"
         />
  <Button Content="Remove Rule" 
          Command="{Binding GeoFenceRuleViewModel.RemoveRuleCmnd}" 
          IsEnabled="{Binding GeoFenceRuleViewModel.IsRuleItemSelected}" />
```

`<WRAP round box >`
You should now be able to remove selected fence rules from the service!
`</WRAP>`

{{ :maria_gdk:programming:getting_started:mariageofencing:remove_fence_rules.png?direct |Remove selected rules.}}

### Adding conditions to the Fence Rules

In real life, you will probably need to customise the fence rules by specifying conditions for geo shapes and tracks affected by the rule. This is accomplished by including condition xml to the **//GeoFenceConditionXml//** and ***TrackConditionXml*** properties of the **//GeoFenceRuleDef//** rule object.

In the sample code you find examples of how to create single or composite conditions. Include the track layer (//IMariaTrackLayer//) and the draw object layer (//IMariaDrawObjectLayer//) as constructor parameters to the **//GeoFenceRuleViewModel//**

 | Geo Fence Condition | Single    | Affecting selected geo shapes and/or tracks, by adding user fields to the selected shapes/tracks, matching the field condition. | 
 | ------------------- | ------    | ------------------------------------------------------------------------------------------------------------------------------- | 
 | :::                 | Composite | :::                                                                                                                             | 
 | Track Conditions    | Single    | Affecting track with identity hostile.                                                                                          | 
 | :::                 | Composite | Affecting track with identity neutral and speeed > 27.7777 ms                                                                   | 



{{ :maria_gdk:programming:getting_started:mariageofencing:fenceruleconditions-.png?direct |Fence Rule Conditions }}
