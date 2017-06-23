# Oppdatering av Nautical Planning Package Norway (ENC)

## ENC data
Bruk MARIA M3. Man må konvertere ett datasett for hvert valg i intended usage filter (Overview, General, Coastal osv.) Velg folder, og kryss av for property data og metadata.

## Landkart

Det er to mapper med landkart i templaten, fordi enkelte tema skal komme lengre bak i tegnerekkefølgen. Landkartene har blitt konvertert direkte fra SOSI til m4m i Maria M3. Man må dermed fortsette med denne metoden for å beholde filnavnstrukturen som templaten forutsetter (går man via shape-format får man en annen filstruktur). N250 og N2000 kan enkelt konverteres av én M3Worker. For N50 er det best å la én M3Worker konvertere ett fylke, det vil si ca 4 fylker per Worker. Det går galt et sted når alle fem Maria M3Workers prøver å lagre filer i samme mappe i \\data\GeoData\TPG. Det gikk bra når 2-3 av Workers'ene i stedet lagrer filer lokalt.





