# Service Resources

The preparation service depends on external resources. These resources fall into two categories, scripts and binaries. The scripts consists of all scripts that are part of the default installation provided by Teleplan. The scripts are in the form of xml files and will be unpacked to the scripts folder in the preparation service working directory. 
Our scripts rely heavily on external command line tools, most notably the tools that come with the Gdal/Ogr package, but also our own command line tools. These are the binaries that are part of of the default installation. These binaries will be unpacked to the bin folder in the prepration service working directory.

All resources that come prepackaged with the service are included as embedded resources that are unpacked at startup. All files conflicts on the service work directory are resolved by simply replacing the files with the files from the embedded resource.
