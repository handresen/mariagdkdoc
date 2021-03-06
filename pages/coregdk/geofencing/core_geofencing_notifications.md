---
title: Geofencing notifications
keywords: geofencing, notifications, rules
tags: [geofencing, track, drawobject, interface]
sidebar: core_geofencing_sidebar
permalink: core_geofencing_notifications.html
summary: Adding notifications when tracks interact with objects.
---

Notifications are added by the geofencing core when tracks interact with geo shapes according to predefined rules. Clients can read these notifications as well as append specific types of information to the notifications. Notifications are accessed through [INotificationHandlingServiceBase](http://support.teleplanglobe.com/MariaGDKDoc/html/754E0C07.htm).

### Reading notifications

Notifications are read using the service method

```csharp
NotificationQueryResult GetNotifications(string notificationConditionXml, 
                                         int lastKnownGeneration);
```
All notifications matching filter, with generation newer than *lastKnownGeneration* are retrieved from the service.

{% include callout.html content="The GeoFencing.Core library contains *NotificationStore* which can be used for a client side storage of notifications. The service tester app uses this technique." type="info" %}

#### Filtering

The filtering syntax used in notification filtering, is the same as used for for geofencing rules and for general track/draw object styling. Using "fieldcondition" in combination with "compositecondition" allows creation of powerful notification filters:

```csharp
var result=notificationClient.GetNotifications(
  @"<fieldcondition op=""Contains"" field=""heading"" value=""Oslo"" />", -1);
```
The condition xml-string can also be generated by creating the conditions programatically, and then using the WriteCondition-member of ConditionXmlParser:

```csharp
FieldCondition c=new FieldCondition("track.somefield", "42", FieldOperator.Contains);
string xml=(new ConditionXmlParser()).WriteCondition(c);
var answer=notifications.GetNotifications(xml, -1);
```


Note that "fieldcondition" for notifications can be used with the following predefined field codes:

 | Field key         | Description                                                                                         | 
 | ---------         | -----------                                                                                         | 
 | id                | Notification guid                                                                                   | 
 | interaction       | *Entering*, *Leaving* or *Crossing*                                                           | 
 | trackid           | Track identifier, unique within a tracklist                                                         | 
 | tracklistid       | Tracklist identifier                                                                                | 
 | geoshapeid        | Geo shape identifier, unique within a geo shape list                                                | 
 | geoshapelistid    | Geo shape list identifier                                                                           | 
 | geofencecondition | XML-representation of the geo shape/geo fence filter                                                | 
 | trackcondition    | XML-representation of the track filter                                                              | 
 | heading           | Textual header of the notification                                                                  | 
 | description       | Textual description of the notification                                                             | 
 | level             | Notification level, *Low*, *Medium* or *High*                                                 | 
 | userdata          | Refers to userdata for the notification. Format: "userdata/`<user>` or */`<key>`"                       | 
 | track.`<field>`     | Refers to track field. Note that only fields defined in the notification rule can be referenced     | 
 | shape.`<field>`     | Refers to geo shape field. Note that only fields defined in the notification rule can be referenced | 


Condition interfaces can be found here: [ICondition](http://support.teleplanglobe.com/MariaGDKDoc/html/904AEA38.htm). Use [Condition factory](http://support.teleplanglobe.com/MariaGDKDoc/html/96C64B09.htm) to create concrete conditions.

##### Service notification filtering

Service filters must be written using xml. Some samples:

```xml
<compositecondition op="And">
    <compositecondition op="Or">
      <fieldcondition op="NEq" field="level" value="Low" />
      <agecondition value="20.0" op="Lt" />
    </compositecondition>
    <fieldcondition op="NEq" field="userdata/*/state" value="acknowledged" />
  </compositecondition>
```

This filter extracts notification state not set to acknowledged (for any user). Messages older than 20 seconds with low notification level are not retrieved.

##### Client side filtering

After retrieving the notification, the client can perform further filtering if required. This can be done using conditions directly:

```csharp
ICondition cond=new FieldCondition("SomeFieldName", "20", FieldOperator.Lt);
bool satisfied=cond.IsSatisfied(null, someNotification.GeoObjectAdapter);
```

One usage is for removing old notifications from the client:

```csharp
// Assume client store containing notifications,
// List`<NotificationDefWrapper>` notifications
var ageCondition=new AgeCondition { Operator = FieldOperator.Gt, Age = 60 }
notificationClient.RemoveAll(a=>ageCondition.IsSatisfied(null,a.GeoObjectAdapter);
```

### Notification user data

User data can be added from the template using [SetNotificationUserData](http://support.teleplanglobe.com/MariaGDKDoc/html/6BEFFB42.htm).

```csharp
	notificationClient.SetNotificationUserData(id,
                new UserDataDefinition { Owner = "myself", 
                                        Key = "somefieldid", 
                                        Value = "1234" });
```

### Logging

Notifications can be logged and restored from file. Use [SetNotificationLogInformation](http://support.teleplanglobe.com/MariaGDKDoc/html/729AAA13.htm) to specify filetype ("xml" or "json") and filename.

Notifications are continously written to file as they are retrieved. It is also possible to restore old notification-logs using [RestoreNotificationLog](http://support.teleplanglobe.com/MariaGDKDoc/html/AEFDD602.htm).




