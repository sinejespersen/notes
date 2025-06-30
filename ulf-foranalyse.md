# Foranalyse ULF og AAKB

Undersøgelse af Ulf som datakilde til oprettelse af begivenheder i Aakb. Den
ønskede arbejdsgang er, at man opretter et begivenhed på `www.ulfiaarhus.dk`,
som derefter kopieres til `www.aakb.dk`.  

## Forudsætning

**Hvis det er en forudsætning**, at `www.ulfiaarhus.dk` skal henvise til
`www.aakb.dk` inden der redirectes Pretix, så kan denne løsning ikke lade sig
gøre

## Opgaven

Derudover at data fra ulfiaarhus' api ikke strukturerede, så hvis disse skal
importeres i `www.aakb.dk` bliver begivenheden i `www.aakb.dk` enten

1. med fejl i tidsrummet (og muligvis andre steder)
2. med krav om at de som opretter begivenheder på `www.ulfiaarhus.dk` følger
   nogle ufravigelige regler for opsætning

Alt i alt 307 timer.

| Timer      | Beskrivelse      |
| ------------- | ------------- |
| 12 timer | Opsætning af udviklingsmiljø |
| 25 timer | Sparring: pull requests og general vidensdeling |
| 20 timer | Konfiguration |
| 50 timer | **Bogholderi**: Hvilke begivenheder er oprettet/eventuelle rettelser i begivenheder, oprettelse af tabel til at holde styr på begivenheder |
| 100 timer | **Feedback og tests**: Denne del dækker over mange spørgsmål og har **høj usikkerhed**: har vi forstået/mappet data korrekt, er begivenheder oprettet som forventet, eventuelle afvigelser der opdages undervejs, opretter de forskellige brugere begivenheder forskelligt? |
| 100 timer | **Strategi for databehandling**: Denne del dækker over omsætning af forplumret data til struktureret data - her ved vi at tidspunkter bliver et problem, og ud fra gennemgang af data ser vi at der kan være andre potentielle problemer ved omsætning af data, **høj usikkerhed** |

## Vores indtryk af opgaven efter snak med Mikkel Gammelgaard

Mikkel Gammelgaard hjalp os med at forstå arbejdsgangen lidt tydeligere, og vi
fik indtryk af, at den arbejdsgang kunden i virkeligheden ville få mest ud af
er, at oprette begivenheder i `www.aakb.dk` som herefter kopieres til
`www.ulfiaarhus.dk`.

Når man opretter begivenheder på `www.aakb.dk` får man struktureret data, og
dette er betydeligt nemmere at arbejde med end ustruktureret data.

### Udfordringer ved denne løsning

Der skal undersøges om

- `www.ulfiaarhus.dk` har et post/put api
- arbejdsgangen med at sende begivenheder til godkendelse skal stadig være
  håndholdt
