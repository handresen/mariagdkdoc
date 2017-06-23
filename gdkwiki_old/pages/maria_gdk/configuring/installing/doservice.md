#  Draw Object Service

The Draw Object service provides tools for placing and manipulating objects on the map.

The **DatabasePath** indicates the path to the sqlite file that holds the draw objects. This parameter is only accessible from the config file, and the default path is *%TMP%\Maria2012DrawObject\DrawObjectStore.sqlite* . Map symbols defined in MIL-STD-2525C, in addition to several more, are included in the installation. 

In the application configuration the default **Service port** is set to 9004.

{{:maria_gdk:drawobjectserviceinstallation.png?350|Maria 2012 Draw Object Service - Install Shield Wizard}}

## Command line installation


*  [Run installer using command line](./commandlineinstall)

*  Supported [MSI property arguments](./propertyarguments)
    * TPGDBPATH
    * TPGSERVICEPORT
    * TPGLOGFILEPATH
    * TPGMAXLLOGFILES
    * TPGMAXLOGSIZE
    * TPGLOGAPPENDER
