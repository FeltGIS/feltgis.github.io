En av FeltGIS sine grunnidéer er å tilby en skalerbar, kostnadseffektiv og fleksibel plattform for typiske feltapper. Med en typisk feltapp tenker vi på en mobilapp for iOS og Android, mobil og nettbrett, med kartstøtte. Appene må støtte arbeidsflyt for områder med og uten dekning. Vi er per i dag live og i produksjon med dette systemet. 

Vi deler inn vår tekniske tilnærming til dette i tre hovedområder: 

* Features
* RasterTiles
* VectorTiles

## Features

Features er redigérbare objekter, med eller uten geometri. De har et skjema som beskriver felter og datatyper, og evt om objektet skal kunne representeres med et punkt, linje eller ei flate. Sammen med geometrien ligger også informasjon om styling, dvs hvordan geometrien skal tegnes i kartet. 

FeltGIS har bygget sitt eget system synkronisering av features mellom skytjenester, mobilapper og den grønne boksen vår (kjører på en mini-PC som er koblet til skogsmaskin-PC). Vi har bred erfaring med tilsvarende mekanismer (ESRI, Realm, Firebase), men har sett det fordelaktig å utvikle vår egen. 

Et tenkt eksempel som illustrerer hvordan vi bruker dette systemet: 

* Sammen med kunden definerer vi en datamodell. I et enkelt tilfelle, feks "observert art". Denne datatypen har en rekke felter for å beskrive observasjonen, samt illustrasjoner for hvordan artene skal fremstå i kartet som punkt. 
* Systemet har støtte for roller og tilgangskontroll. For eksempel, kan feltarbeider kun se egne objekter, eller kanskje også laget sine registreringer, og. Brukere i ledergruppen vil kunne se samtlige registreringer med sin innloggede bruker. 
* Når feltarbeider logger inn i appen første gang, vil alle tilgjengelige observerte artsobjekter synkroniseres og være tilgjengelig i sin helhet i appen.
* Så snart feltarbeider gjør en endring i et objekt, vil det synkroniseres med skytjenester, og evt andre brukere som har tilgang til objektet. Dersom det ikke er dekning, vil objektet sendes inn ved anledning (så snart enheten har dekning igjen). Appen kan godt vise status for når sist synkronisering har blitt gjort mot server, men forholder seg ellers ikke til at enhet ikke har dekning. Appen har også mulighet til å sende inn data i bakgrunnen, dvs når bruker ikke har appen oppe. 
* Varslinger: Dersom kunde ønsker å legge inn triggere for varsling, har vi mulighet til det. For eksempel, dersom en kollega gjør registreringer av en bestemt art i nærheten av der bruker befinner seg, kan vi sende ut pushvarsel om dette. 

Den største motivasjonen for at vi begynte å utvikle dette på egen hånd, var manglende fleksibilitet i andre løsninger. Vi ønsker å ha mulighet til å kunne gjøre endringer i datamodellen, uten å måtte gjøre full synk av datasettet. Vi ønsker også å være fri for evt bindinger til lisensdrivende komponer, feks ArcGIS server. Vi ønsker også ha full fleksibilitet til kartmotor i klienten, slik at vi kan kjøre andre kartmotorer enn ArcGIS runtime SDKene ved behov. Vår kildekode kjører allerede på Windows, Mac/iOS, Android og Linux. 

På toppen av denne synkroniseringsmekanismen har vi en kartserver som mobilappene bruker for å laste inn features i kartet på mobilklientene. Den samme kartmotoren kjøres på den grønne boksen vår, og konsumeres av en webapp som kjører på skogsmaskin-PC. På skysiden har vi et administrasjonssystem for å forvalte de enkelte datasett, samt mulighet for kunde til å gjøre live tilkoblinger egne datasett, for eksempel QGIS eller ArcGIS Desktop. 

## VectorTiles

VectorTiles er en måte pakke sammen et georeferert datasett, basert på et geografisk utsnitt. VectorTiles kan caches opp, slik at en klient effektivt kan hente ut data for større området uten at det blir noen belastning på bakenforliggende databasesystem. VectorTiles er særlig egnet for datasett man kun ønsker å konsumere, uten å gjøre endringer i. Vi har støtte for klargjøring for offline vectordatasett samt presentasjon av disse både på mobil og i webappen vår. VectorTiles har også den fordelen at man dynamisk kan endre styling i klienten, og zooming i klienten tegnes pent da kartmotoren kontinuerlig kan tegne objektene på nytt. 

Vi har mulighet til å cache opp vectortiles direkte fra klienten. For større områder, eller for områder uten dekning, hvor flere brukere har behov for offline tilgang, kan det være fordelaktig å lage en egen kartpakke for disse områdene, på forhånd, og laste disse ned til klient ved behov. 

## RasterTiles

RasterTiles har mye av de samme egenskapene som VectorTiles, men i stedet for å lagre fullstendig objektinformasjon inneholder raster kun en grafisk beskrivelse av det geografiske området. Fordelen er, som med VectorTiles, at det er enkelt å ta med seg offline, ulempen er at skaleringen i klienten ikke blir så pen, samt at man ikke har objektinformasjon for identify-operasjoner (trykk på kartobjekt for mer info) - identify krever nettilgang for å sende ny forespørsel til bakenforliggende tjeneste for mer detaljer om objekt. 

I mange tilfeller har man ikke full tilgang til kildedatasettene, men kun en dynamisk karttjeneste (feks WMS). For å enkelt imøtekomme behovet for offline kart av disse tjenestene, har vi derfor en tjeneste som hjelper oss å konvertere slike dynamiske karttjenester over til RasterTiles, slik at klientapplikasjonene våre kan cache disse opp for offline bruk. 

## Diverse

I forbindelse med skjema som definerer datamodellene, har vi også støtte for rapporter. Dvs, man kan enkelt definere en mal for en rapport, som populeres med registrerte data for bestemte objekt. Disse rapportene kan legges ved som vedlegg til objektene, eller distribueres per e-post. Rapportene kan også inneholde kartutsnitt fra akutell geografi.

## Veien videre

FeltGIS er et ungt selskap, men vi er overbevist om at vår løsning har sin plass sammen med de store og etablerte aktørene i dette markedet - kanskje særlig sett fra et mobile first perspektiv. Vi bygger vår løsning for å fungere i samspill med andres, og søker et fruktbart samspill mellom ulike tekniske løsninger for å unngå "lock in". 

Vi gjør allerede noe synkronisering mot eksterne datasett, men her vil vi utvide etterhvert som behovet oppstår. For eksempel er det nyttig å gjøre full synkronisering mot eksterne datasett. Der vi i dag laster inn dette datasettet som RasterTiles, har vi i blant noe utfordinger med sporadisk nedetid i deres servermiljø. Ved å gjøre en full synkronisering av dette datasettet, og heller presentere det som features eller vectortiles over til våre klienter, vil det gi våre kunder og brukere en bedre brukeroppelvelse. 