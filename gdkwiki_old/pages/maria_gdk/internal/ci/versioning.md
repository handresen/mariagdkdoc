# Versioning

The versioning in Maria GDK is based on the [Semantic Versioning 2.0.0](http://semver.org/) definition.

The version is built up by: [MajorVersion].[MinorVersion].[PatchNr].[BuildNr]
The versions are set in the parent TC project, where corresponding system properties are defined. With the exception of BuildNr, they are used throughout the project. The BuildNr is shared by all core GDK files, but unique for some support tools. The TC property for build nr is %system.ver.subsequentbuildnr% and is increased by one at the end of a successful build chain. The other versions has names matching their function.

