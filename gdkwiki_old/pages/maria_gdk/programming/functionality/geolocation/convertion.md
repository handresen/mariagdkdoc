# Converting placename data

When converting a file containing placename information to a Maria2012 location database, you need a reader for that specific fileformat. See chapter on [creating readers](./readers) for details.

Readers are executed from the commando line. \\
//Usage: `<input file>` `<input format>` `<sqlite database file>` [/clear]//

//Input arguments: \\
[1] - Path to file containing placename information. Must be in a format supported by the LocationServiceSqliteLoader. \\
[2] - Input format string. Currently supported: "ssr", "geonames", "gns", "gns_us" and "matrikkel". \\
[3] - Path to output sqlite file. \\
[4] - Optional argument /clear. If output file exists placename data will be added to the file. Use /clear to force a fresh database.// \\

`<WRAP center round important 100%>`
When adding to a existing database, tables might end up with duplicate entries.
`</WRAP>`

Example: \\
//c:\Maria2012\Location> TPG.GeoFramework.LocationServiceSqliteLoader.exe c:\LocationData\GNS\no.txt gns c:\ServiceTest\LocationData\gns_no.location.sqlite /clear
//

## Supporting data

The different readers uses a small number of csv/txt-files to map information in the sqlite databases (F.ex. mapping from country codes (NO) to country names (Norway)). If these files are not present when converting a database, an error is given but converting the database will still work in some cases. The files should be placed in the same folder as the source datafiles.

Default data files can be found in TPG TFS at `<GDK  root>`Src\Layers\Location\TPG.GeoFramework.GeolocSqliteLoader\SupportData
### Feature classes and codes

Dsg-files should map feature codes and feature classes to descriptive texts. \\

File should be comma separated strings with format *code,name,text,fea_class*. F.ex: 'E,"mosque","a building for public Islamic worship","S - Spot Features",'. The three first entries are feature code, feature name and feature code description. The last entry (fea_class) is a combination of feature class code and feature class text, separated with '-'. 

Example:


CODE,NAME,TEXT,FEA_CLASS,
"MND","mound(s)","a low, isolated, rounded hill","T - Hypsographic",
"MNDU","undersea mound","a low, isolated, rounded hill","U - Undersea",
"MNFE","iron mine(s)","a mine where iron ore is extracted","S - Spot Features",
"MNMT","monument","a commemorative structure or statue","S - Spot Features",
"MNQ","abandoned mine","abandoned mine","S - Spot Features",
```

Used by:
 | Reader   | Filename | 
 | ------   | -------- | 
 | gns      | dsg.csv  | 
 | geonames | dsg.csv  | 
 

### Country codes

cc-files should map country codes to country names. \\
File should be comma separated strings with format *code,name*, f.ex. 'ZI,"Zimbabwe",'.

Example:


CODE,NAME,
UV,"Burkina Faso",
CM,"Cameroon",
CJ,"Cayman Islands",
IP,"Clipperton Island",
CG,"Congo, Democratic Republic of the",
CY,"Cyprus",
DR,"Dominican Republic",
```

Used by:
 | Reader   | Filename | 
 | ------   | -------- | 
 | gns      | cc.csv   | 
 | gns_us   | cc.csv   | 
 | geonames | cc.csv   | 
 | ssr      | cc.csv   | 

### Administrative data

Adm-files should map administrative codes to (numbers and/or letters) to descriptive text.\\
Use *Adm1* for "Administrative division level 1, US state, Norwegian fylke", *Adm2* for "Administrative division level 2, US county, Norwegian kommune" and *Adm3* for "Administrative division level 3, US ?, Norwegian poststed". \\
Files should be comma separated strings with format *code,name*. F.ex: '1662,"Klæbu kommune",'.

Example:


CODE,NAME,
1662,"Klæbu kommune",
0604,"Kongsberg kommune",
0402,"Kongsvinger kommune",
0815,"Kragerø kommune",
1001,"Kristiansand kommune",
```

Used by:
 | Reader    | Filename                                                 | 
 | ------    | --------                                                 | 
 | gns       | See [#Adm data for gns reader](#Adm data for gns reader) | 
 | matrikkel | matrikkel_adm.csv                                        | 

#### Adm data for gns reader

File should be comma separated strings with format *adm1_cd,country_cd,adm1_name*. F.ex: '"04","NO","Buskerud",'.

Example:


ADM1,COUNTRY_CD,ADM1_NAME,
"04","QA","Al Khawr wa adh Dhakhīrah",
"34","WA","Okavango",
"35","AF","Laghmān",
"35","AG","Aïn Defla",
"35","BF","San Salvador",
```

Used by:
 | Reader | Filename     | 
 | ------ | --------     | 
 | gns    | gns_adm1.csv | 


### Navnetyper

Navnetyper.txt should map feature classes and feature codes to descriptive texts.\\

File should be comma separated strings with format *classcode,classtext,code,codetext*. F.ex: 'FC1,"Terrengformer",N1,"Berg","Mindre fjell"'. 

Example:


FC6,"Samferdsel",N165,"Landingsstripe","Landingsplass for privat flygning"
FC6,"Samferdsel",N232,"Plass/torg","I tettsted eller by"
FC7,"Administrative områder",N180,"Nasjon","Selvstendig stat / land (offisielt navn)"
FC7,"Administrative områder",N181,"Fylke","Offisielt navn"
FC7,"Administrative områder",N182,"Kommune","Offisielt navn"
```

Used by:
 | Reader | Filename       | 
 | ------ | --------       | 
 | ssr    | navnetyper.txt | 

