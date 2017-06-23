# Service web APIs

Several of the services in MARIA GDK expose web APIs. These can be used to interface with the services from clients not using WCF. The web services do not typically expose the same level of functionality as the WCF-interfaces. In particular, the high performance [protobuf](https///code.google.com/p/protobuf/) interfaces are not available from the web APIs.

As a main rule, the service call addresses are structured like this:\\
%%http://`<host>`:`<port>`/`<service name>`webservice/[json/|xml/|`<none>`]`<web method>`%%, for instance:\\
[http://localhost:9003/trackwebservice/json/status](http://localhost:9003/trackwebservice/json/status)

For http GET, the content type is specified using [/json/](http://json.org/) or /xml/ in the address as seen above. http GET requests can be entered directly in a browser, and the result will be displayed as plain json/xml.

For http POST/PUT/DELETE, use a web debugging tool, for instance [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler).

Many of the services uses [data contracts](http://msdn.microsoft.com/en-us/library/ms733127(v=vs.110).aspx) in .net to define input types and return types. Output generation and input parsing in json and xml-formats is handled by the data contract serializers.


