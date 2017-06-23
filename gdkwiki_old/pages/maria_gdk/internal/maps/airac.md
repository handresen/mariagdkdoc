# Produksjon av AIRAC-pakke for FMGT


## Bakgrunn

 
MARIA Raster Baseline Norway Aeronautical (AIRAC) pakken er en kartpakke bestående av fire forskjellige flykart i skalaen fra 1:50k til 1:1M. Denne skal konverteres fra Geotiff til QM2 format ca. fire ganger i året, og dette dokumentet beskriver denne prosessen. Det forutsettes at man har installert og tilgang visse programmer og foldere. 

## 1. Nedlasting av rådata

Sørg for å ha en FTP-klient som f.eks Filezilla installert. Som regel plasseres rådata på FTP-serveren til FMGT eller til leverandøren T-Kartor, hvor man laster de ned til lokal disk.

Normalt sett er det fire mapper, en for hvert kartsett (FLMA, LFC, M517AIR og N50AIR)

## 2. Konvertering fra Geotiff til QM2


{{:maria_gdk:internal:maps:airac3.png |}}

Dra inn Geotiff converter, QM Writer og File Storage tool. Bruk følgende instillinger for projeksjon i Geotiff converter:

 | Datasett | Projeksjon-innstilling   | 
 | -------- | ----------------------   | 
 | FLMA     | Ignore GeoTIFF, From XML | 
 | LFC      | Ignore GeoTIFF, From XML | 
 | M517AIR  | Ignore GeoTIFF, UTM 33   | 
 | N50AIR   | Ignore GeoTIFF, UTM 33   | 

Vi har tidligere brukt flere virtuelle maskiner til å konvertere N50AIR-datasettet. Etterhvert som harddisker og datamaskiner har blitt bedre viser det seg at det sannsynligvis er minst like raskt og mye enklere å konvertere alt på en lokal datamaskin, med flere instanser av M3 kjørende samtidig. Man bruker da et skript eller flytter fordeler filene manuelt inn i 3 undermapper, med circa like mange filer i hver. Det er selvsagt viktig at hver tif-fil har alle sine bestanddeler i samme mappe (.tfw, .xml osv,). Deretter kjører man opp tre instanser av M3, og konverterer hver mappene parallelt. 

Merk at parallell prosessering kun er nødvendig for N50AIR, de andre datasettene er såpass små at det tar kort tid.

Ved konvertering må man konfigurere QM writeren til å sette RGB transparency ved 0 0 0 (svart). Dersom dette ikke gjøres vil det dukke opp mange svarte områder rundt bildene. 

## 3. Kontroll av konvertert data

Det er viktig å sikre seg at alle bildene har blitt konvertert, og at de lastes korrekt inn i MARIA. Sjekk antall konverterte bilder og sammenlign med antall bilder før konvertering. Dra inn de fire folderene i en tom situasjonsfil for å se resultatet, pan rundt å visuelt inspisere. Det skal ikke være noen huller i mosaikken. Sjekk antall bilder som har blitt lastet inn i MARIA mot antall bilder på FTP-serveren, ved å trykke på (click to edit) i Configuration. Ved avvik må man finne problemet og fikse det. Husk at mappestrukturen på den nye pakken må være helt lik den gamle pakken for at templaten skal fungere.
{{:maria_gdk:internal:maps:airac4.png |}}


## 4. Bygge installasjonspakke

Legg inn andre data som skal være med i pakken (topografiske kart, høydedata, bakgrunnskart osv.). Oppdater tittelen på templaten med nåværende versjon. Oppdater forrige pakkes dokumentasjon slik at den samsvarer med de nye dataene. For å kunne bygge en installasjonspakke må InstallShield og Installation Builder være installert. Ved første gangs bruk må FMGTs lisensfil kopieres til C:\Program Files (x86)\FMGT\Installation Builder\Resources\LicenseFile. Denne lisensfilen finnes på ''%%\\data\GeoData\FMGT\Arbeidsmapper\Installer projects%%''
Bruk prosjektet fra forrige pakke som utgangspunkt (''%%\\Data\geodata\FMGT\Arbeidsmapper\AIRAC7_2014\Aeronautical.ism%%''). Husk å oppgradere versjonsnummer iht dokumentasjon.

{{:maria_gdk:internal:maps:airac5.png?300 |}}

Til slutt tar man for seg forrige versjon av kartpakken (''%%\\data\GeoData\FMGT\03 FERDIGE PAKKER%%'') og kopierer strukturen i denne. Innholdet i MapPackage-mappene byttes ut med nye data som Installation Builder har produsert. Skriptet «Copy_AIRAC_autoplaystructure.ps1» kan gjøre dette automatisk. ReadMe.pdf byttes ut med ny dokumentasjon. Alle filnavn og mapper skal være identiske med forrige versjon.

Hvis man trenger å endre teksten som står i midten av installasjonsmenyen, må man bruke AutoPlay. Åpne da ''%%\\data\GeoData\TPG\Autoplay\AIRAC\AIRAC.autoplay%%'', endre teksten og lagre med nytt navn. Man får da en mappestruktur som filene fra Installation Builder skal legges inn i.

Lykke til!

## 5. Vanlige feil

####  Det mangler data når man drar alle filene inn i Maria. Alle tiff'er har blitt konvertert men ikke alle vises i kartet.
Dette kan skje når det er ulikheter i pikselstørrelse i de forskjellige rasterene. Prøv:

*  Konvertér datasettet på nytt, men bruk legg inn projeksjon manuelt fra medfølgende xml. Velg da "Ignore GeoTIFF", klikk "Explicit" og fyll i verdier.

*  Lag en ny mappe til qm2 filene som ikke fungerer. Man kan finne ut hvilke det er ved å åpne hele GeoTIFF-datasettet i FME Data Inspector og sammenligne med Maria. Når man åpner GeoTIFF'ene i Data Inspector må man klikke på "Parameters" og velge "Feature Type Name(s): From File Name(s)". På den måte kan man klikke på rasterene og få filnavnet.







