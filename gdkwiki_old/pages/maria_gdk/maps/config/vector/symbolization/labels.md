### Labels

The `<labels>` section of the symbols.xml allows the creation of complex labels for display in MARIA. Labels can be based on primitive attributes. It is possible to define conditional labelling as demonstrated in the example below.

This section also contains the font descriptions used in label rules and label filters.

Example:

```xml
`<labelrule name="R000030">`
  `<label name="R000030_1">`
    `<condition oper="and">`
      `<condition oper="noteq" field="HWT" value="5"/>`
      `<condition oper="noteq" field="HWT" value="6"/>`
    `</condition>`
    `<item type="attrib" characteristics="1" value="NAM"/>`
  `</label>`
  `<label name="R000030_2">`
    `<condition oper="equal" field="HWT" value="6"/>`
    `<item type="string" characteristics="1" value="Minaret"/>`
  `</label>`
  `<label name="R000030_3">`
    `<condition oper="equal" field="HWT" value="5"/>`
    `<item type="string" characteristics="1" value="Marabout"/>`
  `</label>`
`</labelrule>`
```

 | Child element   | Description              | Properties | 
 | -------------   | -----------              | ---------- | 
 | characteristics | Font characteristics.    | C A        | 
 | labelrule       | Label rule definition.   | R C A      | 
 | labelfilter     | Label filter definition. | R C A      | 

#### Characteristics

Defines font characteristics for use in labelrules.

Example:

```xml
`<characteristics fontname="Calibri" scale="1">`
  `<item name="Black" id="1" colorname="Black" size="6"/>`
  `<item name="Red" id="2" colorname="Red" style="condensed" size="8"/>`
  `<item name="Bold" id="3" colorname="Black" size="6" bold="true" italics="true"/>`
`</characteristics>`
```
 | Attribute | Description                             | Properties | 
 | --------- | -----------                             | ---------- | 
 | fontname  | Name of default font to use for labels. | O          | 
 | scale     | Default scale factor.                   | O          | 

 | Child element | Description                                                | Properties | 
 | ------------- | -----------                                                | ---------- | 
 | item          | Font properties. Can be referenced by id from label rules. | R A        | 

##### Item

 | Attribute    | Description                                                                                              | Properties | 
 | ---------    | -----------                                                                                              | ---------- | 
 | id           | Unique identifier for font description.                                                                  | \\         | 
 | fontname     | Name of font used in characteristic.                                                                     | O          | 
 | colorname    | Font color (defined in color table).                                                                     | O          | 
 | style        | Font style. Valid values are [condensed, light condensed, regular].                                      | O          | 
 | bold         | True if text should be in bold.                                                                          | O          | 
 | italics      | True if text should be in italics.                                                                       | O          | 
 | size         | Font size, point (pt) size of the text string.                                                           | O          | 
 | sizespan     | Can be used instead of the size attribute to specify a range of valid font sizes. Not implemented.       | O          | 
 | case         | Indication of capitalization. Valid values are [upper, lower, mixed]. Not implemented.                   | O          | 
 | scale        | Scale factor.                                                                                            | O          | 
 | outline      | Draw labels with thin outline. Default value is true. Implemented since MARIA GDK 3.1.                   | O          | 
 | outlinecolor | Outline color (defined in color table). Default outline color is white. Implementend since MARIA GDK 3.1 | O          | 

#### Labelrule

Example:

```xml
`<labelrule name="R000024">`
  `<label>`
    `<condition oper="equal" field="TYP" value="5"/>`
    `<item type="attrib" characteristics="3" value="NAM"/>`
    `<item type="string" characteristics="3" value="Zoo"/>`
  `</label>`
`</labelrule>`
```
 | Attribute | Description                                                                                  | Properties | 
 | --------- | -----------                                                                                  | ---------- | 
 | name      | Name that identifies the labelrule. Can be referenced from the attribute based script files. | \\         | 

 | Child element | Description              | Properties | 
 | ------------- | -----------              | ---------- | 
 | label         | Label description block. | R C A      | 

##### Label

 | Child element | Description       | Properties | 
 | ------------- | -----------       | ---------- | 
 | condition     | Rule condition.   | O R C A    | 
 | item          | Text description. | R A        | 

#####  Condition

 | Child element | Description     | Properties | 
 | ------------- | -----------     | ---------- | 
 | condition     | Rule condition. | O R C A    | 

 | Attribute | Description                                                                                                                                                                       | Properties | 
 | --------- | -----------                                                                                                                                                                       | ---------- | 
 | oper      | Condition operator. Can both be used as a logical operator with values [and, or], or a compare operator with values [equal, noteq, less, lesseq, grt, grteq, in, mod, undefined]. | \\         | 
 | field     | Attribute field.                                                                                                                                                                  | \\         | 
 | value     | Attribute value.                                                                                                                                                                  | \\         | 
 | remainder | Remainder value to be used together with “mod”-operator.                                                                                                                      | O          | 

##### Item

 | Attribute       | Description                                                                                                               | Properties | 
 | ---------       | -----------                                                                                                               | ---------- | 
 | type            | Text item type. Valid values are [attrib, string].                                                                        | \\         | 
 | characteristics | Font characteristics identifier.                                                                                          | \\         | 
 | value           | Text to display in label. If type is attrib, the value is set to the name of the attribute text should be collected from. | \\         | 
 | break           | If true, linebreak will be inserted succeeding this text item.                                                            | \\         | 

#### Labelfilter

Apply a labelfilter to remove unwanted text in the map.

Example:

```xml
`<labelfilter>`
  `<suppress value="UNK"/>`
  `<suppress value="EMPTY"/>`
  `<suppress value="Unknown"/>`
  `<suppress value="Other"/>`
`</labelfilter>`
```

 | Child element | Description                                        | Properties | 
 | ------------- | -----------                                        | ---------- | 
 | suppress      | Text value to suppress when drawing labels in map. | A R        | 

##### Suppress

