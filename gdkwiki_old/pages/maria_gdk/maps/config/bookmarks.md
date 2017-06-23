# Bookmarks

Bookmarks can be used to navigate easily between important locations in vector and raster maps. Each mappackage can have a separate bookmark-file. The file must have the same name as the mappackage, be located in the same folder and have a "bookmarks.xml"-extension. 
If a template contains multiple mappackages, bookmarks will be merged.

Example:

```xml
`<bookmarks>`
  `<bookmark name="Trondheim" lat="63.43" lon="10.36" scale="35000">`
    `<desc>`Trondheim by`</desc>`
  `</bookmark>`
  `<bookmark name="Bergen" lat="60.385" lon="5.284" scale="35000">`
    `<desc>`Bergen by`</desc>`
  `</bookmark>`
  `<bookmark name="Oslo" lat="59.92" lon="10.73" scale="35000">`
    `<desc>`Oslo by`</desc>`
  `</bookmark>`
`</bookmarks>`
```

##  Bookmarks xml

`<bookmarks>` is the root of the bookmarks-xml.

 | Child element         | Description     | Properties | 
 | -------------         | -----------     | ---------- | 
 | [bookmark](#Bookmark) | Bookmark entry. | O R C A    | 

### Bookmark

 | Child element | Description                      | Properties | 
 | ------------- | -----------                      | ---------- | 
 | desc          | Textual description of bookmark. | R C A      | 

