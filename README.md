# go-geocoder

> Graph-powered geodata. Snel, schaalbaar en stabiel.

Een moderne, graph-gebaseerde oplossing voor Nederlandse postcode- en adresdata. Geen logge SQL-tabellen of verouderde systemen, maar een strak, uitbreidbaar systeem geschreven in Go met Neo4j als backend. Data is toegankelijk via REST, GraphQL en Cypher.

## Wat kun je ermee?

Beantwoord praktische vragen zoals:

- In welke gemeente ligt postcode 3011AB?
- Welke postcodes liggen binnen 15 km van een locatie?
- Wat is het dichtstbijzijnde adres bij deze coördinaten?
- Hoeveel straatnamen telt Leiden?
- Hoe ver liggen twee postcodes van elkaar?
- Is huisnummer 10A geldig op dit adres?

Daarnaast is het dus mogelijk om op relatief eenvoudige manier extra brongegevens toe te voegen en vervolgens gegegevens te exporteren voor eigen gebruik.

**Waarom gebruiken?**
- Supersnel: Graph-indexing, geoptimaliseerd voor het leggen van verbanden tussen gegevens.
- Flexibel: REST, GraphQL, Cypher.
- Uitbreidbaar: Koppel eenvoudig aanvullende datasets (BAG, CBS, NWB, Nominatim, enz.)
- Open Source: Volledige controle, geen vendor lock-in. Je leert er ook nog eens veel van.

**Voor wie?**
- Softwareontwikkelaars en data scientists die met geodata werken.
- Consultants en organisaties in mobiliteit, logistiek, handhaving, etc.
- Iedereen die snelheid, controle en kwaliteit belangrijk vindt.

## Aan de slag

Zorg dat je Git en Docker hebt geinstalleerd. Alle applicaties die ik ontwikkel zijn doorgaans containerized opgezet. Als je VSCode gebruikt, kun je zelfs de gehele ontwikkelomgeving openen in een Dockerized-omgeving, zonder dat je allerlei extra software handmatig moet installeren. Het is 2025, niet 1997.

```sh
mkdir ~/Projects/go-geocoder && cd $_
git clone https://github.com/ricardobalk/go-geocoder.git
docker-compose up -d
```

Voorbeeld REST-request:

```sh
curl -s http://localhost:8080/v1/postcode/3011AB
```

```json
{
  "postcode": "3011AB",
  "plaats": "Rotterdam",
  "provincie": "Zuid-Holland",
  "coordinaten": {
    "lat": 51.9202,
    "lon": 4.4818
  }
}
```

```sh
curl -s "http://localhost:8080/v1/distance?postcodeA=3011AB&postcodeB=1012JS" 
```

```json
{
  "distance_haversine": 59800,
  "distance_path": 62800
}
```

## Licentie

EUPL v1.2 — Europese open source-licentie. Vrij te gebruiken, wijzigen en verspreiden. Compatibel met GPL, LGPL, MPL en meer. Meer info: https://joinup.ec.europa.eu/collection/eupl/eupl-text-eupl-12