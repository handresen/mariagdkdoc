# Configuring the Maria GDK service addins

The Maria GDK service addins are configurable through *settings.json*, which is located at ''C:\ProgramData\TeleplanGlobe\TpgServiceHoster\Config'', and which can be edited with a regular text editor.
The file must be saved and the TpgServiceHoster service restarted for changes to take effect.

## GeneralSettings

*  **ServicePort:** The port on which this computer can be reached from other clients

*  **RegisterAs:** The hostname on which this computer can be reached from other clients. Use [hostname] to automatically resolve uri to local machine name (f.ex. http://[hostname]:9008).

*  **LogPath:** The path where the logfiles of all service addins are stored

*  **MaxLogFiles:** The maximum number of old logfiles to keep from each service addin

*  **MaxLogSize:** The maximum size of each logfile
## MapCatalogSettings

*  **DiscoveryEnabled:** If set to true, the map catalog can be discovered using WCF UDP discovery

## MapSettings

*  **MapCachePath:** Path where the Cache service addin will store cached maps.

*  **MapCacheMaxBytesInMemory:**

*  **MapCacheDiscUsageThreshold:** If the disc of the computer is x% used, the cache service should stop writing to the database.

*  **CatalogServiceUri:** The address to the Catalog service addin. Use [hostname] to automatically resolve uri to local machine name (f.ex. http://[hostname]:9008).

*  **RasterServiceInstances,VectorServiceInstances:** Number of simultaneous instances of the Raster and Vector service addins.

*  **RasterMosaicParams:**
    * **maxMemorySize:** The maximum amount of memory allowed for GDAL-based rendering (raster formats other than geopackage, MBTiles, QM2 and ECW). 
    * **maxDatasetAge:** Maximum number of seconds to keep a dataset in memory since it was last used. Default is 0, which means keep it indefinitely.
    * **multiThread:** Use multithreading when reprojecting/tiling datasets. Only relevant for GDAL-based data sets.
    * **useIndexFiles:** (true/false) - Use index files to improve startup time for raster services with very large datasets. Default: true
    * **indexFilePath:** Directory for index files. Default: ''%ALLUSERSPROFILE%\\TeleplanGlobe\\MariaGDK\\MapIndex''

*  **Recycle:** Recycling of map services.
    * **Mode:** The type of recycling. Disabled - no recycling, Specific - recycles the services at a specific time and days, Interval - recycles the services at a time interval. 
    * **Days:** The number of days between recycles for Specific mode.
    * **Time:** The time of the day to recycle for Specific mode.
    * **Interval:** The recycle interval/time-span for Interval mode.

*  **M6MParams:**
    * **s52RootPath:** Location of s52 symbol files utilized by the Vector service addin.

## SymbolServiceSettings

*  **SymbolServiceConfigDir:**

*  **SymbolServiceProviderConfigName:**

*  **DiscoveryEnabled:** If set to true, the map catalog can be discovered using WCF UDP discovery
## MapPreparationSettings

*  **MapPreparationWorkDir:** Folder to store Map Preparation scripts and tools, as well as temporary files.

*  **MapPreparationMapDir:** Folder to store completed map packages

*  **JobMaxLife:** (hh:mm:ss) Maximum amount of time a Map Preparation job is allowed to exist.

*  **DiscoveryEnabled:** If set to true, the map catalog can be discovered using WCF UDP discovery
## TemplateSettings

    * **DiscoveryEnabled:** If set to true, the map catalog can be discovered using WCF UDP discovery               
    * **DataSources**
      * **IsSavePath:** (true/false) Use the path below to save template files
      * **Path:** The path to search for template files
      * **RecursionDepth:** Specifies how deep in the folder structure to look for template files

## RasterMapPackagesSettings

*  **DataSources**
    * **IsSavePath:** (true/false) Use the path below to save raster map package files
    * **Path:** The path to search for raster map package files
    * **RecursionDepth:** Specifies how deep in the folder structure to look for raster map package files
## VectorMapPackagesSettings

*  **DataSources**
    * **IsSavePath:** (true/false) Use the path below to save vector map package files
    * **Path:** The path to search for vector map package files
    * **RecursionDepth:** Specifies how deep in the folder structure to look for vector map package files
## WmsSettings

*  **ProxySettings**
    * **Address:** If WMS requests must go through a proxy, enter its address here.
    * **User:** The username for the proxy
    * **Password:** The password for the proxy
	
## TrackSettings

*  **DiscoveryEnabled:** If set to true, the map catalog can be discovered using WCF UDP discovery

## GeoFencingSettings

*  **GeoFencingLogFileType** The log file type. Typically xml or json

*  **GeoFencingLogFileName** The log file to use

*  **DiscoveryEnabled:** If set to true, the map catalog can be discovered using WCF UDP discovery
    
## LocationSettings

*  **LocationDefaultDatabase** Default location database to use

*  **LocationCacheSize** Cache size to allow

*  **Datasources** Datasource to look for location databases

*  **DiscoveryEnabled:** If set to true, the map catalog can be discovered using WCF UDP discovery





