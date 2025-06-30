# Foranalyse ULF og AAKB

Undersøgelse af Ulf som datakilde til oprettelse af begivenheder i Aakb. Den
ønskede arbejdsgang er, at man opretter et begivenhed på `www.ulfiaarhus.dk`,
som derefter kopieres til `www.aakb.dk`.

## Konsekvens

**Hvis det er en forudsætning**, at man skal gå via `www.aakb.dk` for at købe billet så kan det ikke lade sig gøre at `www.ulfiaarhus.dk` er stedet hvor man opretter begivenheder, da man i sagens natur ikke kan henvise til uoprettede begivenheder.

## Opgavens forudsætning

Derudover at data fra ulfiaarhus' api ikke strukturerede, så hvis disse skal
importeres i `www.aakb.dk` bliver begivenheden i `www.aakb.dk` enten

1. med fejl i tidsrummet (og muligvis andre steder)
2. med krav om at de som opretter begivenheder på `www.ulfiaarhus.dk` følger
   nogle ufravigelige regler for opsætning (se [Udfordringer med
   datakilden](#udfordringer-med-datakilden)).

## Estimater

Alt i alt 312 timer.

| Timer     | Beskrivelse                                                                                                                                |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------|
| 12 timer  | Opsætning af udviklingsmiljø                                                                                                               |
| 25 timer  | Sparring: code review og general vidensdeling                                                                                              |
| 25 timer  | Integration i DPL CMS                                                                                                                      |
| x timer   | Dokumentation af løsningen                                                                                                                 |
| 50 timer  | **Holde styr på**: Hvilke begivenheder er oprettet/eventuelle rettelser i begivenheder, oprettelse af tabel til at holde styr på begivenheder |
| 100 timer | **Feedback og tests**: Denne del dækker over mange spørgsmål og har **høj usikkerhed**: har vi forstået/mappet data korrekt, er begivenheder oprettet som forventet, eventuelle afvigelser der opdages undervejs, opretter de forskellige brugere begivenheder forskelligt? |
| 100 timer | **Strategi for databehandling**: Denne del dækker over omsætning af forplumret data til struktureret data - her ved vi at tidspunkter bliver et problem, og ud fra gennemgang af data ser vi at der kan være andre potentielle problemer ved omsætning af data, **høj usikkerhed** |
| x timer | Projektledelse                                                                                                                               |

### Eksempler på ustruktureret data

Begivenheden "Boghygge" foreligger i `www.ulfiaarhus.dk` som
<https://www.ulfiaarhus.dk/grundskole/boghygge> og den tilsvarende begivenhed på
`www.aakb.dk` er
<https://www.aakb.dk/hovedbiblioteket/arrangementer/skole/boghygge-0-klasse> og
<https://www.aakb.dk/hovedbiblioteket/arrangementer/skole/boghygge-0-klasse-1>.

Det fremgår af
<https://www.aakb.dk/hovedbiblioteket/arrangementer/skole/boghygge-0-klasse> at
begivenheden løber af stablen 25. februar 2026 kl. 09:30-10:15, mens det i
`www.ulfiaarhus.dk` fremgår mindre ekplicit:

> **Periode** 18.02.2026 - 25.02.2026
>
> Der kan bookes billet til arrangement disse dage:
>
> Onsdag 18. februar kl. 9.30-10.15
>
> Onsdag 25. februar kl. 9.30-10.15

<!--
``` json
  …
  "field_description_of_period": "<p>Der kan bookes billet til arrangement disse dage:&nbsp;</p><p>&nbsp;</p><p>Onsdag 18. februar kl. 9.30-10.15</p><p>Onsdag 25. februar kl. 9.30-10.15</p>",
  …
  "field_period": {
    "type": "daterange",
    "start_date": "18.02.2026",
    "end_date": "25.02.2026",
    "separator": " - "
  },
  …
```
-->

Begivenheden "Børnenes grundlovsfest" er i `www.ulfiaarhus.dk`
(<https://www.aakb.dk/hovedbiblioteket/events/skole/bornenes-grundlovsfest>)
struktureret anderledes:

> **Periode** 04.06.2026 - 04.06.2026
>
> én billet - én klasse
>
> **Varighed** 3 timer
>
> Fra 10:00-13:00

Den tilsvarende begivenhed i aakb.dk:
<https://www.aakb.dk/hovedbiblioteket/events/skole/bornenes-grundlovsfest>.

Ovenstående to eksempler viser udfordringen med at konvertere fra
`www.ulfiaarhus.dk`-data til `www.aakb.dk`-data.


Nedenstående er en (redigeret*) udgave af to forskellige begivenheder fra
ulf-apiet, med providerid 934 (Aarhus Bibliotekerne), som vi har en forståelse
af er det data som angår ITK Dev.

I nedenstående data er der ikke datoer for de to events. I det ene eksempel
(8064) kan man gætte at det foregår d. 04.06.2026, i det andet eksempel (8002)
kan man konkludere det er 2 timer i tidsrummet 06.11.2025-26.03.2026, men der er
ikke konkrete datoer tilgængeligt. Datoerne fremgår af feltet
`field_registration_description`, men ikke på en måde som ville være trivielt at
formalisere, og det er jo, som det fremgår af navnet og indhold på feltet, ikke
det formål feltet er tiltænkt.

```json
  {
        "id": "8002",
        "label": "AI Dilemmaspil",
        "field_all_year": false,
        "field_description_of_duration": null,
        "field_description_of_period": null,
        "field_description_of_price": null,
        "field_duration": "2",
        "field_duration_unit_taxonomy": "timer",
        "field_registration_description": "<p>Tilmelding på bibliotekets hjemmeside:</p><p>06/11/2025 <a href=\u0022https://www.aakb.dk/hovedbiblioteket/arrangementer/skole/ai-dilemmaspil/2025-11-06\u0022>AI Dilemmaspil | Aarhus Bibliotekerne</a></p><p>13/11/2025 <a href=\u0022https://www.aakb.dk/hovedbiblioteket/arrangementer/skole/ai-dilemmaspil/2025-11-13\u0022>AI Dilemmaspil | Aarhus Bibliotekerne</a></p><p>20/11/2025 <a href=\u0022https://www.aakb.dk/hovedbiblioteket/arrangementer/skole/ai-dilemmaspil/2025-11-20\u0022>AI Dilemmaspil | Aarhus Bibliotekerne</a></p><p>05/03/2026 <a href=\u0022https://www.aakb.dk/hovedbiblioteket/arrangementer/skole/ai-dilemmaspil/2026-03-05\u0022>AI Dilemmaspil | Aarhus Bibliotekerne</a></p><p>12/03/2026 <a href=\u0022https://www.aakb.dk/hovedbiblioteket/arrangementer/skole/ai-dilemmaspil/2026-03-12\u0022>AI Dilemmaspil | Aarhus Bibliotekerne</a></p><p>19/03/2026 <a href=\u0022https://www.aakb.dk/hovedbiblioteket/arrangementer/skole/ai-dilemmaspil/2026-03-19\u0022>AI Dilemmaspil | Aarhus Bibliotekerne</a></p><p>26/03/2026 <a href=\u0022https://www.aakb.dk/hovedbiblioteket/arrangementer/skole/ai-dilemmaspil/2026-03-26\u0022>AI Dilemmaspil | Aarhus Bibliotekerne</a></p>",

        "field_period": {
          "type": "daterange",
          "start_date": "06.11.2025",
          "end_date": "26.03.2026",
          "separator": " - "
        },
        "provider": {
          "id": "934",
          "label": "Aarhus Bibliotekerne",
        }
    }
```

```json
    {
        "id": "8064",
        "label": "Børnenes grundlovsfest",
        "field_description_of_duration": "<p>Fra 10:00-13:00</p>",
        "field_description_of_period": null,
        "field_description_of_price": null,
        "field_duration": "3",
        "field_duration_unit_taxonomy": "timer",
        "field_registration_description": null,
        "field_period": {
            "type": "daterange",
            "start_date": "04.06.2026",
            "end_date": "04.06.2026",
            "separator": " - "
        },
        "provider": {
          "id": "934",
          "label": "Aarhus Bibliotekerne",
        }
    },
```

Disse er umulige at omsætte til begivenheder på `www.aakb.dk` da der ikke

*\* Jeg tillod mig at slette irrelevante dele af begivenhederne fordi de var
lange*

## Vores indtryk af opgaven efter snak med Mikkel Gammelgaard

Mikkel Gammelgaard hjalp os med at forstå arbejdsgangen lidt tydeligere, og vi
fik indtryk af, at den arbejdsgang kunden i virkeligheden ville få mest ud af
er, at oprette begivenheder i `www.aakb.dk` som herefter kopieres til
`www.ulfiaarhus.dk`.

Når man opretter begivenheder på `www.aakb.dk` får man struktureret data, og
dette er betydeligt nemmere at arbejde med end ustruktureret data.

Som Mikkel udlægger arbejdsgangen er den følgende

1. Opret begivenhed og datoer i pretix
2. Opret begivenhed i `www.aakb.dk`
3. Opret begivenhed i `www.ulfiaarhus.dk`

Ved at lægge for med at oprette i `www.aakb.dk`, vil man kunne få gavn af
integrationen med pretix og dermed slippe for den manuelle oprettelse i pretix.

### Udfordringer ved denne løsning

Der skal undersøges om

- `www.ulfiaarhus.dk` har et post/put api
- arbejdsgangen med at sende begivenheder til godkendelse skal stadig være
  håndholdt

