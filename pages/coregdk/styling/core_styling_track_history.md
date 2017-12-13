---
title: Track history styling
keywords: tracks, history, styling
tags: [track]
sidebar: core_styling_sidebar
permalink: core_styling_track_history.html
summary: Using track history visualisation controls in Maria GDK. 
---

Track history visualisation controls how previous positions are displayed in the map. History styling includes amount of history to include as well as regular appearance styling. Track history belongs to the "TrackSymbol" style category.

```xml
<stylecategory "TrackSymbol">
  ...
  <compositeitem "History">
    ...
  </compositeitem>
</stylecategory>
```

{% include image.html file="./core/trackhistoryoverview.png" %}

#### Limits

Limits the amount of history data fetched from the service and how much history is displayed in the map. If more than one limit item is set, the most limiting value is applied. 

```xml
<compositeitem name="History">
  <compositeitem name="Limits">
    <valueitem name="MaxLength" value="<percentage of screen diagonal>"/>
    <valueitem name="MaxAge" value="<seconds>"/>
    <valueitem name="MaxCount" value="<history points>"/>
  </compositeitem>
  ...
</compositeitem>
```

 | Item      | Description                                                                                                                                           | 
 | ----      | -----------                                                                                                                                           | 
 | MaxLength | Max length of history measured as percentage of screen diagonal. Note that this is an approximate measure and depends on active scale and projection. | 
 | MaxAge    | Max age in seconds. Any history points older than this value will be discarded/filtered out                                                           | 
 | MaxCount  | Max count of history points                                                                                                                           | 

#### Visual

History visual settings control graphical appearance of the track history.

```xml
<compositeitem name="Visual">
  <valueitem name="DrawLines" value="<true|false>"/>
  <valueitem name="Color" value="<color>"/>
  <valueitem name="LineThickness" value="<pixels>"/>
  <valueitem name="DashStyle" value="<dashes specification>"/>
</compositeitem>
```

 | Item          | Description                                                                                                                                                                                                                                                                                                                        | 
 | ----          | -----------                                                                                                                                                                                                                                                                                                                        | 
 | DrawLines     | If false, no line is drawn. Future extensions may include display of individual history points, for now history is invisible if this is set to false                                                                                                                                                                               | 
 | Color         | Color of history line                                                                                                                                                                                                                                                                                                              | 
 | LineThickness | Thickness of history line in pixels                                                                                                                                                                                                                                                                                                | 
 | DashStyle     | Used to set history line dash. It consists of a comma separated list decimal numbers, the first specifies length of dash in pixels, the second the length of the spacing between dashes, the third the next dash length and so on. The history visualisation below was created using "\<valueitem name="DashStyle" value="4,2,1,2"/>" | 

{% include image.html file="./core/trackhistorydashstyle.png" %}
