---
title: Maria GDK document collection
keywords: sample homepage
tags: [getting_started, navigation]
sidebar: mydoc_sidebar
permalink: index.html
summary: Maria GDK documentation is primarily intended for development teams using the Maria GDK platform and for members of the Maria GDK development team. 
---

## Documentation overview
This documentation has been created using [Jekyll](http://jekyllrb.com/docs/home/) for transforming markdown documentation files into finished html-documentation and [Github pages](https://pages.github.com/) for publishing. Please contact [Teleplan Globe](https://www.teleplanglobe.no/) if you want to contribute to the documentation, or create a pull request in git on the [documentation source](https://github.com/handresen/mariagdkdoc).

Instructions for authoring can be found [here](http://idratherbewriting.com/documentation-theme-jekyll/).

### Document authoring setup
Anyone producing or editing documents for Maria GDK need to set up an authoring environment. This will allow editing, document preview, styling changes and publishing.

[Authoring system setup][authoring_setup]


### Document organization
For detailed  documentation, please select wanted content from the "Content" menu.

|  | Name | Description |
|:---:|:---|:---|
| <img class="tableImage" src="images/logo/marialogo_small.png"/> | [Core GDK functionality][core_landing_page] | Detailed description of layers, functionality and services |
| <img class="tableImage" src="images/logo/marialogo_small.png"/> | [Maria client API](mariaapi_landing_page.html) | Maria API presents simplified interface for working with the GDK services and client layers. Contains sample applications using the Maria API.  |
| <img class="tableImage" src="images/logo/marialogo_small.png"/> | [Maria Map Maker](mmm_landing_page.html) | Used to create and manage maps  |



## Converting from dokuwiki (tips and tricks)
- Interne, autogenererte linker må være lowercase. F.eks. linken: core_vector_mappackages.html#Scalebase for å referere til kapittelet "Scalebase" vil ikke fungere. Bruk core_vector_mappackages.html#scalebase.
- Referanser til filnavn i f.eks. bildelinker må være lowercase.

- Ekstra linjeskift før eller etter en tabell vil ofte ordne opp i tilfeller der tabellen ikke ser ut til å bli tolket slik den skal. Dette gjelder også overskrifter.
- "Harde" linjeskift fra dokuwiki kan dukke opp som en "\\". Fjern der det ikke er behov (f.eks. i tomme tabell-ruter) eller bytt ut med "`<br/>`".

- Noen tabeller som ikke har header har fått generert header av første rad. 
- Noen tabeller/avsnitt på bunnen av en side har av ukjente grunner ikke kommet med. Pass på å sammenligne ny side med gammel side i dokuwiki for å forsikre deg om at alt er med.


{% include links.html %}
