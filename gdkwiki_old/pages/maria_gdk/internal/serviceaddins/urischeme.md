# Uri Scheme for addin services

The addins really have a three part URI:

*  The base address consisting of hostname and port. This is defined by the hoster application, not by the individual addin.

*  First part of path after the base address. This is unique per addin. An example here for the track service with a hoster that defines a base address of `<nowiki>`http://localhost:9008`</nowiki>` would be `<nowiki>`http://localhost:9008/tracks.`</nowiki>`

*  Subsequent path elements can be used for additional endpoints that an addin exposes.

## Conventions used for endpoints exposed by addins

### First part of path
Exposes the default endpoint with a basic http endpoint, This part has to be unique per addin.
Example:


http://localhost:9008/tracks
```

### Services that exposes a REST web interface

For addins that exposes web interfaces the second part of the path should be **web** and should be configured for automatic format selection:
Example:


http://localhost:9008/tracks/web
```
Configured with endpoint behavior:

```xml
`<behavior name="RestBehaviorAuto">`           
     `<webHttp automaticFormatSelectionEnabled="True" />`
`</behavior>`
```

If desired the service can also expose endpoints with specific return formats.
Examples:


http://localhost:9008/tracks/web/json
```
Configured with endpoint behavior:

```xml
`<behavior name="RestBehaviorJSON">`
   `<webHttp defaultOutgoingResponseFormat="Json"/>`
`</behavior>`
```


http://localhost:9008/tracks/web/xml
```
Configured with endpoint behavior:

```xml
`<behavior name="RestBehaviorJSON">`
   `<webHttp defaultOutgoingResponseFormat="Xml"/>`
`</behavior>`
```

### Services with multiple basic http endpoints

Services that exposes multiple basic http endpoints should expose the most used at the root path described in [First part of path](urischeme#first_part_of_path) then use subsequent path elements for hosting other endpoints. The same scheme [described](urischeme#services_that_exposes_a_rest_web_interface) before should be used for REST versions of these basic http endpoints.
Example from the geo fencing service:


http://localhost:9008/geofencing/rules
http://localhost:9008/geofencing/notifications

http://localhost:9008/geofencing/web/rules
http://localhost:9008/geofencing/web/notifications

http://localhost:9008/geofencing/web/json/rules
http://localhost:9008/geofencing/web/json/notifications

http://localhost:9008/geofencing/web/xml/rules
http://localhost:9008/geofencing/web/xml/notifications
```


