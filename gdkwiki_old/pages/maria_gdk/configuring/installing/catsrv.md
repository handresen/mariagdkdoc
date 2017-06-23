#  Map Catalog Service

The Map Catalog Service holds information regarding all templates in the map folder.

In the application configuration the map root path should be set to the folder containing the actual map files. By default, the **Service port** is set to 9008, hence the service address:
    http://localhost:9008
All the other services (except the Track Service and Location Service) require the address of the Map Catalog Service during installation. If this address is changed from the default value, the change must also be made in each of the other service installations. The address to the Map Catalog Service can also be changed in each service's config file after installation.


{{..:MARIA2012_-_Installing_services_html_986f55a.png?350|Maria 2012 Map Catalog Service - Install Shield Wizard}}

## Command line installation


*  [Run installer using command line](./commandlineinstall)

*  Supported [MSI property arguments](./propertyarguments)
    * TPGMAPROOTPATH
    * TPGRECURSIONDEPTH
    * TPGSERVICEPORT
    * TPGLOGFILEPATH
    * TPGMAXLLOGFILES
    * TPGMAXLOGSIZE
    * TPGLOGAPPENDER
