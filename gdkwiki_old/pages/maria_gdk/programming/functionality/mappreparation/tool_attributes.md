## Available script tools

The following scripts are available:


#### MapSignature

Creates an unique GUID which is required by many tools. This should be the first tool in the script.
#### CopySourceVectorData

Copies vector data to the Temp directory. Conditions should be used so that the script only is used for vector data.
#### CopySourceRasterData

Copies raster data to the Temp directory. Conditions should be used so that the script only is used for raster data.
#### FileListAssembler

Creates a list of available data source files. The tool honors the following parameters:

*  searchpattern - Listed files will use given searchpattern.

*  keepdirectory - Will only list the directory name.

*  singlefile - wil only list the first file.

*  byextension - File search flags will include file extension.

*  byname - File search flags will include file name.
#### GdalBuildVrt

Builds vrt-file from given input.

The tool must be defined with reference to the following other tools:

*  **mapsignature** - Must point to the **MapSignature** tool

*  **filepath** - Must point to the **FileListAssembler** tool

*  **srs** - Must point to the **GdalSrsInfo** tool





#### CleanupTempData

Will remove working directory. This is done automatically by Maria Map Maker when an import is removed so this tool is normally not necessary to include in a script.



#### GdalAddo

Creates gdal overviews from source data.

The tool must be defined with reference to the following other tools:

*  **filepath** - Must point to the **FileListAssembler** tool



#### M6MDatasetBuilder

Creates a M6M dataset and folderIndex from a regular m6m file.

The tool must be defined with reference to the following other tools:

*  **mapsignature** - Must point to the **MapSignature** tool

*  **outpath** - Must point to the tool that has copied the data source f.ex the **CopySourceVectorData** tool.
#### Ogr2M6M

Uses ogr2ogr.exe to convert source data to the M6M format.

The tool must be defined with reference to the following other tools:

*  **outpath** - Must point to the tool that has copied the data source f.ex the **CopySourceVectorData** tool.
#### ExtractMapInfo

Retrieves feature information from the M6M data file.

The tool must be defined with reference to the following other tools:

*  **mapsignature** - Must point to the **MapSignature** tool

*  **outpath** - Must point to the tool that has copied the data source f.ex the **CopySourceVectorData** tool.

#### ATocSubdatasetCollector

Uses output from gdalinfo.exe to get subdatasets contained within a TOC file.

The tool must be defined with reference to the following other tools:

*  **atocpath** - Must point to the **FileListAssembler** tool.
#### GdalSrsInfo

Uses gdalsrsinfo.exe to see if source data has projection info embedded.

The tool must be defined with reference to the following other tools:

*  **filepath** - Must point to the **FileListAssembler** tool





#### GdalInfo

Runs gdalinfo on the input dataset and creates a new response with the result in JSON format.

The tool must be defined with reference to the following other tools:

*  **sourcedata** - Must point to the **FileListAssembler** tool
#### OgrInfo

Runs ogrinfo on the input dataset and creates a new response with the result in vector tiles.

The tool must be defined with reference to the following other tools:

*  **sourcedata** - Must point to the **FileListAssembler** tool
#### GdalOverviewLevels

Calculates how many overview levels to be created.

The tool must be defined with reference to the following other tools:

*  **vrtfile** - Must point to the **GdalBuildVrt** tool

*  **srs** - Must point to the **GdalSrsInfo** tool.
#### GdalTranslate

Translates input data into GeoPackage.

The tool must be defined with reference to the following other tools:

*  **mapsignature** - Must point to the **MapSignature** tool.

*  **vrtfile** - Must point to the **GdalBuildVrt** tool.

*  **srs** - Must point to the **GdalSrsInfo** tool.

#### GdalWarp

Transforms input data into GeoPackage using supplied parameters for nodata.

The tool must be defined with reference to the following other tools:

*  **mapsignature** - Must point to the **MapSignature** tool.

*  **vrtfile** - Must point to the **GdalBuildVrt** tool.

*  **srs** - Must point to the **GdalSrsInfo** tool.

*  **info** - Must point to the **GdalInfo** tool. 

#### JsonResultsPackager

Packages json string into results object.

The tool must be defined with reference to the following other tools:

*  **rawjson** - Must point to the **GdalInfo** tool
#### ExpandPaletted

Expands paletted image to rgb.

The tool must be defined with reference to the following other tools:

*  **srs** - Must point to the **GdalSrsInfo** tool.

*  **filepath** - Must point to the **FileListAssembler** tool.
#### GenericCommand

Generic tool that can be used to create a custom execution. The exe and parameter must be defined for each tool.

The tool must be defined with reference to the following other tools:

*  **targetfilename** - Must point to the target tool.
#### M6MtoVTile

Uses ToVectorTileset.exe to convert M6M data to vector tiles.

The tool must be defined with reference to the following other tools:

*  **layersinfo** - Must point to the **OgrInfo** tool.
#### VtilesMerge

Uses ToVectorTileset.exe to merge vector tile files.

The tool must be defined with reference to the following other tools:

*  **mapsignature** - Must point to the **MapSignature** tool.
#### GenerateReport

Creates information for Maria Map Maker.

The tool must be defined with reference to the following other tools:

*  **outpath** - Must point to the **Ogr2M6M** tool.

*  **targetfilename** - Must point to the **VtilesMerge** tool.

#### Elevation

Uses Gpkg_elev.exe to process elevation data.

The tool must be defined with reference to the following other tools:

*  **mapsignature** - Must point to the **MapSignature** tool.

*  **vrtfile** - Must point to the **GdalBuildVrt** tool.
#### VectorPackageWriter

Creates the vector mappackage xml that will be deployed to the vector service.

The tool must be defined with reference to the following other tools:

*  **mapsignature** - Must point to the **MapSignature** tool.

*  **packagelocation** - Must point to the tool that has copied the data source f.ex the CopySourceVectorData tool.

*  **report** - Must point to the **ExtractMapInfo** tool.
#### RasterPackageWriter

Creates the raster xml that will be deployed to the raster service.

The tool must be defined with reference to the following other tools:

*  **mapsignature** - Must point to the **MapSignature** tool.

*  **datapath** - Must point to the **FileListAssembler** tool.

#### DeployToService

Copies all necessary files to the raster service directory. Requests reload of raster service data. Makes sure that the newly created map signature is registered with the catalog service.

The tool must be defined with reference to the following other tools:  

	* **mapsignature** - Must point to the **MapSignature** tool.
#### DeployedMapResults

Creates the result of the script.

The tool must be defined with reference to the following other tools:

*  **mapsignature** - Must point to the **MapSignature** tool.



