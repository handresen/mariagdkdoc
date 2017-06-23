# Link files

The Vector and Raster Map services searches for Maria GDK map files in the default folder provided at installation. Map packages can also be located in secondary locations using links.xml-files pointing to the data. 
`<WRAP center round important 45%>`
Avoid circular references in link.xmls.
`</WRAP>`

File containing links should be named *.links.xml.

Example maps.links.xml:

```xml
`<links>`
  `<link path="H:\Maps\" recursiondepth="3"/>`
  `<link path="C:\servicetest\kart\"/>`
`</links>`
```
## Links

 
 | Child element | Description                                           | Properties | 
 | ------------- | -----------                                           | ---------- | 
 | link          | Link to location containing mappackages and datasets. | O R A      | 

## Link

