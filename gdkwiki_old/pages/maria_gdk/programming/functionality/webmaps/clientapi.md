# Client API

The purpose of the Maria2012GDK web maps API is to allow available WMS and other web maps resources to be easily integrated in a Maria client application. Interaction with an external source consists of responses to http 'GET' requests from the client. A typical such response would, in case of WMS, be a raster map (or maptile) drawn according to specifications contained in the request.

These specifications consists of two parts - a *content* part and a *geometry* part. A main feature of this client API is to provide an interface for the *content* part, whereas the *geometry* part is taken care of "behind the scene". New requests are issued automatically as needed along with viewport changes.    

## WebMapsClientManager

The WebMapsClientManager maintains client side resource handling for web maps clients and encapsulates their interaction with services as well as other parts of the Maria map system.

The WebMapsClientManager constructor needs instances of
[IMapServiceManager](http://support.teleplanglobe.com/MariaGDKDoc/html/BEC7F48B.htm)
and 
[IMapResources](http://support.teleplanglobe.com/MariaGDKDoc/html/A631E704.htm) as input. The construction, as illustrated below, should be performed after startup at a point where these parameters are available.

```csharp
IMapServiceManager _msm;
IMapResources _mr;
IWebMapsClientManager _wmcm;

//...

_msm = new MapServiceManager(....);
_mr = new MapResources(....);

//...
_wmcm = new WebMapsClientManager(_msm, _mr);
```

The WebMapsClientManager implements the IWebMapsClientManager interface.

## WebMaps client factory

The WebMapsClientFactory implements the IWebMapsClientFactory interface providing constructors for various kinds of web maps clients as listed below:

 | Return type               | Method                                 | 
 | -----------               | ------                                 | 
 | ** IOGCWMSClient **       | CreateOGCWMSClient(string name);       | 
 | ** IOGCWMTSClient **      | CreateOGCWMTSClient(string name);      | 
 | ** IOGCWFSClient **       | CreateOGCWFSClient(string name);       | 
 | ** IOGCCSWClient **       | CreateOGCCSWClient(string name);       | 
 | ** IVirtualEarthClient ** | CreateVirtualEarthClient(string name); | 
 | ** IOpenStreetClient **   | CreateOpenStreetClient(string name);   | 

Once created, for the new client object to be given access to necessary resources and services, it must be assigned to the WebMapsClientManager. Do as follows:

```csharp
IWebMapsClientFactory fac = new WebMapsClientFactory();
//...

IOGCWMSClient newWMSClient = fac.CreateOGCWMSClient("newWMSClientName");
//...
//Assuming _wmcm is a valid IWebMapsClientManager instance

_wmcm.AddClient(newWMSClient);
```


## WebMaps client types

All web maps clients inherit from a common base class WebMapsClient implementing the IWebMapsClient interface. `<link to doc here>`

This class provides common features like timeout settings and error notifications. Although all service- and webrequests are handled asynchronously, a sensible timeout value ensures we will not wait idefinetly for a response. Default is 20 seconds, which is more than enough in most, but not all cases. It depends on the server.

Web communications are prone to error situations. They may of course be due to conditions on the client side. But just as likely the reason may be on the server side, due to factors for which the client has no control. Anyway, in case of an error, the ** OnWebMapsError ** event will be raised for notification with an ** IWebMapsErrorResult **:

 | Member             | Value                                                           | 
 | ------             | -----                                                           | 
 | ** Request **      | string : The submitted request                                  | 
 | ** Message **      | string : An error message                                       | 
 | ** ResponseData ** | byte[] : Raw representation of response data                    | 
 | ** ResponseText ** | string : String respresentation of response data, if applicable | 

In case of a timeout event, there is of course no server response. Only the Message field will be valid, containing the single word 'Timeout'. Otherwise, the ResponseText or ResponseData fields may contain useful hints on how the error situation may be amended and possibly overcome.

### OGC type clients

These clients are built upon concepts and standards specified by the Open Geospatial Consortium
[(OGC)](http://www.opengeospatial.org/standards). A common feature among servers supporting these standards is the existence and provision of a service metadata document stating its capabilities.

Consequently, a typical first request to services of this kind is a query asking for this document. It contains an overview description of the data available from the server, as well as instructions on how to form subsequent requests.

In the Maria2012GDK web maps API, all OGC type clients inherit from OGCWebMapsClient implementing the IOGCClient interface.  `<add link, and a few words on url>`
Request for the capabilities document is issued by calling the ** GetCapabilities() ** method. Upon reception the ** OnCapabilitiesReady ** is raised. Do as follows:

```csharp
//Assuming newWMSClient has been created like above

newWMSClient.OnWMSCapabilitiesReady += OnWMSCapabilityReady;

newWMSClient.Url = "http://some wms server";
newWMSClient.GetCapabilities();

//....

//..and implement the handler for the event
void OnWMSCapabilityReady(object sender, OGCWMSCapabilities ogcWMSCap)
{
//The sender will be the client object.
//The ogcWMSCap will be an instance of OGCWMSCapabilities containing parsed elements of the received capabilities document. 
}
```


Another useful feature of the IOGCClient interface is the ** RunCustomRequest(string strRequest) ** method, where the string parameter ** strRequest ** denotes the full URI of a request or query, just as it would appear in a web browser's address line. The various OGC type clients descibed in more detail below all provide shortcut methods to form class specific requests. But since built-in transistions take place behind the scene, an explicit alternative may still be handy for for debugging or test purposes. Upon reception of a server response, either the already mentioned ** OnWebMapsError ** or an ** OnCustomDataReady ** event will be raised. The handler for the latter should look like:

```csharp
void OnCustomDataReady(object sender, byte[] data)
{
//The content and interpretation of the data parameter depend on the request issued.
}
```

#### Authentication / Credentials

Web services may employ access control demanding a client to submit credentials for authorization.

##### Http Authentication

Http authentication require requests to be tagged with a valid user-password combination. 
If the IOGCClient ** UseCredentials ** flag is set to true, the client API will try to pass the values of ** User ** and ** Password ** properties accordingly. Otherwise, these properties are ignored.

##### Baat Authentication

The Norwegian Mapping Authority employs a different kind of authentication control, called BAAT. The details of this mechanism will not be explained here, except that it relies on a valid 'ticket' - obtained from a dedicated server - being submitted along with every request. To access any such service, the value of the ** TicketService ** property must match the name given to that service within the BAAT system. Otherwise this property ** must ** be left empty. (See also the MariaGDK WebMapsServer documentation on further info, as BAAT ticket administration is handled on the server side)   
####  

Authentication failures will be sent back to the client and the application is notified through the OnWebMapsError event.

#### List of Properties

For a full list of properties and methods, see the GDK documentation `<link>`. A few of them are explained in somewhat more detail here.

##### Url

The server web address - uniform resource locator - string. Without any parameters unless they are necessary for initial entry point location on the server side, or it is stated in the ** Requests ** section of the server capabilities. 

#####  Version

This property is a string formed by three digits separated by points - like ** 1.3.0 **. All requests must contain a valid version tag as one of its parameters. The only exception being ** GetCapabilities **. If it is ommitted from that request, the server will send metadata for the highest version it supports.

##### ExtentLayoutOverride

Optional parameter to facilitate non-standard formatting of spatial boundingboxes in requests. When necessary, it may be used to override the default version dependent standard. Default value is ** None **, meaning that no such override should take place.

 | Value         | Description                                                                                              | 
 | -----         | -----------                                                                                              | 
 | ** None **    | No override. Use standard version dependent formatting                                                   | 
 | ** SRS_X_Y ** | Use the ** SRS ** tag to denote coordinate systems. Make ** X ** or ** longitude ** the first coordinate | 
 | ** SRS_Y_X ** | Use the ** SRS ** tag to denote coordinate systems. Make ** Y ** or ** latitude ** the first coordinate  | 
 | ** CRS_X_Y ** | Use the ** CRS ** tag to denote coordinate systems. Make ** X ** or ** longitude ** the first coordinate | 
 | ** CRS_Y_X ** | Use the ** CRS ** tag to denote coordinate systems. Make ** Y ** or ** latitude ** the first coordinate  | 































#### OGCWMS

OGCWMS is the most commonly used protocol to receive cartographic maps from a web maps server. For a full description of this protocol, refer to the [ OGC documentation found here ](http://portal.opengeospatial.org/files/?artifact_id=14416).

Raster map tiles are obtained in response to ** GetMap ** requests. These requests consists of a KVP formatted string, containing two kinds of information - *content* and *geometry*. In this context, the *geometry* - part - except for possible initiation hints -  will be taken care of dynamically by the MariaGDK map engine, in accordance to normal navigation operations like pan & zoom. That leaves the *content* - part to be specified - mainly in terms of selected ** Layers ** and  ** Styles **.

 

##### Layers

An array of selected layer *names* as they appear in the server capabilities ** Layer ** section. Be sure not to confuse layer *name* with layer *title*, as the latter is just a human intelligible description. All layers have a title. But if it has a name as well, that name denotes a selectable theme and may appear as part of a ** GetMap ** request.

##### Styles

An optional array of *styles* selected among the ones listed in the server capabilities ** Styles ** sections. If present, the number of style elements in this array must match the number of layer names submitted in the Layers list. And each style must be applicable to the corresponding layer element.

##### Format

An optional MIME-type format descriptor. Acceptable values may be found from the server capabilities ** Formats ** sections. Default value is ** image/png **.

##### CRS

A descriptor for the coordinate system to be employed - in requests and hence also for returned images. Acceptable settings, commonly in terms of ** EPSG codes **, are listed in the server capabilities ** CRS ** or ** SRS ** sections. If not present, the client will try to negotiate or deduce a suitable value automatically. However, both server performance and the perceived quality of returned images may depend on a sensible selection being made.
Example: If polar regions are to be shown, it will not be wise to select a geographical (Lat,Lon) or Mercator type CRS.

##### Annex

Any KVP formatted string. If nonempty, it will be literally added to the request. It makes it easy to include any combination of request parameters not explicitly available, including ** Dimension ** or ** Frames ** - like *Time* or *Elevation*. The Annex may also be used to supply conditional- or custom style instructions, like an ** SLD ** script. It may be used to provide any auxiliary information in the request assumed to be intelligible on the server side.

##### Urlfull

If nonempty, this overrides the settings above. It will be assumed to contain a full KVP formatted request. Except those specifying the *geometry* part, that is. Those will be dynamically calculated and added to comply with viewport navigations.

##### RegionOfInterest

Optional 'region of interest' in geographical coordinates. When set to a valid GeoRect `<link>` struct, that region
- transformed to the selected coordinate system (see CRS above) - will serve as a base tile. It overrides any bounding box information obtainable from the capabilities. It thus makes the availability of such information - often missing for many supported coordinate systems - less crititical. 
For this reason, it should be used for standard WMS only, not for WMS-C where a tile structure has to match
those supported by the WMS server exactly.

##### ExtentOverride

An optional OGCBoundingBox object. If specified (not null), it will be used as a base tile, and hence override any information supplied elsewhere from which the base tile otherwise would be constructed. That includes the RegionOfInterest parameter above, and all info or guidelines that might be found or deduces from the server capabilities. Hence it should be set with some caution, and only upon positive *knowledge* of what would serve as a good basetile for the coordinate system in question.

#### OGCWMS Methods

##### Update()

In addition to ** GetCapabilities **, any OGCWMS server must support at least one command: ** GetMap **. A properly formatted GetMap request along with suitable parameters issued from, say, the address line in a web browser, would yield a map tile image according to the specifications entered.

In our case, the MariaGDK map engine takes care of issuing GetMap requests dynamically as needed. It will supply the *geometry* part of each single request. In addition, it will organize received map tiles and finally reproject, scale and translate those needed to fill the current display viewport.

So, there is no GetMap() method here. Instead, there is the ** Update() ** method which tells the MariaGDK map engine that the desired *content* part of a WMS raster layer is completely specified and it may start issuing requests and collecting tiles.

If the *content* part is changed in any way, for instance by adding or removing theme layers, another call to ** Update() ** is needed for these changes to take effect. Likewise, for the base tile to change, for instance due to a different preferred coordinate system ( **CRS** ) being selected, the **Update()** method must be called.

Example:

```csharp
//Assuming wmscl is an OGCWMS client

//...
wmscl.Layers = new string[] { "Oceans", "Countries", "Water", "Roads" };
//...
//Other settings 
//None taking effect until
wmscl.Update();
```
                             
##### GetFeatureInfo

##### GetLegendGraphic

##### DescribeLayer



                             















































#### OGCWMTS

#### OGCWFS

#### OGCCSW

### Microsoft Virtual Earth (Bing) clients

### Openstreet clients


