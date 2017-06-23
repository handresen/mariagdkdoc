## Script tool attributes

The tools in the script must be defined properly so that correct tools are run and with correct setup. Each tool must use a minum of the required attributes. Optional attributes does not have to be defined.

#### name

Identifies the tool and must be a known tool. See [Tools available](maria_gdk/programming/functionality/mappreparation/available_tools) for a list of known tools.

```xml
  `<scripttool name="FileListAssembler" ... />`
```

#### displayname (optional)

The given value is displayed instead of **name**. 

```xml
  `<scripttool displayname="Getting list of files" ... />`
```

#### id (optional)

Other tools in the script can reference to this tool using the given value. The value must therefore be unique. This attribute can be ignored if no other tools refer to this tool.

```xml
  `<scripttool ... id="_listfiles" .../>`
```

#### commandlineparam (optional)

```xml
  `<scripttool ... commandlineparam="singlefile=false byextension=true searchpattern=\.tif|\.jpg" .../>`

  `<scripttool ... commandlineparam="-progress -f M6M -t_srs WGS84 &quot;{workdir}\out\ &quot; &quot;{sourcedata}&quot;" .../>`
```

#### exe (optional)

This attribute is not necessary for all tools since the tool's Name attribute internally identifies the exe-file. Check the tools' documentation to see if this attribute has to be defined or can be ignored.

```xml
  `<scripttool ... exe="gdalsrsinfo.exe" .../>`
```

#### optional (optional)

```xml
  `<scripttool ... optional="true" .../>`
```

#### shouldbeexecuted (optional)

```xml
  `<scripttool ... shouldbeexecuted="true" .../>`
```

#### stage (optional)

```xml
  `<scripttool ... stage="0" .../>`
```

#### description (optional)

```xml
  `<scripttool ... description="0" .../>`
```

#### resultfrom (optional)

A tool might need to get data from other tools that have been executed. This attribute is used to create these links. Check the tool's documentation to see if any links must be defined.

```xml
  `<scripttool ... >`
     ...
     `<resultfrom tool="_listfiles" parameterkey="sourcedata"/>`
     ...
  `</scripttool>`
```
##### tool

This attribute creates the link by repeating the other tool's **id** attribute.
##### parameterkey

#### parameter (optional)

```xml
  `<scripttool ... parameter="0" .../>`
```

#### mustrun (optional)

```xml
  `<scripttool ... mustrun="0" .../>`
```

#### groupid (optional)

```xml
  `<scripttool ... groupid="0" .../>`
```

#### step (obsolete)

This attribute should not be used to set the order of execution. Instead let the tools be executed in the order they are defined in the xml file.

```xml
  `<scripttool ... step="0" .../>`
```



