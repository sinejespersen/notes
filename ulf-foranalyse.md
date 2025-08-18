# Foranalyse ULF og AAKB

Undersøgelse af Ulf som datakilde til oprettelse af begivenheder i Aakb. Den
ønskede arbejdsgang er, at man opretter et begivenhed på `www.ulfiaarhus.dk`,
som derefter kopieres til `www.aakb.dk`.

## Konsekvens ved at overføre data fra `www.ulfiaarhus.dk` til `www.aakb.dk`

**Hvis det er en forudsætning**, at man skal gå via `www.aakb.dk` for at købe
billet så kan det ikke lade sig gøre at `www.ulfiaarhus.dk` er stedet hvor man
opretter begivenheder, da man i sagens natur ikke kan henvise til uoprettede
begivenheder (note: dette er accepteret på møde d. 14. august).

## Forudsætninger for estimater
 
Opgavens kompleksitet og potentielle omfang, er dikteret af ustruktureret data, som løbende er beskrevet i denne analyse. Derfor er nedenstående estimater baseret på at der kan indarbejdes en stringent arbejdsgang og faste rammer for oprettelse af ny data i de omtalte systemer.
 
Det er vigtigt at undestrege at grundlaget for estimaterne er baseret på at der kan identificeres 1-3 skabeloner af begivenheder som der skal oprettes på `aakb.dk`. Noget som vi fik opfattelsen af var muligt, til mødet med Mikkel og Lisbeth d. 14. august.
 
Derudover er det også en forudsætning at der ikke ændres markant i udstillelse af data på `ulfiaarhus.dk`, da dette vil kunne ændre markant på opgavens omfang.
 
Den største usikkerhed i dette projekt er arbejdet med de ustruktureret data. Ved at introducere faste skabeloner, kan vi minimere opgavens kompleksitet på udvikler niveau, men introducere mere kompleksitet i oprettelsesprocessen, da en skabelon skal følges - Hvorvidt dette er en udfordring, kan vi ikke vurdere.
 
### Forudsætninger for opgaven
- Henvisning fra `ulfiaarhus.dk` til `aakb.dk` kan ikke lade sig gøre.
- Begivenheder der skal importeres til `aakb.dk` skal oprettes i aftalte skabeloner.
- Udstillingen af data via API på `ulfiaarhus.dk` ændres ikke.
- **Nedenstående er estimater**. Dette er vores bud på opgavens omfang og både tage kortere og længere tid. Alle forbrugte timer afregnes.

## Estimater

| Timer     | Beskrivelse                                                                                                                                | Usikkerhed |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------|-----|
| 50 timer | Projektledelse                                                                                                                               | Lav |
| 15 timer  | Opsætning af udviklingsmiljø                                                                                                               |Lav |
| 25 timer  | Sparring: code review og general vidensdeling                                                                                              |Lav |
| 50 timer  | Integration i DPL CMS: Hvilke begivenheder er oprettet/eventuelle rettelser i begivenheder, oprettelse af tabel til at holde styr på begivenheder |   Lav |                                                                                                                  |
| 30 timer   | Dokumentation af løsningen                                                                                                                 | Lav |
| 30 timer  | Møder om data/oprettelsesflow                                                                                                                 | Lav |
| 15 timer | Skabeloner til dataoprettelse i samarbejde med Aarhus Bibliotekerne | Lav |
| 40 timer | Udviklingsarbejde med parsing af skabeloner | Mellem |
| 40 timer | Feedback/tests | Høj |

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

Nedenstående er to forskellige begivenheder fra ulf-apiet, med providerid 934
(Aarhus Bibliotekerne), som vi har en forståelse af er det data som angår ITK
Dev.

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
