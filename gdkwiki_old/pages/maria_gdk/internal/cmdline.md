# Maria GDK Command line tools

The Maria GDK contains a few command line tools that may be useful during development and production of data.

This page serves as an index with links to further documentation of these tools. 

## Data production tools

These tools are mainly used for converting or generating data for use in the Maria Services.

### Raster production tools


*  *Source:* Maria Qt project: `<MariaRoot>`\Native\Maria.pro'', Maria2012GDK Solution: `<MariaRoot>`\Tools\TileMapBuilder''

*  *Compiled Binaries:* ''\\tpg-geostore\misc\bin\MariaNativeCmd''

*  //[TileMapBuilder](maria_gdk/maps/config/raster/tilemapbuilder)//

*  //[GeoPackage tools](maria_gdk/maps/config/raster/maria_geopackage)//

### Vector map production tools


*  *Source:* Maria project: `<MariaRoot>`\Native\maptools''

*  *Compiled Binaries:* ''\\tpg-geostore\misc\bin\vectortools''

*  //[Vectortiles](maria_gdk/maps/convertmap/vector_tiles)//

*  //[M6M indexing](maria_gdk/maps/convertmap/indexing)//

*  //[M6M indexing](maria_gdk/maps/convertmap/indexing)//

*  *Extractmapinfo*

## Service utilities and monitoring

These tools are used to check status of services, to perform maintenance tasks and to test service operation.

### TrackInfo


*  *Source:* Maria project: `<MariaRoot>`\Src\Tools\TrackInfo''

Check status of tracklist, add and delete lists, performance testing. Run without arguments for help.

### MapCatalogClient


*  *Source:* Maria project: `<MariaRoot>`\Src\Tools\MapCatalogClient''

Check status of map catalog client. List registered maps and services. Run without arguments for help.

###  ServiceManagementTool

*  *Source:* Maria project: `<MariaRoot>`\Src\Hosts\AddInHosting\ServiceManagementTool''

Control and query against services running in the addin hosting mechanism.

Example:
*tpgsc show -i*

`<file>`Name: Catalog Service Add In
Description: Hosts the Map Catalog Service as an add in.
Version: 1.4.0.0
Uri: http://tpg-mve-7040:9008/catalog
Started time: 21.10.2016 11.14.55
State: Running
Id: 08727704-9590-4646-8dae-2a1bb3c52ec1

Name: Vector Map Service Add In
Description: Hosts the Vector Map Service as an add in.
Version: 1.4.0.0
Uri: http://tpg-mve-7040:9008/vectormap/62086ebf-90ba-4f08-b33f-3c4121740d0e
Started time: 21.10.2016 11.14.55
State: Running
Id: 9f451338-0e0a-4af1-9bac-60f8244532ed

Name: Raster Map Service Add In
Description: Hosts the Raster Map Service as an add in.
Version: 1.4.0.0
Uri: http://tpg-mve-7040:9008/rastermap/97ceaeb4-874d-4fd2-9efa-6f4f6eeaa900
Started time: 21.10.2016 11.14.55
State: Running
Id: 6a36c351-92ea-4601-82b8-280ee68236ff`</file>`

#### Options:

    show         Displays info. For options run show --help.

    configure    Configures persistent options for the tool.For options run
               configure --help.

    kill         Kills a service using the value supplied to the as search
               string. If multiple services are found, user will be prompted to
               choose. For options run kill --help.

    restart      Restart a service using the value supplied to the as search
               string. If multiple services are found, user will be prompted to
               choose. For options run restart --help.

    start        Instantiates an addin using the value supplied to the as search
               string. If multiple services are found, user will be prompted to
               choose. For options run start --help.

    search       Searches for available add ins or instances. For options run
               search --help.

    rescan       Scans for new addins. For options run search --help.


Use --help to show more detailed help for each option.

Configure will create a file (service.config) containing the address of the management service. Specify the address using -c. If this file doesn't exist -c option needs to be specified for each run.

Example:
tpgsc configure -c http://localhost:9008


