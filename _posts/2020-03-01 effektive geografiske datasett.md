# Effektive geografiske datasett

Et geografisk datasett kan ganske enkelt beskrives som et geografisk tilknytta datasett. Geografien kan være et punkt, linjer eller flater. I tillegg har man ofte tilhørende attributtdata knytta til geografien. Attributtene beskriver egenskapene til geometrien, for eksempel visningsegenskaper, datering for endring, hvem som gjorde endringen, etc. 

I en mobil verden er det avgjørende å vurdere på størrelsen på geografiske datasett. En mobil klient vil måtte laste over deler eller hele datasett for å kunne presentere datasett i kart, søk eller tabeller, på brukers mobil. Desto mer effektivt datasettet er satt opp, jo større geografisk område kan bruker ta med seg ut i felt, for å jobbe videre uten dekning. 

## SR16

For å kunne gjøre dette litt mer konkret, tenkte jeg ta en titt på [Skogressurskart SR16](https://www.nibio.no/tema/skog/kart-over-skogressurser/skogressurskart-sr16), som forvaltes av [Nibio](https://www.nibio.no/). Nibio tilbyr nedlasting av datasettet som GDB-format. Eksempel for Innlandet fylke:

* 691MB: gdbtable.zip
* 1.05GB: gdbtable (pakket ut)

Dette er da størrelsen på kun geometriene, støttestrukturer (index, etc) og attributtdata kommer i tillegg. 

## TWKB

[TWKB](https://github.com/TWKB/Specification/blob/master/twkb.md) er et lagringformat for geometrier. TWKB har som mål å komprimere datasettet mest mulig, slik at det tar minst mulig plass på mobil, men også at det blir mindre data å laste ned til mobilen. Både for linjer og flater er geometriene beskrevet som lister av punkter. Disse punktene ligger ofte nært hverandre, slik at det kan være interessant å lagre kun endringen i x- og y-komponenten av geometrien, i stedet for å lagre hele x- og y-verdien på nytt for hvert punkt. TWKB kan også settes til å begrense presisjonen i geometriene. For visning på mobil, kan man feks anta at det er tilstrekkelig med desimeter-presisjon for en geometri. 

Størrelsen på dette samme datasettet for Innlandet, lagret med TWKB:

* 270MB: TWKB med 1 desimeters presisjon
* 204MB: TWKB med 1 meters presisjon

## Attributter

For noen datasett er det store attributt-tabeller tilknyttet. Her må man gjøre en vurdering på hvilke felter/kolonner man ønsker å synkronisere over til mobil, men vi har noen ekstra triks i FeltGIS, for å gjøre pakke ned slike datasett. 

I mange datasett, er det ofte de samme verdiene som går igjen. Ideelt sett, burde datakilden brukt kodelister for effektiv representasjon, men ettersom det ofte ikke er tilfelle, kan man heller skape disse kodelistene dynamisk. Eksempelvis, i stedet for å lagre "offentlig veg" og "privat veg" for hver enkelt geometri i et vei-datasett, kan man i stedet lagre koder (1 og 2) for disse verdiene, og la selve verdiene ligge i en egen tabell. For større datasett har vi også mulighet til å bruke konvensjonelle komprimeringsalgoritmer (på alle attributtene, samlet), for å ytterligere redusere størrelsen på datasettet. 

FeltGIS har en fleksibel plattform, bygget for å kunne takle typiske utfodringer vi kjenner fra typiske feltapper. Selv om mobildata blir rimeligere, og kanskje gratis, er det ikke gitt at overføringshastigheten ute i felt går opp, dersom man i det hele tatt har mobildekning. 