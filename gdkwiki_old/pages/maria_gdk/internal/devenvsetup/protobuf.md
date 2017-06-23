# Installing and adding protobuf

##  Installing protobuf on VS2013

Visual studio 2013 tool support for protobuf.net requires some manual steps. It is only required if you need to add/modify .proto-files within the project.


*  Verify that protobuf is not installed by loading a project with protobuf entries (eg Maria2012GDK/Layers/Track/*.proto), right click entry with a proto file, "Run custom tool". If an error message is displayed, protobuf is not installed for VS2013 

*  Close all instances of VS

*  Download and install [https://code.google.com/p/protobuf-net/downloads/detail?name=protobuf-net-VS10.msi](https///code.google.com/p/protobuf-net/downloads/detail?name=protobuf-net-VS10.msi)

*  Unzip and run {{:maria_gdk:internal:devenvsetup:protobuftool-vs12.zip|Registry fix}}

*  Run "devenv.exe /setup" as administrator, located ad `<Program Files x86>`/Microsoft Visual Studio 12.0/Common7/ide

*  Start VS 2013

*  Verify that the protobuf is installed by executing the first step

If you need to add .proto-files to the project using "Add-New item...", do the following:

*  Varify that protocol buffers template is not installed: Right click a C# project, "Add-New item...". If there is no entry named "Protocol Buffers", protobuf templates are not installed

*  Locate C:\Users\`<user>`\Documents\Visual Studio 2013\Templates\ItemTemplates\Visual C#

*  Copy {{:maria_gdk:internal:devenvsetup:protobuf.zip|}} to this location, do not decompress

*  Restart VS

*  Verify that templates are installed by executing first step




## Adding protobuf to a project

### C++

*  Add new file in Protobuf-catalog, name the file `<file>`.proto. This file should be source controlled

*  Open project-properties and edit ConfigurationProperties/Build Events/Pre-Build Event/Command Line to include the new file.
	
Example: \\
// .\protoc.exe FeatureInfo.proto FeatureQuery.proto FeatureGroupInfo.proto CommonMessages.proto GeoIndexMessages.proto GeoMessages.proto AttributeMessages.proto DatasetMessages.proto LabelData.proto --cpp_out=.;.\stdafxify FeatureInfo.pb.cc FeatureQuery.pb.cc FeatureGroupInfo.pb.cc CommonMessages.pb.cc GeoIndexMessages.pb.cc GeoMessages.pb.cc AttributeMessages.pb.cc DatasetMessages.pb.cc LabelData.pb.cc
//


*  Build project and open file location folder. Two new files `<file>`.pb.cc and `<file>`.pb.h should be created in folder

*  Add files to project

*  Navigate to Local Changes/Pending changes window and UNDO adding of the two new files

*  Files should still be listed in project but NOT in source control
	
`<WRAP center round important 80%>`
MAKE SURE ONLY .proto file is source controlled, otherwise generating new pb.cc/pb.h-files will fail due to ready-only files.
`</WRAP>`

### C#


*  Add new protobuf-file in Layers\Map\TPG.GeoFramework.Map.Proto (or other location set up for adding protobuf-files). Name the file `<namespace>``<file>`.proto (f.ex. TPG.GeoFramework.Map.FeatureGroupInfo.proto).

*  Navigate to the file system folder containing the new protobuf-file.

*  Locate the new file(s) and copy to Libraries/TPG.ProtobufStore

*  Delete the original .proto-file from the solution

*  Add the copied .proto-file to the solution under Libraries/TPG.ProtobufStore. If a corresponding .cs-file has been created - do NOT add this to source control/solution

*  Navigate to the Layers\Map\TPG.GeoFramework.Map.Proto again and add .proto-file from previous step AS A LINK (right click, add existing, show all files, select FeatureGroupInfo.proto, add as link). File should now be shown in solution as a linked file.

*  .cs-file 

