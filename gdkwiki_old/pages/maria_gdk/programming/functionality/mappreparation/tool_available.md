# Defining scripts

## Available scripts

All scripts are written as xml-documents and embedded into the MapPreparationService.dll. The scripts are unpacked to the **scripts** catalog under the **Temp** directory during startup of the service.

## Using personal scripts

Changing the scripts in the **temp\scripts** directory will not have any effect since the MapPreparationService only loads its own embedded scripts when starting the service. Modified scripts on the **temp\script** catalog will then be overwritten and any changes will be lost.

However, personal scripts can be used by saving the script files in the sub-directory **temp\scripts\private**. All script files in this directory will be loaded during startup and will be available in addition to the embedded scripts.

Any private scripts will be shown in the dropdown list using the **Italic** font and also tagged with the text '**(private)**'.

## How are the tools run?

The tools are run in the order they are defined in the xml-file. 

The obsolete tool attribute **[Step](maria_gdk/programming/functionality/mappreparation/script_attributes)** can also be used to define the step order, but running the tools in a different order than defined will result in a very unreadable xml code and is not recommended. It is highly recommended not to use the tool's **[Step](maria_gdk/programming/functionality/mappreparation/script_attributes)** attribute, but define the tools in the required execution order!

## Running the tools in stages

By default, all tools are defined to stage #0 with the result that all tools will be run when the script is run. However, the stage feature can be used to run some of the tools and read back for example a report produced by the last run tool. The user can then decide to continue the script from the next stage or rerun the previous tools. The stage for each tool is defined using the attribute **[Stage](maria_gdk/programming/functionality/mappreparation/script_attributes)**. 

Example:

```xml
  `<scripttool ... Stage="0">`
	...
  `</scripttool>`
  `<scripttool ... Stage="0">`
	...
  `</scripttool>`
  `<scripttool ... Stage="1">`
	...
  `</scripttool>`
```

The execution of the tools in the script is then controlled with the attributes **[StartAtStage](maria_gdk/programming/functionality/mappreparation/available_tools)** and **[StopAtStage](maria_gdk/programming/functionality/mappreparation/available_tools)**. A typical usage will be to set the start and stop stage before calling **IMapPreparationServiceClient.ProcessData** or **IMapPreparationServiceClient.ReProcessData**.

For a practical example using the stage attribue, check the **vectortilesdatascript.xml** where the script is run in 2 stages, controlled from the **Maria Map Maker** application.

## How to define a script

The script starts with a header section followed by the tools. See **[Script attributes](maria_gdk/programming/functionality/mappreparation/available_tools)** for documentation on how the script must be defined.

A private script can be easiest be created by copying an existing script to the private folder and then change the name and necessary content.



