#  Web Map Proxy Service

The Web Map Proxy Service is a proxy service for Web Map Services. The service is routing request and responses for WMS tiles from the client.

In the application configuration the default **Service port** is set to 9032.

{{:maria_gdk:wmsserviceinstallation.png?350|Maria 2012 Web Map Proxy Service - Install Shield Wizard}}
### Proxy configuration

The Web Map Proxy Service can be forced to connect to a Web Service through a proxy service. These settings are currently not available in the installer and must therefor be set directly in the application's configuration file.
Proxy settings are:

*  address: proxy server url

*  user: In case the proxy server demands credentials

*  password: In case the proxy server demands credentials

*  bypassonlocal: whether to bypass the proxy server for local url

*  Bypass address: (0 …*) url that do not use the proxy server  
Example from application configuration file:

```xml
`<configuration>`
  `<configSections>`
    ...
    `<section name="proxySettings" type="TPG.GeoFramework.WebMapsService.ProxyConfiguration, TPG.GeoFramework.WebMapsService" />`
  `</configSections>`
  ...
  `<proxySettings>`
    `<proxy address="http://myproxyexample.com" user="" password="" bypassOnLocal="true" />`
    `<byPass address="http://a.b.c" />`
    `<byPass address="http://d.e.f" />`
    `<byPass address="http://g.h.i" />`
  `</proxySettings>`
...
`</configuration>`
```

Example from the configuration file for addins:

```javascript
"WmsSettings": {
        "ProxySettings": {
            "Address": "",
            "User": "",
            "Password": "",
            "BypassOnLocal": "true",
            "BypassAddresses": [ "http://a.b.c", "http://d.e.f", "http://g.h.i" ]
        },
        "TicketServerSettings": {
            "TicketServerUri": "ticketserviceurl.com",
            "TicketServices": [
                {
                    "ServiceId": "someId",
                    "User": "MyUserName",
                    "Password": "MyPassword"
                }
            ]
        }
    }
```
## Command line installation


*  [Run installer using command line](./commandlineinstall)

*  Supported [MSI property arguments](./propertyarguments)
    * TPGCATALOGSERVICEURL
    * TPGRECURSIONDEPTH
    * TPGSERVICEPORT
    * TPGREGISTERAS
    * TPGLOGFILEPATH
    * TPGMAXLLOGFILES
    * TPGMAXLOGSIZE
