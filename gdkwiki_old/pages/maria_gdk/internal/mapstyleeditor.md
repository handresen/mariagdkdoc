#  Map Style Editor

//This application has been superseded by [Maria Toolbox](maria_gdk/maps/config/mariatoolbox) and is no longer in use. //

This document is intended for map experts with the aim to create and edit map packages for Maria 2012 Geo Development Kit. The Map Style Editor is a standalone program bundled with a SimpleMapServiceHost, created from the nightly builds of the Maria 2012 Map Service. For more information regarding the Map Service and parameters of the vector map, please read *Maria2012 Vector Maps* as this will not be discussed in this document.\\
{{maria_gdk:maps:config:vector:maria2012_-_map_style_editor_html_m437f1aa6.png?400|Maria Map Style Editor}}

 ======  Prerequisites ======

The Map Style Editor is delivered as an installer, either as a single MSI-file or an EXE-file. The EXE-file will check for missing prerequisites, and install these if needed.\\ 
Some knowledge of XML (Extensible Markup Language) is recommended, although not required.

#  Setting up the Map Style Editor

##  Folder structure

The Map Style Editor is able to open, edit and create new map packages on already installed services (i.e NORCCIS). When updated be sure to restart these services in order to see the applied changes in the application. It is recommended to create a new folder structure when creating new maps in order to separate new map packages from already installed map packages.

##  Configuring the Map Style Editor

When opening the Map Style Editor for the first time, it is necessary to configure the included Catalog Service through the settings. Set up Catalog Template Path and Maps Path to point to folder structure as mentioned above.

{{maria_gdk:maps:config:vector:MARIA2012_-_Map_Style_Editor_html_m4d715204.png?273x162|Settings}}

It is possible to change the port number either at random or manually. The port numbers from 9100 to 9200 are predefined as random interval, and these are often available.

Map Style Editor is now ready to open, edit and create vector maps. Windows 8 users may have to open the editor as an administrator in order for the Map View to function.

# Working with maps

[Working with maps](./mapstyleeditor/editmaps)


#  Working with templates

[Working with templates](./mapstyleeditor/edittemplates)

#  Map Tools

[Map tools](./mapstyleeditor/maptools)


