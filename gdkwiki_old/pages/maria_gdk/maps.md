# Maps in Maria GDK

[Download as pdf](http://support.teleplanglobe.com/dokuwiki/doku.php?do=export_pdfns&pdfns_ns=maria_gdk:maps&book_title=Maps%20in%20Maria%20GDK)\\

[Preparing and configuring map packages for Maria GDK.](./maps/config)
\\
## Vector Maps

The Vector Map Service is contained in the Maria GDK and dedicated to serve maps from vector data. The Vector Map Service reads the open standard format [Vector Tiles](https///github.com/mapbox/vector-tile-spec). These files are produced using [Geospatial Data Abstraction Library / OGR](http://www.gdal.org/) or [FME Desktop](http://www.safe.com/fme/fme-desktop/), and a proprietary tool. [Maria Map Maker](./maps/config/mariamapmaker) contains all the neccessary components for producing and styling vector maps. 


## Raster Maps

The Raster Map Service is contained in the Maria GDK and dedicated to serve maps from raster data. The Raster Map Service contains dedicated readers for the following formats:

*  Geopackage

*  MBTiles

*  ECW

*  QM2

In addition, the Raster Map Service uses [Geospatial Data Abstraction Library](http://www.gdal.org/) to read a wide variety of other raster formats. The Raster Service is controlled by a set of configuration files that defines the composition of each dataset. The Raster service produces data for visual maps, elevation analysis, and effect layers.\\



{{tag>[Map]}}
