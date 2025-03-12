# go-geocoder

> Graph-powered geodata at scale.

A postcode-to-place system — but really well done. No clunky legacy COBOL code, no SQL hacking. Just sleek, with smart relationships and top-notch algorithmic optimisations. Written in Go. Fetch data via Cypher, REST, and/or GraphQL.

This project makes use of the amazing open data and conversion tools from [`bagconv`](https://github.com/berthubert/bagconv) by **Bert Hubert**. Serious craftsmanship. His work provides a solid foundation for, among other things, geolocations, addresses, and links to the BAG. Additionally, this project uses data from 

But we’re taking this project further: We’re going all out. Everything is fully normalised, connected, and brutally optimised for speed and the ability to create and export custom datasets. No messing around with spreadsheets, no SQL struggles, no slow queries — just full throttle like an RS6 with stolen plates.

This isn’t an alternative, it’s an upgrade.

## What can you do with it?

- **Query data like a boss:**
  - All postcodes in a town/province.
  - Find postcodes within 15km of a coordinate.
  - What’s the longest street name in North Holland, actually?
  - View street, neighbourhood, and municipality statistics.
  - Nearest postcode to a coordinate.
  - Coordinate to address and vice versa.
  - Is Groningen really that far away? 😉
  - Distance between address A and B. Soon with travel distance based on the [National Road Database](https://www.nationaalwegenbestand.nl/nwb-downloaden). But converting shapefiles to usable GeoJSON, processing them, and importing is still a bit of a work in progress (and requires some computing power 😉), but it’s on the roadmap.
  - All street names within a specific municipality. Or all street names in South Holland, of course.
  - BAG/CBS links by place or postcode.
  - Check if a house number extension is correct.
  - Enrich data with your own datasets, very slick.
  - Full exports for data science nerds, or because someone from the council asked for a list, no problem.
  - Does an address fall within a certain service area?
  - Routing, clustering, heatmaps — you name it!

- **Blistering speed:**  
  Everything’s pre-cooked with relationships and clever graph indexing. Think in nodes and relationships, not rows. We’re not here fumbling through tables with clammy hands.

- **Scalable and expandable:**  
  New datasets? More granularity? CBS, Kadaster, Neighbourhoods, Street levels, BRP data, VNG charging stations? No problem. Go wild.

## Who’s it for?

- Developers who understand that a join doesn’t offer any love and prefer to work with targeted data rather than digging through tables of spaghetti data.
- Organisations that want to combine, enrich, and make location data quickly searchable, preferably on their own infrastructure.
- Data scientists who can’t be bothered to combine 10 different Excel files just to make one little map.
- Consultants who don’t want to rely on expensive APIs or outdated data sources.
- (Geo-)nerds who want to be in control of their own data, infrastructure, and queries.
- Individuals and organisations working in logistics, mobility, enforcement, cadastre, and similar fields where you deal with address data.

## Why is this lit?

- Open architecture.
- Fast, flexible, and expandable.
- No vendor lock-in.
- No more SQL joins, making it easier to use.
- More control over your own data.

## Tech Stack

Not entirely unimportant — what’s this built with?

- **Golang** – Lightweight and fast backend. Yes, it can also be done with Rust. Feel free to submit a PR. 😉
- **Neo4j** – The core. No joins, no fuss. Pretty straightforward:
  ```cypher
  MATCH (pc:PostalCode { identifier: '3011AB' })
        -[:IN_CITY]
        ->(c:City) 
  RETURN c.name; // => Rotterdam
  ```
  P.S. I’ll be adding many more examples soon ;-)

- **GraphQL (gqlgen)** – For flexible access.
- **REST API** – For classic integrations.
- **Docker** – For easy startup and deployment.
<!-- - **Kubernetes** – For large-scale deployments. -->

---

# Let's go!

Now for the more technical instructions to get started with this project.

## Docker, clone repo, bam.

- Make sure you have Docker. For Windows and Mac, you’ll need Docker Desktop. Proper legends use Linux, obviously. ;-)

- Clone the repo: `git clone https://github.com/ricardobalk/terranode.git`

- Start the containers: `docker-compose up -d`


### Services

The setup of the Docker containers is currently as follows:

<!-- Maybe this -->
- `go-geocoder-database`:  
  The database where all the data lives. It’s fantastic at telling you which postcodes are in a particular place, which places are within 10km of a coordinate, or to spit out the shortest distance between two postcodes. And it does all that in mere milliseconds with easy-to-read queries.

- `go-geocoder-worker-*`:  
  Workers that periodically fetch, import, and/or process data in the background with minimal downtime, so you can focus on your own work.

- `go-geocoder-query-gateway`:  
  REST and GraphQL APIs made in Go, giving you access to the whole lot for external integrations.

<!--
### Example Queries

#### GraphQL

Here are some example GraphQL queries to get you started.

```graphql
query {
  place(name: "Rotterdam") {
    postcodes {
      code
      location {
        latitude
        longitude
      }
    }
  }
}
```

```graphql
query {
  distance(postcodeA: "3011AB", postcodeB: "1012JS") {
    kilometres
    metres
  }
}
```
-->

## Contributing to this project

- Questions? Cool.
- Issues? Fix it.
- Feedback? Only if there’s code attached.

## License

**EUPL v1.2** — European open-source license. In a nutshell:

**Benefits**
- Free to use, modify, and share.
- Legally aligned with European law.
- No vendor lock-in.
- Compatible with GPL, LGPL, MPL, and more.

**Things to keep in mind**
- Less well-known outside Europe, but legally solid within the EU.
- If you distribute the software (or a derivative work), you must also provide the source code and make it available under a suitable open-source license.

More information: https://joinup.ec.europa.eu/collection/eupl/eupl-text-eupl-12