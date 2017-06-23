#  Vector Map Service

The Map Service is dedicated to produce maps from vector data. It reads a proprietary Teleplan Globe map format (m6m) to ensure optimal performance and stable operation.

In the installation configuration, **Map root path** should be set to the folder containing the map files or a [links.xml](maria_gdk/maps/config/links) file to redirect the service to another folder. **Map recursion depth** indicates how many levels down from the map root folder the service will search for maps. **No. of Instances to Run** indicates how many instances of the Map Service will run simultaneously.

If several instances of the service are run, each instance will pick the first available port in the **Service Port Range**. By default the **Service port range** is set to 9050-9099.


{{:maria_gdk:mapservice_installation.png?350|Maria 2012 Map Service - Install Shield Wizard}}

## Command line installation


*  [Run installer using command line](./commandlineinstall)

*  Supported [MSI property arguments](./propertyarguments)
    * TPGMAPROOTPATH
    * TPGCATALOGSERVICEURL
    * TPGINSTANCES
    * TPGRECURSIONDEPTH
    * TPGSERVICEPORTEND
    * TPGSERVICEPORT
    * TPGREGISTERAS
    * TPGLOGFILEPATH
    * TPGMAXLLOGFILES
    * TPGMAXLOGSIZE
