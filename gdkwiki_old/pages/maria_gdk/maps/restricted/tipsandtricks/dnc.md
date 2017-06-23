# Oppdatering av Digital Nautical Charts (DNC) kartpakke til Maria GDK

## DNC databasene

### Edition og Notice to Mariners

DNC er delt opp i 29 regioner, også kalt databaser. Hver database har et "Edition"-nummer og et "Notice to Mariners"-nummer. NtM-nummeret har formen (ukenummer)/(årstall), og finnes i lineage.doc, som ligger i hvert bibliotek (f.eks COA07J) i hver database. Edition-nummeret finner man ved å først finne NtM nummeret, for deretter å lese README.TXT som følger med hver patch. Her finner man oversikt over hvilke NtM patchen inneholder og hvilken Edition databasen skal ha for at patchen skal fungere. Man må derfor lete seg kronologisk bakover i README-filene for å finne det NtM-nummeret som står i lineage.doc. Da har man også Edition-nummeret.

Hver patch inneholder en ny NtM, mens patcher som inneholder ny Edition (såkalt Database patch) kommer med ujevne mellomrom. Når man har påført en Database patch og fått en ny Edition, har man en såkalt "Baseline".

### Patching

Hver patch inneholder alle oppdateringer siden forrige Edition (baseline). Det betyr at databasen må være oppdatert til siste edition før man kan legge på den siste patchen. Dette er **VELDIG VIKTIG**. Kombinerer man en database og en patch med ulike Edition-nummer, kan databasen bli ødelagt. En database som har blitt patchet kan ikke patches igjen. Man må derfor spare på den upatchede Baseline versjonen, og heller patche kopier av denne. 

Eksempel: DNC10 (Middle East) er Edition 25 og NtM21/14. Den nyeste patchen krever at denne databasen er Edition 26. Man må da finne ut hvilken av de tidligere patchene er en "Database patch" som oppdaterer databasen til neste Edition-nummer. Etter at man har påført denne, kan man videre påføre den aller siste patchen. Den nyeste "Database patch"'en kan også lastes ned fra [National Geospatial-Intelligence Agency](http://msi.nga.mil/NGAPortal/DNC.portal?_nfpb=true&_st=&_pageLabel=dnc_home_page&regionCode=00) Her velger man en region, og laster ned "Edition patch"
