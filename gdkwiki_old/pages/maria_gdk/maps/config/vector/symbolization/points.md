### Points

The `<points>` section of the feature symbolization file contains all point symbol definitions.

 | Child element     | Description                                | Properties | 
 | -------------     | -----------                                | ---------- | 
 | [config](#Config) | Configuration settings for points symbols. | O C        | 
 | [point](#Point)   | Point symbol definition.                   | O R C      | 

####  Config

 | Child element               | Description                                                                                                                                          | Properties | 
 | -------------               | -----------                                                                                                                                          | ---------- | 
 | [basescale](#Basescale)     | Scale factor applied to all pointsymbols and linepatterns used in symbolization. Local scale factors will be applied on top of this basescale value. | O A        | 
 | [defaultroot](#Defaultroot) | Default root path to location of symbol/pattern files used in symbolization.                                                                         | O A        | 

##### Basescale

 | Attribute | Description                         | Properties | 
 | --------- | -----------                         | ---------- | 
 | value     | Scale factor. Default value is 1.0. | \\         | 

##### Defaultroot

 | Attribute | Description                                               | Properties | 
 | --------- | -----------                                               | ---------- | 
 | path      | Relative path to where the pointsymbol-files are located. | \\         | 

#### Point

The `<point>` blocks defines point symbols which are used either as a part of a complex line/area symbolization or as a single point map element. Point symbols can be rotated, offset and/or scaled.

Example:

```xml
`<point>`
  `<sgname>`Blue072_Obstruction`</sgname>`
  `<origo type="centerx_y" y="0.25"/>`
  `<extent w="2.90" h="3.7"/>`
  `<scale value="0.7"/>`
  `<rotation offset="45.0"/>`
`</point>`
```

 | Child element         | Description                                                                                                                                                                                                                                        | Properties | 
 | -------------         | -----------                                                                                                                                                                                                                                        | ---------- | 
 | sgname                | The name of the symbol graphic. This name is used when referencing the point definition from the script files or from other parts of the symbols.xml file.                                                                                         | \\         | 
 | [origo](#Origo)       | The defined origo of the symbol (in mm).                                                                                                                                                                                                           | A          | 
 | [extent](#Extent)     | The defined width and height of the symbol (in mm). **Note:** extent is only used when defining origo type "xy", "centerx_y" or "x_centery". X and y are then relative to the extent defined. Extent will not impact the size of the point symbol. | A          | 
 | [scale](#Scale)       | Scale factor used for modifying the symbol graphic size.                                                                                                                                                                                           | O A        | 
 | [rotation](#Rotation) | Symbol rotation.                                                                                                                                                                                                                                   | O A        | 

##### Origo

 | Attribute | Description                                                                    | Properties | 
 | --------- | -----------                                                                    | ---------- | 
 | type      | How origo is defined. Valid values are: xy, center, centerx_y, x_centery.      | \\         | 
 | x         | Origo x coordinate. Mandatory when value of type attribute is x_centery or xy. | \\         | 
 | y         | Origo y coordinate. Mandatory when value of type attribute is centerx_y or xy. | \\         | 

#####  Extent

 | Attribute | Description                           | Properties | 
 | --------- | -----------                           | ---------- | 
 | w         | Width of the symbol graphic (in mm).  | \\         | 
 | h         | Height of the symbol graphic (in mm). | \\         | 

##### Scale

 | Attribute | Description   | Properties | 
 | --------- | -----------   | ---------- | 
 | value     | Scale factor. | \\         | 

##### Rotation

 | Attribute | Description                       | Properties | 
 | --------- | -----------                       | ---------- | 
 | offset    | Rotation offset value in degrees. | \\         | 

