# Preprocess Vector Data

The Vector Map Service is contained in the Maria GDK and dedicated to produce maps from vector data. The Vector Map Service reads a proprietary Teleplan Globe map format (.m6mdata, .m6mindex), as well as vector tiles (.vtiles.sqlite). Both of these formats are created from sets of m6m files. An m6m file is equivalent to a single layer in a spatial dataset.

The m6m files may be produced using [Geospatial Data Abstraction Library / OGR](http://www.gdal.org/) or [FME Desktop](http://www.safe.com/fme/fme-desktop/). Both require a writer / driver developed and distributed by Teleplan Globe to be able to produce m6m files.\\

### Basic guides

Basic instructions on how to translate vector files to a format that can be read by Maria GDK


*  [Producing m6m files using FME](./convertmap/fme)

*  [Producing m6m files using OGR](./convertmap/ogr)

*  [Building m6m datasets from m6m files](./convertmap/indexing)

*  [Building vector tilesets from m6m files](./convertmap/vector_tiles)


### Advanced guides

Instructions on larger operations related to map conversion and the creation of map packages for MariaGDK

*  [S-57 Hydrographic Data Translation](./convertmap/S-57)

*  [N50 Data Translation](./convertmap/N50)

*  [OpenStreetMap data](./convertmap/osm)

{{tag>[vector] [map] [m6m] [FME] [OGR] [GDAL]}}
