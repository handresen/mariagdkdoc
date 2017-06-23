# Web maps overview

`<WRAP center round todo 60%>`
under construction
`</WRAP>`

The Maria GDK supports acquisition and utilization of web distributed cartographic data from external sources. These will commonly be referred to as "web maps". Strictly speaking, we may not necessarily be talking about 'maps' at all, rather other kinds of metadata or related information, geospatial or otherwise. In this context we let "web maps" include:


*  Open Geospatial Consortium (OGC) Web Map Services (WMS), together with the derived standards WMTS and WMS-C  

*  Bing maps from Microsoft

*  Open Street maps

*  Simple OGC Web Feature Services

*  Usage of map catalogs to locate map services

The GDK framework provides mechanisms to allow web maps data to be obtained and displayed in a few simple steps. As for WMS, once the source is defined - usually in terms of an URL, one or several themes and possibly a region of interest - the web map will behave like any other Maria map layer from the clients point of view.

Access to the internet is a requirement.    

See

*  [Web maps service](./webmaps/webmapsservice)\\

*  [Map template](./webmaps/template)\\

*  [Client API](./webmaps/clientapi)\\
