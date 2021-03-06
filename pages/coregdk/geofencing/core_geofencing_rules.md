---
title: Geofencing rules
keywords: geofencing, rules
tags: [geofencing, track, drawobject, interface]
sidebar: core_geofencing_sidebar
permalink: core_geofencing_rules.html
summary: Defining geofencing rules.
---

Rules define the circumstances that are required in order to trigger a geofencing event. In addition, a rule contains the information used to create the contents of the geofencing event. 

{% include image.html file="./core/geofencing/geofencingruleexecution.png" %}

Rules are added through [IGeoFencingRuleServiceBase](http://support.teleplanglobe.com/MariaGDKDoc/html/36DA80D0.htm) using the
[AddRule method](http://support.teleplanglobe.com/MariaGDKDoc/html/E6F88825.htm):

```csharp
GeoFenceRuleDef genericRuleDef = new GeoFenceRuleDef();
genericRuleDef.Id = Guid.Parse("{4BB9385F-E2B3-457B-AEF1-5B77950FF9D6}");
genericRuleDef.Actions = new List<ActionBaseDef> { 
  new NotificationActionDef { NotificationTemplate = template } };
genericRuleDef.Interactions = 
  GeoFenceInteractions.Entering | 
  GeoFenceInteractions.Leaving |
  GeoFenceInteractions.Crossing;
genericRuleDef.GeoFenceConditionXml = 
  @"<fieldcondition op=""Eq"" field=""TriggerCode"" value=""1""/>";

fenceRuleServiceClient.AddRule(genericRuleDef);
```



### Interactions

Interactions can be "Entering", "Leaving" or "Crossing". When defining a rule, one or more interaction types can be combined using flags/bitwise or.

#### "Entering" and "Leaving"

A track is considered "Entering" a shape if the previous position was outside of the object and the current position is inside the object. "Leaving" occurs when previous position was inside the shape, and current position is outside of the shape. "Entering" and "Leaving" are only defined for objects with an area, it does not apply to line type shapes without a buffer.

{% include callout.html content="*  **Entering** occurs only when both current and previous positions are defined. If a track suddenly appears within an area, there is no way to determine if or when it has actually entered the area<br/>

*  The same goes for **leaving**" type="info" %}


#### "Crossing"

A track is considering "Crossing" a shape if the geodesic line between previous and current position intersects an odd number of line segments of the shape. "Crossing" is only defined for line type objects, and is not valid for objects with an area. By monitoring "Entering" and "Leaving" type of interactions for a single track, a client can easily implement "Crossing" type events for shapes with area.


### Filters

Filters in a rule are used to connect a rule to a set of shapes and tracks. By default a rule applies to all tracks and shapes in the system. The example above contains the following filter:

```csharp
genericRuleDef.GeoFenceConditionXml = 
  @"<fieldcondition op=""Eq"" field=""TriggerCode"" value=""1""/>";
```
This filter causes the rule to only consider shapes with the field "TriggerCode" set to "1". This allows setup of a single, generic rule that is only triggered for selected shapes.\\ \\
These filters are defined in the same way as track/draw object styling filters. Complex filters can be created by using nested conditions:

```xml
<compositecondition op="Or">
  <speedcondition value="0kts" op="Eq"/>
  <speedcondition value="" op="Undefined"/>     
</compositecondition>
```

{% include callout.html content="Rules are very loosely connected to tracks and geo shapes. In order to create 1:1 connections to either a track or a shape, the corresponding filter must reference a unique property or a unique combination of properties of the track or shape, for instance the track id or object id." type="info" %}

### Templates

Templates define the contents of the report/event that is generated.

```csharp
var template = new NotificationTemplateDef();
template.Heading = "[track.name]([track.type]) [interaction] [shape.Name]";
template.TrackFields.AddRange(new[] { "name", "type", "identity" });
template.GeoShapeFields.AddRange(new[] { "Name" });
```

In this example, the template generates a report heading by referencing fields in the track and shape that generated the event. This allows for generic templates. The event above might generate the event heading: "M/F Petter Wessel entering Langesund". Any text of the type **[track.`<field>`]** is replaced with the responding track field. **[shape.`<field>`]** is replaced with the contents of the corresponding shape field. Only fields included in the template TrackFields and GeoShapeFields can be referenced in this manner. In the above example, the following statement allows references to the track fields [track.name], [track.type] and [track.identity]:

```csharp
template.TrackFields.AddRange(new[] { "name", "type", "identity" });
```

Note that defining these fields in the template requires advance knowledge of the field names actually included in the tracks with the exception of the following, predefined values:

 | Key              | Description                                                  | 
 | ---              | -----------                                                  | 
 | [interaction]    | Interaction type, one of "Entering", "Leaving" or "Crossing" | 
 | [level]          | Notification level, "Low", "Medium" or "High"                | 
 | [time]           | UTC time on the format HH:mm:ssZ, 12:23:34Z                  | 
 | [datetime]       | ISO 8601 utc datetime, 2014-10-28T11:56:58Z                  | 
 | [track.course]   | Track course in degrees, 0 is north                          | 
 | [track.speed]    | Track speed in m/s                                           | 
 | [track.speedkts] | Track speed in knots                                         | 

#### Predefined notification values

By design, the notifications are mostly static and cannot be changed. For instance, the track name or the interaction type cannot be changed. However, various user interactions can take place. Example are acknowledging a notification or marking a notification as read. This is done by appending user data messages to the notification. These messages are composed of a user identifier, a field key and a field value. The template can add initial user data messages to the notification. These will be added using the user id "system". More on this in [Notifications](./core_geofencing_notifications.html)

```csharp
private void AddRuleWithPredefinedValues()
{
  GeoFenceRuleDef r = new GeoFenceRuleDef();

  var template = new NotificationTemplateDef { Heading = 
    "[track.name] [interaction] [shape.Name]" };
  template.TrackFields.AddRange(new[] { "name" });
  template.GeoShapeFields.AddRange(new[] { "Name" });
  template.NotificationLevel = NotificationLevel.Medium;
  
  // Any notification generated based on this rule will contain
  // userdata region=national for user "system"
  template.PredefinedValues.Add(new Tuple`<string, string>`("region","national"));

  r.Id = Guid.Parse("{807821B5-7432-4B23-BBF6-1F3251EC497D}");
  r.Actions = new List`<ActionBaseDef>`{
    new NotificationActionDef { NotificationTemplate = template }};
  r.Interactions = GeoFenceInteractions.Entering | GeoFenceInteractions.Leaving;
  r.GeoFenceConditionXml = 
    @"`<fieldcondition op=""Contains"" field=""Name"" value=""territorialområde""/>`";

  _ruleServiceClient.AddRule(enteringAndLeavingRuleDef);
}
```
The following filter definitions include notifications generated with the rule above:

```xml
`<fieldcondition field="userdata/system/region" op="Eq" value="national"/>`
`<fieldcondition field="userdata/*/region" op="Eq" value="national"/>`
```



