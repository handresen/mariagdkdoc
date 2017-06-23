# Produksjon av Vector Baseline Norway til MARIA 5.4 (PIT Leveranse) 2015

Older Versions: [2014](./olderversions/2014)



## Programvare


FME m/GeoSOSI, PowerGUI Script Editor, Maria M3, Diverse support tools

## N50

{{:maria_gdk:internal:maps:vbn1.png |}}

I år fikk vi levert N50 SOSI-data for alle kommunene zippet fylkesvis, start med å pakke de ut til en lokal mappe. Bruk FME med workspacet sosi2shape_FKB for å konvertere fra SOSI til Shape. Ved å starte dette workspacet med WorkspaceRunner fra et annet workspace kan man starte opptil 7 instanser av FME. Konverteringen vil dermed gå langt raskere. Husk å sette Attribute validation level til «none» i Reader parameters, resultatet skal bli en mappe for hver SOSI fil med en Shape fil for hver geometritype. Bruk skriptet Move_files_1dir_up for å kopiere alle filene inn i en ny mappe, fordelt på fylke. Resultatet blir da seende slik ut: 

{{:maria_gdk:internal:maps:vbn2.png |}}


Vi kan da benytte Maria M3 for å konvertere fra Shape til M4M. M3 funker dårlig på Windows 7/8/8.1. Vi bør derfor enten benytte en lokal VM med Windows XP eller VM parken https://tpn-vmlab.teleplan.no (Maria M3 Worker 1-5). I mappen Support Tools finnes det en M3 template og en batch-fil som kjører M3 i kommandovinduet. På denne måten kan man spre arbeidsmengden (for eksempel fylkesvis) utover de forskjellige VMene for å få det prosessert raskere. Etter at M3 har konvertert alle filene må vi benytte scriptet MoveAndRenameN50Files.ps1 for å flytte filene til riktig mappe samt og gi de nye navn. Dette gjøres slik at man kan bruke forrige template uten å måtte gjøre mange store endringer. 

Navnefilene konverteres direkte fra SOSI til m4m i M3.

## FKB

Konvertér fra SOSI til shape med samme fremgangsmåte som N50. Maria M3 har en tendens til å krasje når mange store filer blir lastet inn i samme kjøring. Derfor er det best å kjøre et skript i hver MariaM3Worker (RunM3.ps1) som konverterer én og én fil. Sannsynligvis vil flere filer, spesielt høydekurver og BygnAnlegg, være for store for M3. Disse må splittes opp. Dette kan gjøres med skriptet SplitShapefile. 

Etter konvertering til m4m benyttes MoveAndRenameFKBFiles.ps1 for å flytte filene inn i riktig struktur samt å rename de. 

## Vbase

Vbasen inneholder blant annet gatenavn. Vi benytter oss kun av egenskapen VEGSENTERLINJE, så man trenger ikke ta med de andre. Disse filene blir brukt for å vise labels. Om man laster ned Vbasen fylkesvis fra Norge Digitalt, er denne projisert i UTM sone 33. Dette funker bra unntatt i Finnmark. Vbase for Finnmark må man laste ned kommunevis for å få filene i UTM sone 35, gjøres ikke dette vil man få en offset i Øst Finnmark (grunnet lang avstand fra senter meridian). Vbasen konverteres fra SOSI til Shape i FME, og fra Shape til m4m i M3. Videre brukes Vbasen for å hente ut veinummer og veikategori (Europaveg, Riksveg og Fylkesveg) som vi også bruker som label. Dette kan gjøres ved å benytte seg av FME workspace vdataextVegKategoriLangAvstand som konverterer fra SOSI til Shape. Bruk M3 (ikke bruk Optimizer, den får M3 til å feile på denne operasjonen) til å hente ut navn fra shapefil og konverterer til m4m.

## Kontroll av data

For å kontrollere at man har fått med seg all data er det flere prosesser man kan benytte seg av. Det første er å bruke Resolve knappen i Maria 5.4 (Map Source). Denne sjekker at man har alle filene i templaten tilgjengelig. Dersom den mangler vises det et utropstegn i map editoren. Filnavnet må være likt i filstrukturen som i templaten for at Maria skal kunne finne den. Etter dette er gjort er det fornuftig å sammenlikne filstrukturene mellom årets og fjorårets mapper. Et bra verktøy å bruke til dette er programmet WinMerge, som grundig sammenlikner filer og mappestrukturer. Dersom det har kommet nye filer som ikke var med på fjorårets kartpakke, vil WinMerge lett finne dette.

