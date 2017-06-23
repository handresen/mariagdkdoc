# Installing Maria GDK service addins

*Applies to Maria GDK 2.0 and newer*
## Service addins

The Maria GDK services is organized in a service addin infrastructure. You can read more about this at [MSDN](https///msdn.microsoft.com/en-us/library/bb384200(v=vs.110).aspx) This means that all service addins will run within the single Windows service "TpgServiceHoster".

## Installation

Installation of the service addins and the required hosting mechanism is done with a single setup.exe (Maria GDK Services Suite). 

Progressing through the installation wizard, you will be able to either choose which addins to install (Custom), or install all addins (Complete). In the last step you can choose to change some advanced settings:
{{ :maria_gdk:configuring:servicessuie-advancedsettings.png?450 |}}

*  **Geo data root directory:** The addins will search this directory for geodata on startup. However, if you import map data using Maria Toolbox or the Preparation Service, this data will be stored in ''C:\ProgramData\TeleplanGlobe\MariaGDK\GeoData\Maps''. This can be changed after installation in the *settings.json* file, located at ''C:\ProgramData\TeleplanGlobe\TpgServiceHoster\Config''


* **Default number of service instances:** Several instances of the raster and vector services can be run simultaneously.


* **Common Port number, Register As:** These two values will make up the address on which this computer can be reached by other clients.

All these settings can be changed later in the TpgServiceHoster [settings.json](./configureaddins) file.
