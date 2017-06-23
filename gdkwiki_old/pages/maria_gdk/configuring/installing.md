#  Installing Maria GDK services

*Applies to Maria GDK 1.5 and older*
#  Overview

This document provide guidelines for installing and configuring the services in Maria 2012 Geo Development Kit. Maria 2012 consists of a set of services, providing information to one or several map application clients. This guideline includes the nine services available, but the required services are a subject to the map application.
{{ :maria_gdk:maria_2012_environment.png?400 |Maria 2012 environment}}


 ======  Windows Installer Package ======

The services are built as either a Windows Installer Package (.msi) or an executable (.exe) InstallShield Wizard. The .exe file will check system requirements and install dependencies such as .NET framework, whereas the .msi will only install the service itself.

It is recommended to use the .exe file in order to meet the current requirements.

#  Installing the services

##  Default values

By default the installation path is set to *C:\Program Files (x86)\TeleplanGlobe\* for all the services, this is however a subject of choice. It is possible to change the default service port, although this requires the user to change the config files or during installation on several services to match the new service port given.

The installers allow the user to replace the default [hostname] (automatically resolve hostname on start-up) to a fixed host name, ip or alias. The property may also be set as an msi argument:
    //Command line
    msiexex.exe ...... TPGREGISTERAS=AliasName 
    //resulting URI template
    "http://AliasName:[port]"
This applies to map related services. Please see the individual services for a list of supported properties.

[Map Catalog Service](./installing/catsrv)\\ 
[Map Service](./installing/mapsrv)\\ 
[Raster Map Service](./installing/rasmapsrv)\\ 
[Map Cache Service](./installing/cacsrv)\\ 
[Propagation Service](./installing/propsrv)\\ 
[Track Service](./installing/trcsrv)\\ 
[Location Service](./installing/locsrv)\\ 
[Web Map Proxy Service](./installing/webmapsrv)\\ 
[Draw Object Service](./installing/doservice)
#  Service configuration

In each service installation directory, there is a file whose name ends with "`<ServiceName>`.Service.exe.config". This file contains the settings specified during the installation. It is therefore possible to change the settings post installation by editing the config files. 
`<WRAP center round important 60%>`
Editing configuration files incorrectly may cause the service to fail.
`</WRAP>`

