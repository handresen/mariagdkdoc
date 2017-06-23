# Script attributes

A script must be defined properly so that it is connects to the correct file types. This is done by specifying conditions for the dropped file(s) like file extension, raster type, elevation data etc.

The script file must contain three main sections:

*  **The header** which defines the type of script and resulting output type

*  **Conditions**

*  **Tools** defined in the order of execution


# The header

An example of a valid header is as follows:

```xml
  `<script servicetype="RasterService" name="RasterToGeoPackageWithNoData" displayname="Converts raster to geopackage" scripttype="DeployToService" outputtype="GeoPackageWithNodata" resultstepid="_deployedmapresults">`
```
    
The header attributes are documented below.
### servicetype

This attribute identifies which map preparation service this script defines.

```xml
  `<script servicetype="VectorService" ... >`
```
    
Valid values are:

*  **VectorService** - Map service producing rasterized vectordata, styled label data and feature queries

*  **RasterService** - Map service producing rasterdata based on standarized raster formats

*  **CacheService** - Map service combining data from multiple map services

*  **WmsService** - Web map service

*  **PropagationService** Propagation service for caculating radio / radar coverage



### name

The script's **name** and is used for logging purposes when running the script.

```xml
  `<script ... name="VectorTilesDataScript" ... >`
```
    
### displayname

If defined, the **displayname** will be used instead of the **name** attribute when running the script. 

Note: this attribute is not honored by all tools.

```xml
  `<script ... displayname="Converts raster to geopackage" ... >`
```
    
### scripttype

Defines the script type.

```xml
  `<script ... scripttype="DeployVerifiedResults" ... >`
```

Valid values are:


*  **DeployToService** - Prepare data and deploy to service.

*  **InformationExtraction** - Will only analyze data and provide data.

*  **ExportData** - Script will export data, but not deploy to service.

*  **DeployVerifiedResults** - Script will deploy data that has been verified by user.
### outputtype

Defines the script's output type.

```xml
  `<script ... outputtype="VectorTiles" ... >`
```
    
Valid values are:  
 

*  **M6M** - Vector M6M data.

*  **VectorTiles** - Vector tiles data.

*  **OriginalRaster** - The source raster is not changed as output.

*  **GeoPackage** - Raster geopackage.

*  **GeoPackageWithNodata** - Raster geopackage with a nodata transform.

*  **Json** - Json string.

### resultstepid

This attribute defines which step the client can read back result from. For most scripts, this will be the final step in the script.

```xml
  `<script ... resultstepid="_toolboxreport" ... >`
```

### resultstep (obsolete)

This attribute is replaced with the **resultstepid** attribute.

```xml
  `<script ... resultstep="9" ... >`
```

# The script content

### description

The description gives a comprehensive explanation of the script's purpose and is used as a tooltip in the Maria Map Maker application.

```xml
  `<script ... >`
    ...
    `<description>`Converts source data to vector tiles./>
    ...
  `</script>`
```

### conditiondescriptor

Defines how the following conditions relates. The script can have 0 to many conditions, but normally there should at least be one condition.

```xml
  `<script ... >`
    ...
    `<conditiondescriptor operator="Any" />`
    ...
  `</script>`
```

Valid values for the **operator** attribute is:


*  **All** - All conditions should match.

*  **Any** - Any of the conditions should match.


### condition

The condition describes what must be met if the script can be used with the dropped datasource.

```xml
  `<script ... >`
    ...
    `<condition type="fileextcondition" parameters="ext=shp|gdbtable negated=false" />`
    ...
  `</script>`
```

##### type

Describes which type of condition this is.

Legal values are:

*  **fileextcondition** - The extension must be one of the given extension values.

*  **filenamecondition** - The name of the dropped file must be equal to the given value.

*  **palettedcondition** - The dropped file must be a paletted raster file.

*  **elevationcondition** - The dropped file must contain elevation data.

*  **gpkg_rastercondition** - The dropped file must be a GeoPackage raster file.

*  **gpkg_vectorcondition** - The dropped file must be a GeoPackage vector file.




##### parameters

This attribute is used to give necessary input to the condition. Each **condition** honors different values.


*  **fileextcondition** - __ext__: extension for data source file

*  **filenamecondition** - __parameters__: data source file name.

*  **palettedcondition** - __exe__: program used to check for paletted, __searchpattern__: search pattern fordata source files, __negated__: inverts result if true.

*  **elevationcondition** - __exe__: program used to check for paletted, __searchpattern__: search pattern fordata source files, __negated__: inverts result if true.

*  **gpkg_rastercondition** - __negated__: inverts result if true.

*  **gpkg_vectorcondition** - __negated__: inverts result if true..





### scripttool

Defines a specific action that shall be executed. See list of available **[tools](maria_gdk/programming/functionality/mappreparation/tool_attributes)**.

```xml
  `<script ... >`
    ...
    `<scripttool ... >`
      ...
    `</scripttool>`
    ...
  `</script>`
```

# Script attributes used by code

These should only be used by code from the client for example the MariaToolBox application.
### maxfilesize

This attribute controls if the data source should be used directly without temporary copying to the temp folder. This behaviour only applies to data sources of type ecw, mbtiles and gpkg.

The default value (maxfilesize is not defined) is to use directly without copying.

If given, data sources larger that the given value in kB is not copyied.

```xml
  `<script ... maxfilesize="1024" ... >`
```
### repositoryworkingfolder

If given, the repository will be copied to this folder.

```xml
  `<script repositoryworkingfolder="" ... >`
```
### startatstage

This attribute is used to control which stages that shall be executed. The execution will start with tools defined at this stage.

By default, all tools belong to stage #0 so this attribute is normally not used.

```xml
  `<script startatstage="" ... >`
```
### stopatstage

This attribute is used to control which stages that shall be executed. The execution will end with tools defined at this stage.

By default, all tools belong to stage #0 so this attribute is normally not used.

```xml
  `<script stopatstage="" ... >`
```
### skipstepsifpossible

When re-running a script, some tools can be skipped. One example is copying the data source files which is not necessary to repeat when re-running.

This attribute should be set to true when the script is re-run .


```xml
  `<script skipstepsifpossible="" ... >`
```

