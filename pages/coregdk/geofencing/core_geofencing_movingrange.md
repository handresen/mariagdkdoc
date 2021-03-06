---
title: Track to track geofencing
keywords: geofencing, services, tracks, fences, rules
tags: [geofencing, track]
sidebar: core_geofencing_sidebar
permalink: core_geofencing_movingrange.html
summary: Create moving geo fences.
---

It is possible to create moving geo fences by adding a *moving range* object to the geofencing service and associating it with a track. The moving range will then behave in the same manner as other geo shapes, and can be set up to generate notification events by using rules.

```csharp
var mrd = new MovingRangeDef();
mrd.Id = Guid.Parse("{DCA13C9F-CE36-4B65-8710-313A312B36CC}");
mrd.TrackId = trackId;
mrd.ListId = listId;
mrd.Name = "Simple moving rangering";
mrd.RangeMeters = 1000;
mrd.Fields = new[]
{
  new KeyValuePair`<string, string>`("fencetype", "moving range")
};

_fenceRuleServiceClient.SetRangeObject(mrd);
```
And a mathcing rule:

```csharp
var genericRuleDef = new GeoFenceRuleDef();

var template = new NotificationTemplateDef {
  Heading = "[track.name] [interaction] proximity to"+
    "[shape.track.name] ([shape.fencetype])" };
template.TrackFields.AddRange(new[] { "name" });
template.GeoShapeFields.AddRange(new[] { "track.name", "rangename" });
template.NotificationLevel = NotificationLevel.Low;

genericRuleDef.Id = Guid.Parse("{4BB9385F-E2B3-457B-AEF1-5B77950FF9D8}");
genericRuleDef.NotificationTemplate = template;
genericRuleDef.Interactions = GeoFenceInteractions.Entering;
genericRuleDef.GeoFenceConditionXml = 
  @"`<fieldcondition op=""Eq"" field=""fencetype"" value=""moving range""/>`";

_fenceRuleServiceClient.AddRule(genericRuleDef);

```

### Special considerations

*  Each range object is associated with a single track. If that track is not present, the range ring will not be active

*  A rule that includes the range object must be active, otherwise no notifications will be generated

*  Defining lots of range objects with large ranges has the potential to create very many notifications
   
### Notification template fields

Notification template fields behave the same way for dynamic range objects as for regular shapes, in addition the following replacements are supported:

 | Key                   | Description                                                               | 
 | ---                   | -----------                                                               | 
 | [shape.track.`<field>`] | Refers to trackfield of the track that controls the dynamic range         | 
 | [track.`<field>`]       | Refers to trackfield of the track that enters or leaves the dynamic range | 
 | [shape.`<field>`]       | Refers to field of the dynamic range                                      | 




