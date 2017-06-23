### Lines

The `<lines>` section of the file contains all line element definitions.

 | Child element | Description                       | Properties | 
 | ------------- | -----------                       | ---------- | 
 | config        | Configuration settings for lines. | \\         | 
 | line          | Line element definition.          | R C        | 

#### Config

 | Child element | Description                                                                                                                                          | Properties | 
 | ------------- | -----------                                                                                                                                          | ---------- | 
 | basescale     | Scale factor applied to all pointsymbols and linepatterns used in symbolization. Local scale factors will be applied on top of this basescale value. | O A        | 
 | defaultroot   | Default root path to location of symbol/pattern files used in symbolization.                                                                         | O A        | 

##### Basescale

 | Attribute | Description                         | Properties | 
 | --------- | -----------                         | ---------- | 
 | value     | Scale factor. Default value is 1.0. | \\         | 

##### Defaultroot

 | Attribute | Description                                                                               | Properties | 
 | --------- | -----------                                                                               | ---------- | 
 | path      | Relative path to where the pointsymbol-, linepattern- and areapattern-files, are located. | \\         | 

#### Line

The `<line>` nodes defines lines which are used either as a part of a complex line/area symbolization, or as a single line element. Lines can be defined as one or more pens or as linepatterns. Point symbols can be attached to the line endpoints or centered.

Linepatterns and symbols must be available as .png-files.

Example (simple line):

```xml
`<line>`
  `<sgname>`Dk-Brown1815_0.4mmSolidLine`</sgname>`
  `<pen style="solid" width="0.4" colorname="dkbrown1815"/>`
`</line>`
```

Example (complex line):

```xml
`<line>`
  `<sgname>`LN_Dk-Brown1815_Cliff_High`</sgname>`
  `<pen style="solid" width="0.2" colorname="dkbrown1815"/>`
  `<pen style="user" width="1.0" colorname="dkbrown1815" dashes="0.1,0.5" compoundarray="0.0,1.0"/>`
`</line>`
```

Example (complex line with attached point symbols):

```xml
`<line>`
  `<sgname>`LN_Black_Bridge_DrawLift`</sgname>`
  `<multiline spacing="1.3"/>`
  `<pen style="solid" width="0.3" colorname="black"/>`
  `<symbol name="Black_0.15mm-0.75mmLenWingTick" placement="start" rotoffset="0" yoffs="-0.65"/>`
  `<symbol name="Black_0.15mm-0.75mmLenWingTick" placement="start" rotoffset="90" yoffs="0.65"/>`
  `<symbol name="Black_0.15mm-0.75mmLenWingTick" placement="end" rotoffset="0" yoffs="-0.65"/>`
  `<symbol name="Black_0.15mm-0.75mmLenWingTick" placement="end" rotoffset="90" yoffs="0.65"/>`
  `<symbol name="Black_1.3mmCircleOpen" placement="mid"/>`
`</line>`
```

Example (complex line using linepatterns):

```xml
`<line>`
  `<multiline spacing="6.0"/>`
  `<sgname>`LN_Dk-Brown1815_Cut_LargeGap`</sgname>`
  `<linepattern name="Dk-Brown1815_Cut" type="png">`
    `<scale value="1.0"/>`
    `<ctrlpts start="0.0,0.5" end="1.0,0.5"/>`
  `</linepattern>`
`</line>`
```

 | Child element | Description                                                                                                                                                | Properties | 
 | ------------- | -----------                                                                                                                                                | ---------- | 
 | sgname        | The name of the symbol graphic. This name is used when referencing the point definition from the script files or from other parts of the symbols.xml file. | \\         | 
 | pen           | Defines a pen style for drawing.                                                                                                                           | O R A      | 
 | multiline     | Defines a multiline (two parallell lines drawn). Only valid in combination with `<pen>`.                                                                     | O A        | 
 | symbol        | Defines a symbol to be drawn on the start- end- or center-point of a line. Only valid in combination with `<pen>`.                                           | O A        | 
 | linepattern   | Defines a patternfile to use when symbolizing.                                                                                                             | O C A      | 

##### Pen

 | Attribute     | Description                                                                                                                                                                                              | Properties | 
 | ---------     | -----------                                                                                                                                                                                              | ---------- | 
 | style         | Valid values are “solid” and “user”. The “user” style is useful when defining lines custom dash/dot spacing.                                                                                 | \\         | 
 | width         | Pen width.                                                                                                                                                                                               | O          | 
 | colorname     | Pen color (defined in color table).                                                                                                                                                                      | O          | 
 | dashes        | Comma separated list of dash/space values used when creating custom dash/dot lines. Mandatory when style is set to user.                                                                                 | O          | 
 | compoundarray | Comma separated list of values to specify a compound pen. Can f.ex. be used in combination with the dashes attribute to draw a line with ticks on just one side. Values are in percent of the pen width. | O          | 

##### Multiline

 | Attribute | Description                                                 | Properties | 
 | --------- | -----------                                                 | ---------- | 
 | spacing   | Spacing (in mm) between the parallell lines in a multiline. | O          | 
 
##### Symbol

 | Attribute | Description                                                                                                                                         | Properties | 
 | --------- | -----------                                                                                                                                         | ---------- | 
 | name      | Name of the symbol graphic to be used in combination with the pen style. Should correspond to an existing sgname already defined in a `<point>` node. | O          | 
 | placement | Placement of the symbol on the line. Valid values are start, end and center.                                                                        | O          | 
 | rotoffset | Rotation offset.                                                                                                                                    | O          | 
 | xoffs     | Offset in the X direction.                                                                                                                          | O          | 
 | yoffs     | Offset in the Y direction.                                                                                                                          | O          | 

##### Linepattern

 | Attribute | Description                                                                                                                                               | Properties | 
 | --------- | -----------                                                                                                                                               | ---------- | 
 | name      | Linepattern filename (only basename, no extension). Should be located at the path pointed to by the `<defaultroot>` node in the config section of the file. | \\         | 
 | type      | File extension                                                                                                                                            | \\         | 

 | Child element | Description                                                            | Properties | 
 | ------------- | -----------                                                            | ---------- | 
 | scale         | Scale factor used for modifying the filepattern size.                  | O A        | 
 | ctrlpts       | Control points used to place the corners of the linepattern correctly. | O A        | 

##### Scale

 | Attribute | Description   | Properties | 
 | --------- | -----------   | ---------- | 
 | value     | Scale factor. | O          | 

##### Ctrlpts

 | Attribute | Description                                                        | Properties | 
 | --------- | -----------                                                        | ---------- | 
 | start     | X and Y coordinates specifying the start point of the linepattern. | O          | 
 | end       | X and Y coordinates specifying the end point of the linepattern.   | O          | 

