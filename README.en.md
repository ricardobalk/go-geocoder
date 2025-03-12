
# go-geocoder

> Graph-powered geodata. Fast, scalable, and stable.

A modern, graph-based solution for Dutch postcode and address data. No bulky SQL tables or outdated systems, but a sleek, extensible system written in Go with Neo4j as the backend. Data is accessible via REST, GraphQL, and Cypher.

## What can you do with it?

Answer practical questions like:

- In which municipality is Dutch postal code 3011AB located?
- Which postcodes are within 15 km of a location?
- What is the nearest address to these coordinates?
- How many street names are there in the Dutch city 'Leiden'?
- How far apart are two postcodes?
- Is house number 10A valid at this address?

In addition, it’s relatively easy to add extra source data and then export data for your own use.

**Why use it?**
- Super fast: Graph indexing, optimized for making connections between data points.
- Flexible: REST, GraphQL, Cypher.
- Extensible: Easily link additional datasets (BAG, CBS, NWB, Nominatim, etc.)
- Open Source: Full control, no vendor lock-in. Plus, you’ll learn a lot from it.

**Who is it for?**
- Software developers and data scientists working with geodata.
- Consultants and organizations in mobility, logistics, enforcement, etc.
- Anyone who values speed, control, and quality.

## Getting started

Make sure you have Git and Docker installed. Typically, all applications I develop will be containerized. If you’re using VSCode, you can even open the entire development environment in a Dockerized setup, without having to install all kinds of extra software manually. It’s 2025, not 1997.

```sh
mkdir ~/Projects/go-geocoder && cd $_
git clone https://github.com/ricardobalk/go-geocoder.git
docker-compose up -d
```

Example REST request:

```sh
curl -s "http://localhost:8080/v1/postcode/3011AB"
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

## License

EUPL v1.2 — European open source license. Free to use, modify, and distribute. Compatible with GPL, LGPL, MPL, and more. More info: https://joinup.ec.europa.eu/collection/eupl/eupl-text-eupl-12