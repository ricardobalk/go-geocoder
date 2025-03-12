
# go-geocoder

> Graph-powered geodata at scale.

Implementatie van een postcode-naar-plaats systeem — maar dan echt goed. Geen logge legacy-code in COBOL, geen SQL-harkwerk. Gewoon strak, met slimme relaties en goede algorithmische optimalisaties. Geschreven in Go. Gegevens opvragen via Cypher, REST en/of GraphQL.

Dit project maakt onder andere gebruik van de geweldige open data en conversietools uit [`bagconv`](https://github.com/berthubert/bagconv) van **Bert Hubert**. Serieus vakwerk. Zijn werk vormt een mooie basis voor o.a. geolocaties, adressen en koppelingen naar BAG. Daarnaast maakt dit project gebruik van gegevens van 

Maar in dit project trekken we het breder: We gaan helemaal gek. Alles gaat los, volledig genormaliseerd, gekoppeld, en keihard geoptimaliseerd voor snelheid en de mogelijkheid om custom datasets samen te stellen en te exporteren. Geen gehannes met spreadsheets, geen SQL-geploeter, geen trage queries — gewoon gas erop als een RS6 met gestolen kentekenplaten.

Dit is geen alternatief, dit is een upgrade.

## Wat kun je ermee?

- **Gegevens opvragen als een baas:**
  - Alle postcodes in plaats/provincie.
  - Postcodes binnen 15km van een coördinaat vinden.
  - Wat is de langste straatnaam van Noord-Holland eigenlijk?
  - Straat-, wijk-, en gemeentestatistieken zien.
  - Dichtstbijzijnde postcode bij coördinaat.
  - Coördinaat naar adres en andersom.
  - Is Groningen nou echt zo ver weg? 😉
  - Afstand tussen adres A en B. Binnenkort ook met reisafstand op basis van het [Nationaal Wegenbestand](https://www.nationaalwegenbestand.nl/nwb-downloaden). Maar shapefiles naar bruikbaar GeoJSON omzetten, verwerken en importeren, kost nog wat liefde (en wat rekenkracht 😉), maar het staat op de roadmap.
  - Alle straatnamen binnen 'n specifieke gemeente. Of alle straatnamen van Zuid-Holland, dat kan natuurlijk ook.
  - BAG/CBS koppelingen op plaats of postcode.
  - Even checken of een huisnummer-toevoeging klopt.
  - Gegevens verrijken met je eigen datasets, heel dik.
  - Complete exports voor data science nerds, of omdat iemand van de gemeente om een lijst vroeg, prima.
  - Valt 'n adres binnen een bepaald servicegebied?
  - Routing, clustering, heatmaps — je verzint het maar!

- **Supersnel:**  
  Alles is voorgekookt met relaties en slimme graph-indexing. Denk in nodes en relaties, niet in rijen. We zitten hier niet met onze klamme handjes door tabellen te harken.

- **Schaalbaar en uitbreidbaar:**  
  Nieuwe datasets? Meer granulariteit? CBS, Kadaster, Wijken, Straatniveaus, BRP-gegevens, laadpalen van VNG? Geen probleem. Ga helemaal los.

## Voor wie?

- 	Developers die begrijpen dat een join geen liefde biedt en liever met gerichte data werken dan te harken door tabellen met spaghettidata.
- Organisaties die locatiegegevens willen combineren, verrijken en graag razendsnel doorzoekbaar hebben, het liefst op hun eigen infrastructuur.
- Data scientists die geen zin hebben om 10 verschillende Excel-bestanden te combineren voor één kaartje.
- Consultants die niet afhankelijk willen zijn van dure APIs of verouderde databronnen.
- (Geo-)nerds die zelf de baas willen zijn over hun data, infrastructuur én queries.
- Personen en organisaties werkzaam in de logistiek, mobiliteit, handhaving, kadaster, en vergelijkbare vakgebieden waar je te maken hebt met adresgegevens.

## Waarom is dit lit?

- Open architectuur.
- Snel, flexibel en uitbreidbaar.
- Geen vendor lock-in.
- Geen SQL joins meer, dus laagdrempeliger.
- Meer controle over je eigen data.

## Tech Stack

Niet geheel onbelangrijk &mdash; waar is dit mee gebouwd?

- **Golang** – Lichtgewicht en snelle back-end. Ja, het kan ook met Rust. Maak maar een PR aan. 😉
- **Neo4j** – De kern. Geen joins, niet moeilijk. Best overzichtelijk:
  ```cypher
  MATCH (pc:PostalCode { identifier: '3011AB' })
        -[:IN_CITY]
        ->(c:City) 
  RETURN c.name; // => Rotterdam
  ```
  P.S. Ik zal binnenkort veel meer voorbeelden toevoegen ;-)

- **GraphQL (gqlgen)** – Voor flexibele toegang.
- **REST API** – Voor klassieke integraties.
- **Docker** – Voor makkelijk opstarten en deployen.
<!-- - **Kubernetes** – Voor uitrollen op grotere schaal. -->

---

# Let's go!

Nu de wat meer technische instructies om aan de slag te gaan met dit project.

## Docker, clone repo, bam.

- Zorg dat je Docker hebt. Voor Windows en Mac heb je Docker Desktop. Echte eindbazen gebruiken natuurlijk Linux. ;-)

- Clone de repo: `git clone https://github.com/ricardobalk/terranode.git`

- Start de containers: `docker-compose up -d`


### Services

De opzet van de Docker containers is momenteel als volgt:

<!-- Wellicht dit -->
- `

- `go-geocoder-database`:
  Database waar alle gegevens in staan, is mega goed in jou te vertellen welke postcodes in een bepaalde plaats liggen, welke plaatsen binnen 10km van een coördinaat zitten, of om de kortste afstand tussen twee postcodes uit te spugen. En dat binnen enkele milliseconden met makkelijk leesbare queries.

- `go-geocoder-worker-*`: Workers die op de achtergrond periodiek data ophalen, importeren en/of verwerken, met minimale downtime, zodat jij je kan richten op je eigen werk. 

- `go-geocoder-query-gateway`: 
  REST en GraphQL APIs gemaakt in Go, geeft toegang tot de hele bende aan externe integraties.

<!--
### Voorbeeld van Queries

#### GraphQL

Hieronder heb je wat GraphQL-queries die je op weg helpen.

```graphql
query {
  plaats(naam: "Rotterdam") {
    postcodes {
      code
      locatie {
        latitude
        longitude
      }
    }
  }
}
```

```graphql
query {
  afstand(postcodeA: "3011AB", postcodeB: "1012JS") {
    kilometers
    meters
  }
}
```
-->

## Bijdragen aan dit project

- Vragen? Prima.
- Problemen? Los het op.
- Feedback? Alleen als er code bij zit.

## Licentie

**EUPL v1.2** — Europese open source-licentie. Kort samengevat:

**Voordelen**
- Vrij te gebruiken, wijzigen en delen.
- Juridisch afgestemd op Europees recht.
- Geen vendor lock-in.
- Compatibel met GPL, LGPL, MPL en meer.

**Aandachtspunten**
- Minder bekend buiten Europa, maar juridisch ijzersterk binnen de EU.
- Als je de software (of een afgeleid werk) distribueert, moet je ook de broncode meeleveren en het onder een passende open source-licentie beschikbaar stellen.

Meer informatie: https://joinup.ec.europa.eu/collection/eupl/eupl-text-eupl-12