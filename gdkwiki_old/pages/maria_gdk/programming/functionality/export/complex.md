## Complex Export

The complex export functionality is similar to the [Offscreen Export](maria_gdk/programming/functionality/export/offscreen) in that the layers to export has to be set up manually. The entry point is [OffscreenRasterExportFactory](http://support.teleplanglobe.com/MariaGDKDoc/html/BAA96374.htm) that will provide a [IComplexOffScreenExporter](http://support.teleplanglobe.com/MariaGDKDoc/html/6488453B.htm) implementation. This interface provides the property [OffScreenRasterExporter](http://support.teleplanglobe.com/MariaGDKDoc/html/500ED1B7.htm) that should be used for setting up the layers to export.

The difference between [Offscreen Export](maria_gdk/programming/functionality/export/offscreen) and the the complex export is the fact that the former results in a bitmap, while the latter has a intermediate step where a [ExportCanvas](http://support.teleplanglobe.com/MariaGDKDoc/html/AFDAF2E.htm) is provided. This class exposes functionality to add visual elements at predetermined places. Apart from that, the ExportCanvas inherits WPF Canvas, and can as such be used in just the same way in order to customize the output in flexible manner.

When the canvas has all the desired elements use [FinalizeExport](http://support.teleplanglobe.com/MariaGDKDoc/html/21D78C30.htm) method of the ExportCanvas class to get the final bitmap. This method will create the bitmap taking into account dpi and all other factors to create a correct bitmap.

