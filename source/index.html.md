---
title: Cities API Documentation

language_tabs:
  - shell: cURL
  - javascript: JavaScript
  - python: Python

toc_footers:
  - <a href='https://rapidapi.com/hthought/api/cities-by-api-ninjas'>Subscribe on RapidAPI</a>
  - <a href='https://github.com/slatedocs/slate'>Powered by Slate</a>

search: true

code_clipboard: true

meta:
  - name: description
    content: Cities API - Geographic data API with 86 endpoints covering places, countries, timezones, and more
---

# Introduction

Everything is a place. The Cities API gives you unified access to 13+ million geographic features — cities, lakes, mountains, airports, parks — through one consistent interface. Query any feature type the same way: search, filter, calculate distances, find what's nearby.

Plus 252 countries, 418 timezones, and 1.8 million postal codes. **86 fast REST endpoints.**

## Everything is a Place

Most geo APIs treat cities, airports, and natural features as separate resources with different endpoints and parameters. We don't.

This means you can do things other APIs can't:

| Question | Other APIs | Cities API |
|----------|-----------|------------|
| Distance from a city to a lake? | ❌ Different endpoints, manual calculation | ✅ `/places/distance?from=5128581&to=5554072` |
| Airports near a mountain? | ❌ No relationship between features | ✅ `/places/{mountainId}/nearby?class=S&featureCode=AIRP&radius=100` |
| Timezone of the nearest city to coordinates? | ❌ Cities API only, no reverse geocoding | ✅ `/places/reverse?latitude=...&longitude=...` |
| Distance between largest port and smallest park? | ❌ Ports and parks in different databases | ✅ Same `/places` endpoint, filter by type |
| Sunrise time at a lake? | ❌ Astronomy APIs don't know about lakes | ✅ `/places/{lakeId}/sun-times?date=...` |
| All features within a bounding box? | ❌ Need separate calls per feature type | ✅ `/places/bbox?north=...&south=...` |
| Midpoint between an airport and a seaport? | ❌ No cross-type calculations | ✅ `/places/midpoint?ids={airport},{port}` |

**Real examples that work right now:**

```shell
# Distance from New York City to Lake Tahoe
curl "https://cities-api.p.rapidapi.com/places/distance?from=5128581&to=5554072" \
  -H "X-RapidAPI-Key: YOUR_API_KEY"

# Nearest airport to Mount Everest
curl "https://cities-api.p.rapidapi.com/places/1283416/nearby?class=S&featureCode=AIRP&radius=200" \
  -H "X-RapidAPI-Key: YOUR_API_KEY"
```

One data model. Infinite possibilities.

## Real-World Scenarios

See how developers use the Cities API to build powerful features in just a few calls. These scenarios show the complete journey — starting from search (no magic codes) to getting exactly the data you need.

### 1. Ski Trip Planning (Tourism)

**Goal:** Find the best Alps skiing destination with convenient access.

| Step | What | Endpoint | Result |
|------|------|----------|--------|
| 1 | Find Switzerland | `/places?class=A&q=Switzerland` | Country code → CH |
| 2 | Discover mountain code | `/feature-codes?class=T` | Mountain → MT |
| 3 | Find high peaks | `/places?country=CH&class=T&featureCode=MT&minElevation=3000` | Dom (4545m), Matterhorn (4478m) |
| 4 | Check airport access | `/places/2659729/nearby?class=S&featureCode=AIRP&radius=100` | Sion Airport (37km) |
| 5 | Find lodging | `/places/2659729/nearby?class=P&radius=30` | Breuil-Cervinia (5km, pop: 753) |

**How this works:** Start with a country name search, discover the right feature codes, then filter precisely by elevation. No guessing codes — the API tells you what's available.

### 2. Supply Chain Planning (Logistics)

**Goal:** Set up import/export hub with sea and air access.

| Step | What | Endpoint | Result |
|------|------|----------|--------|
| 1 | Discover city class | `/feature-classes` | Populated places → P |
| 2 | Find major port city | `/places?class=P&q=Rotterdam&country=NL&minPopulation=100000` | Rotterdam (868k pop) |
| 3 | Find nearby seaports | `/seaports?q=Rotterdam&limit=5` | Waalhaven Port, Rotterdam Port |
| 4 | Find cargo airports | `/places/2747891/nearby?class=S&featureCode=AIRP&radius=80` | Rotterdam The Hague Airport (4.7km) |

**How this works:** Discover available classes and codes first, search by name with population filters, then use proximity endpoints. The API guides you through what's available.

### 3. Digital Nomad Search (Remote Work)

**Goal:** Find affordable cities with good infrastructure in Southeast Asia.

| Step | What | Endpoint | Result |
|------|------|----------|--------|
| 1 | Find Thailand code | `/places?class=P&q=Bangkok&country=TH` | Bangkok → TH |
| 2 | Discover city class | `/feature-classes` | Populated places → P |
| 3 | Find mid-size Thai cities | `/places?country=TH&class=P&q=Chiang` | Chiang Mai (127k pop) |
| 4 | Check timezone | `/places/1153671` | UTC+7 (Asia/Bangkok), THB currency |
| 5 | Verify airport access | `/places/1153671/nearby?class=S&featureCode=AIRP&radius=100` | Chiang Mai Intl (3.2km) |

**How this works:** Find country codes from city searches, discover feature classes for filtering, then narrow down with population ranges. Local info gives timezone for remote meetings, and nearby airport means easy travel.

### 4. Geographic Discovery Challenge

**Goal:** Find the climate zone and tomorrow's sunrise time at the nearest city to the exact opposite point of Mount Everest on Earth.

| Step | What | Endpoint | Result |
|------|------|----------|--------|
| 1 | Discover feature types | `/feature-classes` | Terrain → class T, Populated → class P |
| 2 | Discover terrain codes | `/feature-codes?class=T` | Mountain → code MT |
| 3 | Find Mount Everest | `/places?country=NP&class=T&featureCode=MT&minElevation=8000` | Mount Everest (id: 1283416), 8,848m |
| 4 | Calculate antipode | `/places/1283416/antipode` | -27.99°S, -93.07°W (Pacific Ocean) |
| 5 | Find nearest city | `/places/reverse?latitude=-27.99&longitude=-93.07` | San Juan Bautista, Chile (1,496 km) |
| 6 | Get climate zone | `/places/3873019/climate-zone` | Cfa (Humid Subtropical) |
| 7 | Get sunrise tomorrow | `/places/3873019/sun-times?date=2026-01-07` | Sunrise: 04:53 (America/Santiago) |

**How this works:** Start by discovering what feature classes exist (terrain vs populated places), then find specific codes (mountains). Search for the highest peaks in Nepal to locate Everest. Use the antipode endpoint to calculate the exact opposite point on Earth. Find the nearest populated place using reverse geocoding. Finally, get climate classification and astronomical data for that remote Chilean island town.

### 5. Nordic Border Challenge

**Goal:** Find the Nordic country that's NOT in the EU, get a postal code from its second-largest city, find the nearest airport, then calculate the distance to the nearest EU neighbor's capital and compare their currencies, time zones, and electrical standards.

| Step | What | Endpoint | Result |
|------|------|----------|--------|
| 1 | Discover continents | `/continents` | Europe → EU |
| 2 | Find Nordic countries | `/countries?continent=EU&limit=100` | NO, SE, FI, DK, IS |
| 3 | Check EU membership | `/countries/eu-members` | SE, FI, DK in EU → Norway (NO) not |
| 4 | Discover city class | `/feature-classes` | Populated places → class P |
| 5 | Find top Norwegian cities | `/places?country=NO&class=P&limit=5` | 1. Oslo (1.08M), 2. Bergen (294k) |
| 6 | Get Bergen postal code | `/postal-codes/nearby?latitude=60.39&longitude=5.32` | 5003 Bergen |
| 7 | Find nearest airport | `/places/3161732/nearby?class=S&featureCode=AIRP&radius=50` | Bergen Flesland (12.5 km) |
| 8 | Get Norway's neighbors | `/countries/NO/neighbors` | FI, RU, SE (all except RU in EU) |
| 9 | Find nearest EU capital | `/countries/SE` → `/places/distance` | Stockholm (720.1 km) |
| 10 | Convert to miles | `/convert/distance?value=720.1&from=km&to=mi` | 447.45 miles |
| 11 | Compare timezones | `/places/.../` for both | Europe/Oslo vs Europe/Stockholm = 0 hrs |
| 12 | Compare currencies | Country endpoints | NOK (Krone) vs SEK (Krona) |
| 13 | Compare electrical | `/countries/NO/electrical` + `/countries/SE/electrical` | Identical: 230V, 50Hz, C & F |

**How this works:** Start by discovering continents, find Nordic countries in Europe. Use the EU membership endpoint to identify which Nordic country isn't in the EU (Norway). Discover the populated places class, then list Norwegian cities by population to find the second-largest (Bergen). Use coordinate-based postal code lookup, find the nearest airport, get Norway's neighbors to identify which are in the EU. Calculate distances to neighboring capitals, convert units, and compare local info and electrical standards.

## What's Included

| Category | What You Get |
|----------|--------------|
| **Places** | 13M+ features: cities, towns, villages, lakes, rivers, mountains, valleys, airports, parks, and more |
| **Search & Filter** | By country, name, feature class, feature code, population, elevation, bounding box |
| **Nearby & Proximity** | Find features near any place or coordinates, reverse geocoding |
| **Hierarchy** | Parent/child relationships, administrative divisions |
| **Geographic Calculations** | Distance, bearing, midpoint, route planning, along-route search, antipode, hemisphere |
| **Time & Astronomy** | Local time, UTC offset, timezone lookup, sun/moon rise/set times, time difference |
| **Climate** | Köppen climate classification for any location |
| **Travel** | Nearby airports, seaports, shipping hubs |
| **Local Info** | Combined timezone, currency, language, phone code for any place |
| **Population** | Rankings by country, region, and world |
| **Alternate Names** | Translations and alternate names in 40+ languages |
| **Countries** | 252 countries with neighbors, stats, electrical standards, driving side, formats |
| **Organizations** | G7, G20, BRICS, OPEC, NATO, EU, Schengen, Commonwealth, UN membership |
| **Timezones** | 418 timezones with current times, DST info, and offsets |
| **Postal Codes** | 1.8M postal codes with coordinates and nearby search |
| **Airports & Seaports** | Standalone search by country and name |
| **Capitals** | All world capital cities |
| **Reference Data** | Feature classes, feature codes, languages, currencies, continents, phone codes |
| **Validation** | Coordinate validation, land/water detection, postal code validation |
| **Unit Conversion** | Distance, elevation, area, temperature, speed, coordinates (metric ↔ imperial) |

## Features

- **13+ Million Places** — Cities, towns, lakes, rivers, mountains, airports, parks, and more
- **Unified Interface** — Query any feature type with the same parameters: class, featureCode, country
- **1.8 Million Postal Codes** — Full postal code database for 100+ countries
- **86 Endpoints** — Comprehensive coverage for any location-based use case
- **Multi-language Support** — Place names in 40+ languages
- **Distance & Proximity** — Calculate distance between any two places, find what's nearby
- **Hierarchy & Relationships** — Parent regions, child places, administrative divisions
- **Timezone Intelligence** — Local time, UTC offsets, time differences, sunrise/sunset times
- **Country Data** — 252 countries with neighbors, stats, electrical standards, driving sides
- **Political & Economic Blocs** — EU, NATO, G7, G20, BRICS, OPEC, Commonwealth, Schengen, UN
- **Geographic Calculations** — Bearing, antipode, midpoint, hemisphere, climate zones
- **Elevation Filtering** — Find mountains over 4000m, highest cities, terrain features
- **Weekly Updates** — Data refreshed weekly from GeoNames.org

## Use Cases

- **Travel Apps** — Find airports near any location, get local time, sunrise/sunset, currency
- **Shipping & Logistics** — Calculate distances, find nearby postal codes, validate addresses
- **Mapping & Search** — Reverse geocoding, bounding box queries, autocomplete
- **International Business** — EU/NATO membership, electrical standards, date formats, phone codes
- **Outdoor & Tourism** — Find lakes, mountains, parks; distances to natural features
- **Analytics & BI** — Enrich datasets with geographic metadata and hierarchies
- **E-commerce** — Validate addresses, estimate delivery zones, currency lookups
- **Weather & Climate** — Match coordinates to places, timezones, and climate zones

## Why Choose This API?

- **Complete Coverage** — Not just cities. Lakes, mountains, airports, parks — 13M+ features
- **Unified Interface** — One API pattern for all geographic features
- **Fast Response Times** — Optimized SQLite with sub-100ms queries
- **Simple Pricing** — No hidden fees, predictable costs
- **Clear Limits** — No rate surprises, no unexpected throttling
- **Reliable** — High uptime infrastructure

# Authentication

```shell
curl "https://cities-api.p.rapidapi.com/places?country=US&class=P" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places?country=US&class=P',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places',
    params={'country': 'US', 'class': 'P'},
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
```

All requests require authentication via RapidAPI:

| Header | Value |
|--------|-------|
| `X-RapidAPI-Key` | Your API key |
| `X-RapidAPI-Host` | `cities-api.p.rapidapi.com` |

# Pagination

```json
{
  "places": [...],
  "hasMore": true,
  "nextPage": "/places?country=US&class=P&offset=25&limit=25"
}
```

List endpoints return paginated results with descriptive field names:

| Parameter | Default | Max | Description |
|-----------|---------|-----|-------------|
| `limit` | 25 | 100 | Results per page |
| `offset` | 0 | — | Results to skip |

The response includes `hasMore` (boolean) and `nextPage` (URL or null) for easy iteration.

# Places

The `/places` endpoint is the core of the API. It provides access to all 13+ million geographic features.

## Feature Classes

| Class | Name | Examples | Count |
|-------|------|----------|-------|
| `P` | Populated Places | Cities, towns, villages | 5.2M |
| `H` | Hydrographic | Lakes, rivers, streams | 2.6M |
| `S` | Spots/Buildings | Airports, schools, churches | 2.5M |
| `T` | Terrain | Mountains, hills, valleys | 1.8M |
| `L` | Parks/Areas | Parks, reserves, regions | 0.5M |
| `A` | Administrative | Countries, states, counties | 0.5M |

## Search Places

```shell
curl "https://cities-api.p.rapidapi.com/places?country=US&class=P&q=New" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places?country=US&class=P&q=New',
  { headers: { 'X-RapidAPI-Key': 'YOUR_API_KEY', 'X-RapidAPI-Host': 'cities-api.p.rapidapi.com' }}
);
```

```python
response = requests.get(
    'https://cities-api.p.rapidapi.com/places',
    params={'country': 'US', 'class': 'P', 'q': 'New'},
    headers={'X-RapidAPI-Key': 'YOUR_API_KEY', 'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'}
)
```

> Response:

```json
{
  "places": [
    {
      "id": 5128581,
      "name": "New York City",
      "latitude": 40.71427,
      "longitude": -74.00597,
      "featureClass": "P",
      "featureCode": "PPL",
      "countryCode": "US",
      "region": "NY",
      "population": 8804190,
      "elevation": 10,
      "timezone": "America/New_York"
    }
  ],
  "hasMore": true,
  "nextPage": "/places?country=US&class=P&q=New&offset=25&limit=25"
}
```

Search and filter any geographic feature.

### HTTP Request

`GET /places`

### Query Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `country` | Yes* | ISO country code (required unless class specified) |
| `class` | Yes* | Feature class: P, H, T, S, L, A |
| `featureCode` | No | Specific type: PPL, LK, MT, AIRP, etc. |
| `excludeCodes` | No | Codes to exclude (comma-separated) |
| `q` | No | Search by name (requires country or class) |
| `minPopulation` | No | Minimum population |
| `maxPopulation` | No | Maximum population |
| `minElevation` | No | Minimum elevation (meters) |
| `maxElevation` | No | Maximum elevation (meters) |

## Get Place

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "id": 5128581,
  "name": "New York City",
  "asciiname": "New York City",
  "latitude": 40.71427,
  "longitude": -74.00597,
  "featureClass": "P",
  "featureCode": "PPL",
  "countryCode": "US",
  "region": "NY",
  "population": 8804190,
  "elevation": 10,
  "timezone": "America/New_York",
  "modifiedAt": "2024-01-15"
}
```

Get any feature by ID.

### HTTP Request

`GET /places/:id`

## Alternate Names

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/names?language=es" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "place": { "id": 5128581, "name": "New York City" },
  "names": [
    { "name": "Nueva York", "language": "es", "isPreferred": true },
    { "name": "NYC", "language": "en", "isShort": true }
  ]
}
```

Get translations and alternate names.

### HTTP Request

`GET /places/:id/names`

| Parameter | Description |
|-----------|-------------|
| `language` | Filter by language code (es, de, fr, etc.) |

## Parent (Hierarchy)

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/parent" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "place": { "id": 5128581, "name": "New York City" },
  "parent": {
    "id": 5128638,
    "name": "New York",
    "featureCode": "ADM1",
    "countryCode": "US"
  }
}
```

Get the administrative parent (state, province, region).

### HTTP Request

`GET /places/:id/parent`

## Children

```shell
curl "https://cities-api.p.rapidapi.com/places/5332921/children?class=P&limit=10" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "place": { "id": 5332921, "name": "California" },
  "places": [
    { "id": 5368361, "name": "Los Angeles", "population": 3898747 },
    { "id": 5391959, "name": "San Francisco", "population": 873965 }
  ],
  "hasMore": true,
  "nextPage": "/places/5332921/children?class=P&offset=10&limit=10"
}
```

Get places contained within this place.

### HTTP Request

`GET /places/:id/children`

| Parameter | Description |
|-----------|-------------|
| `class` | Filter by feature class |
| `featureCode` | Filter by feature code |

## Nearby Places

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/nearby?radius=50&class=P" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "places": [
    { "id": 5099836, "name": "Jersey City", "distance": 9.2 },
    { "id": 5106292, "name": "Newark", "distance": 14.3 }
  ],
  "hasMore": false,
  "nextPage": null,
  "reference": { "id": 5128581, "latitude": 40.71427, "longitude": -74.00597 }
}
```

Find features within a radius.

### HTTP Request

`GET /places/:id/nearby`

| Parameter | Default | Description |
|-----------|---------|-------------|
| `radius` | 50 | Radius in km |
| `class` | — | Filter by feature class |
| `featureCode` | — | Filter by feature code |
| `excludeCodes` | — | Codes to exclude |
| `minPopulation` | — | Minimum population |

## Nearby (by Coordinates)

```shell
curl "https://cities-api.p.rapidapi.com/places/nearby?latitude=40.7128&longitude=-74.006&radius=25" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Find features near any coordinates.

### HTTP Request

`GET /places/nearby`

| Parameter | Required | Description |
|-----------|----------|-------------|
| `latitude` | Yes | Latitude |
| `longitude` | Yes | Longitude |
| `radius` | No | Radius in km (default: 50) |

## Reverse Geocode

```shell
curl "https://cities-api.p.rapidapi.com/places/reverse?latitude=40.7128&longitude=-74.006" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "coordinates": { "latitude": 40.7128, "longitude": -74.006 },
  "nearest": {
    "id": 5128581,
    "name": "New York City",
    "distance": 0.2
  }
}
```

Find the nearest place to coordinates.

### HTTP Request

`GET /places/reverse`

## Bounding Box

```shell
curl "https://cities-api.p.rapidapi.com/places/bbox?north=41&south=40&east=-73&west=-75&class=P" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Find features within a geographic rectangle.

### HTTP Request

`GET /places/bbox`

| Parameter | Required | Description |
|-----------|----------|-------------|
| `north` | Yes | North latitude |
| `south` | Yes | South latitude |
| `east` | Yes | East longitude |
| `west` | Yes | West longitude |

## Distance

```shell
curl "https://cities-api.p.rapidapi.com/places/distance?from=5128581&to=2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "from": { "id": 5128581, "name": "New York City" },
  "to": { "id": 2643743, "name": "London" },
  "distance": 5570.2,
  "unit": "km"
}
```

Calculate distance between any two features.

### HTTP Request

`GET /places/distance`

| Parameter | Required | Description |
|-----------|----------|-------------|
| `from` | Yes | First place ID |
| `to` | Yes | Second place ID |

## Bearing

```shell
curl "https://cities-api.p.rapidapi.com/places/bearing?from=5128581&to=2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "from": { "id": 5128581, "name": "New York City" },
  "to": { "id": 2643743, "name": "London" },
  "bearing": 51.2,
  "compassDirection": "NE",
  "distance": 5570.2
}
```

Get compass bearing between places.

### HTTP Request

`GET /places/bearing`

## Midpoint

```shell
curl "https://cities-api.p.rapidapi.com/places/midpoint?ids=5128581,2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "midpoint": { "latitude": 52.34567, "longitude": -37.12345 },
  "inputPlaces": [
    { "id": 5128581, "name": "New York City" },
    { "id": 2643743, "name": "London" }
  ],
  "nearestPlace": { "id": 3421319, "name": "Nuuk", "distanceFromMidpoint": 1245.3 }
}
```

Find the geographic midpoint of multiple places.

### HTTP Request

`GET /places/midpoint`

## Route

```shell
curl "https://cities-api.p.rapidapi.com/places/route?ids=5128581,2643743,2988507" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Calculate an optimized route through multiple places (max 10).

### HTTP Request

`GET /places/route`

## Along Route

```shell
curl "https://cities-api.p.rapidapi.com/places/along-route?from=5128581&to=5368361&class=P" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Find places along a route between two points.

### HTTP Request

`GET /places/along-route`

## Compare Places

```shell
curl "https://cities-api.p.rapidapi.com/places/compare?ids=5128581,2643743,2988507" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Compare multiple places side by side (max 5).

### HTTP Request

`GET /places/compare`

## Extremes

```shell
curl "https://cities-api.p.rapidapi.com/places/extremes?country=US&class=P" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "northernmost": { "id": 5879400, "name": "Utqiagvik", "latitude": 71.29 },
  "southernmost": { "id": 5856195, "name": "Hilo", "latitude": 19.73 },
  "highest": { "id": 5427946, "name": "Leadville", "elevation": 3094 },
  "mostPopulous": { "id": 5128581, "name": "New York City", "population": 8804190 }
}
```

Find extreme places (northernmost, highest, etc).

### HTTP Request

`GET /places/extremes`

## Sun Times

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/sun-times?date=2026-06-21" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "place": { "id": 5128581, "name": "New York City" },
  "date": "2026-06-21",
  "sun": {
    "sunrise": "05:25",
    "sunset": "20:31",
    "solarNoon": "12:58",
    "daylightHours": 15.1
  },
  "timezone": "America/New_York"
}
```

Calculate sunrise, sunset, and day length.

### HTTP Request

`GET /places/:id/sun-times`

| Parameter | Default | Description |
|-----------|---------|-------------|
| `date` | Today | Date in YYYY-MM-DD format |

## Sun Times (by Coordinates)

```shell
curl "https://cities-api.p.rapidapi.com/places/sun-times?latitude=40.7128&longitude=-74.006&date=2026-06-21" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Get sun times for any coordinates.

### HTTP Request

`GET /places/sun-times`

## Local Time

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/local-time" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "place": { "id": 5128581, "name": "New York City" },
  "localTime": "2026-01-06T08:30:00-05:00",
  "timezone": "America/New_York",
  "utcOffset": -5
}
```

Get current local time.

### HTTP Request

`GET /places/:id/local-time`

## UTC Offset

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/utc-offset" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Get UTC offset information.

### HTTP Request

`GET /places/:id/utc-offset`

## Time at Coordinates

```shell
curl "https://cities-api.p.rapidapi.com/places/time-at?latitude=40.7128&longitude=-74.006" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Get current time at any coordinates.

### HTTP Request

`GET /places/time-at`

## Same Timezone

```shell
curl "https://cities-api.p.rapidapi.com/places/same-timezone?id=5128581" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Find other places in the same timezone.

### HTTP Request

`GET /places/same-timezone`

## Places by Currency

```shell
curl "https://cities-api.p.rapidapi.com/places/by-currency?code=EUR" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Find places that use a specific currency.

### HTTP Request

`GET /places/by-currency`

## Local Info

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/local-info" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "place": { "id": 5128581, "name": "New York City" },
  "country": { "code": "US", "name": "United States", "capital": "Washington" },
  "timezone": {
    "id": "America/New_York",
    "utcOffset": -5,
    "currentLocalTime": "2026-01-06T08:30:00-05:00"
  },
  "currency": { "code": "USD", "name": "US Dollar" },
  "phoneCode": "+1",
  "languages": ["en-US", "es-US"]
}
```

Get combined local information.

### HTTP Request

`GET /places/:id/local-info`

## Population Rank

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/population-rank" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "place": { "id": 5128581, "name": "New York City", "population": 8804190 },
  "rank": {
    "country": 1,
    "region": 1,
    "world": 24
  }
}
```

Get population rankings.

### HTTP Request

`GET /places/:id/population-rank`

## Metro Area

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/metro-area" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Get metropolitan area information.

### HTTP Request

`GET /places/:id/metro-area`

## Climate Zone

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/climate-zone" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "place": { "id": 5128581, "name": "New York City" },
  "climate": {
    "zone": "Cfa",
    "name": "Humid Subtropical",
    "description": "Hot, humid summers and mild winters",
    "hemisphere": "Northern",
    "tropicalZone": false
  }
}
```

Get Köppen climate classification.

### HTTP Request

`GET /places/:id/climate-zone`

## Antipode

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/antipode" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "place": { "id": 5128581, "name": "New York City" },
  "antipode": { "latitude": -40.71427, "longitude": 105.99403 },
  "nearestPlaceToAntipode": {
    "id": 1796236,
    "name": "Jiuquan",
    "distanceFromAntipode": 2847.3
  }
}
```

Find the opposite point on Earth.

### HTTP Request

`GET /places/:id/antipode`

## Hemisphere

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/hemisphere" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "place": { "id": 5128581, "name": "New York City" },
  "hemisphere": {
    "northSouth": "Northern",
    "eastWest": "Western"
  },
  "distanceFromEquator": 4524.8,
  "distanceFromPrimeMeridian": 5570.1
}
```

Get hemisphere location.

### HTTP Request

`GET /places/:id/hemisphere`

## Nearby Airports

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/airports" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "place": { "id": 5128581, "name": "New York City" },
  "airports": [
    { "id": 5122432, "name": "John F Kennedy International Airport", "distance": 19.2 },
    { "id": 5124497, "name": "LaGuardia Airport", "distance": 12.8 }
  ],
  "hasMore": false,
  "searchRadius": 100,
  "unit": "km"
}
```

Find airports near a place.

### HTTP Request

`GET /places/:id/airports`

| Parameter | Default | Description |
|-----------|---------|-------------|
| `radius` | 100 | Search radius in km |
| `limit` | 10 | Maximum results |

## Nearby Seaports

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/seaports" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Find seaports near a place.

### HTTP Request

`GET /places/:id/seaports`

## Shipping Hub

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/shipping-hub" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Find the nearest shipping/logistics hub.

### HTTP Request

`GET /places/:id/shipping-hub`

# Countries

## List Countries

```shell
curl "https://cities-api.p.rapidapi.com/countries" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Get all countries with pagination.

### HTTP Request

`GET /countries`

## Get Country

```shell
curl "https://cities-api.p.rapidapi.com/countries/US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "iso": "US",
  "iso3": "USA",
  "isoNumeric": "840",
  "fips": "US",
  "name": "United States",
  "capital": "Washington",
  "areaSqKm": 9629091,
  "population": 327167434,
  "continent": "NA",
  "tld": ".us",
  "currencyCode": "USD",
  "currencyName": "Dollar",
  "phone": "1",
  "postalCodeFormat": "#####-####",
  "postalCodeRegex": "^\\d{5}(-\\d{4})?$",
  "languages": "en-US,es-US,haw,fr",
  "geonameid": 6252001,
  "neighbours": "CA,MX,CU",
  "equivalentFipsCode": null
}
```

Get country details.

### HTTP Request

`GET /countries/:code`

## Neighbors

```shell
curl "https://cities-api.p.rapidapi.com/countries/FR/neighbors" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "country": { "code": "FR", "name": "France" },
  "neighbors": [
    { "code": "DE", "name": "Germany" },
    { "code": "ES", "name": "Spain" },
    { "code": "IT", "name": "Italy" },
    { "code": "BE", "name": "Belgium" },
    { "code": "CH", "name": "Switzerland" }
  ]
}
```

Get bordering countries.

### HTTP Request

`GET /countries/:code/neighbors`

## Country Stats

```shell
curl "https://cities-api.p.rapidapi.com/countries/US/stats" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Get detailed statistics about a country.

### HTTP Request

`GET /countries/:code/stats`

## Compare Countries

```shell
curl "https://cities-api.p.rapidapi.com/countries/compare?codes=US,JP,DE" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Compare multiple countries (max 5).

### HTTP Request

`GET /countries/compare`

## Landlocked

```shell
curl "https://cities-api.p.rapidapi.com/countries/landlocked" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Countries without coastline.

### HTTP Request

`GET /countries/landlocked`

## Islands

```shell
curl "https://cities-api.p.rapidapi.com/countries/islands" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Island nations.

### HTTP Request

`GET /countries/islands`

## Largest

```shell
curl "https://cities-api.p.rapidapi.com/countries/largest?by=area" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Largest countries by area or population.

### HTTP Request

`GET /countries/largest`

| Parameter | Default | Values |
|-----------|---------|--------|
| `by` | area | `area`, `population` |

## Smallest

```shell
curl "https://cities-api.p.rapidapi.com/countries/smallest?by=area" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Smallest countries by area or population.

### HTTP Request

`GET /countries/smallest`

## By Currency

```shell
curl "https://cities-api.p.rapidapi.com/countries/by-currency/EUR" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Countries using a currency.

### HTTP Request

`GET /countries/by-currency/:code`

## By Language

```shell
curl "https://cities-api.p.rapidapi.com/countries/by-language/es" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Countries where a language is spoken.

### HTTP Request

`GET /countries/by-language/:code`

## By Phone Code

```shell
curl "https://cities-api.p.rapidapi.com/countries/by-phone-code/1" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Countries by phone country code.

### HTTP Request

`GET /countries/by-phone-code/:code`

## By TLD

```shell
curl "https://cities-api.p.rapidapi.com/countries/by-tld/.de" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Country by top-level domain.

### HTTP Request

`GET /countries/by-tld/:tld`

## By Driving Side

```shell
curl "https://cities-api.p.rapidapi.com/countries/driving-side/left" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Countries by driving side (left or right).

### HTTP Request

`GET /countries/driving-side/:side`

## Electrical Standards

```shell
curl "https://cities-api.p.rapidapi.com/countries/US/electrical" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "country": "US",
  "countryName": "United States",
  "electrical": {
    "voltage": 120,
    "frequency": 60,
    "plugTypes": ["A", "B"]
  }
}
```

Voltage and plug types.

### HTTP Request

`GET /countries/:code/electrical`

## Localization

```shell
curl "https://cities-api.p.rapidapi.com/countries/localization/DE" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Localization settings (languages, number format).

### HTTP Request

`GET /countries/localization/:code`

## Format

```shell
curl "https://cities-api.p.rapidapi.com/countries/format/JP" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Date format, measurement system, driving side.

### HTTP Request

`GET /countries/format/:code`

## Calling Info

```shell
curl "https://cities-api.p.rapidapi.com/countries/calling/US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Phone calling information.

### HTTP Request

`GET /countries/calling/:code`

## Major Cities

```shell
curl "https://cities-api.p.rapidapi.com/countries/major-cities/JP" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Largest cities in a country.

### HTTP Request

`GET /countries/major-cities/:code`

# International Organizations

## G7 Members

```shell
curl "https://cities-api.p.rapidapi.com/countries/g7" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "organization": "G7",
  "description": "Group of Seven major advanced economies",
  "countries": [
    { "code": "US", "name": "United States" },
    { "code": "JP", "name": "Japan" },
    { "code": "DE", "name": "Germany" }
  ],
  "hasMore": true
}
```

### HTTP Request

`GET /countries/g7`

## G20 Members

`GET /countries/g20`

## BRICS Members

`GET /countries/brics`

## OPEC Members

`GET /countries/opec`

## NATO Members

`GET /countries/nato-members`

## EU Members

`GET /countries/eu-members`

## Schengen Area

`GET /countries/schengen`

## Commonwealth

`GET /countries/commonwealth`

## UN Members

`GET /countries/un-members`

# Timezones

## List Timezones

```shell
curl "https://cities-api.p.rapidapi.com/timezones" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

### HTTP Request

`GET /timezones`

## Get Timezone

```shell
curl "https://cities-api.p.rapidapi.com/timezones/America-New_York" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "timezoneId": "America/New_York",
  "countryCode": "US",
  "gmtOffset": -5,
  "dstOffset": -4,
  "rawOffset": -5
}
```

Note: Use `-` instead of `/` in timezone IDs (e.g., `America-New_York`).

### HTTP Request

`GET /timezones/:id`

## Timezone at Location

```shell
curl "https://cities-api.p.rapidapi.com/timezone-at?latitude=40.7128&longitude=-74.006" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Get timezone for coordinates.

### HTTP Request

`GET /timezone-at`

## Time Difference

```shell
curl "https://cities-api.p.rapidapi.com/time-difference?from=5128581&to=2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "from": { "id": 5128581, "name": "New York City", "timezone": "America/New_York" },
  "to": { "id": 2643743, "name": "London", "timezone": "Europe/London" },
  "difference": {
    "hours": 5,
    "description": "London is 5 hours ahead"
  }
}
```

### HTTP Request

`GET /time-difference`

# Postal Codes

## Search Postal Codes

```shell
curl "https://cities-api.p.rapidapi.com/postal-codes?country=US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

### HTTP Request

`GET /postal-codes`

| Parameter | Required | Description |
|-----------|----------|-------------|
| `country` | Yes | ISO country code |

## Lookup Postal Code

```shell
curl "https://cities-api.p.rapidapi.com/postal-codes/US/10001" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "postalCodes": [
    {
      "postalCode": "10001",
      "placeName": "New York",
      "region": "New York",
      "regionCode": "NY",
      "latitude": 40.7484,
      "longitude": -73.9967
    }
  ],
  "hasMore": false,
  "nextPage": null,
  "country": "US"
}
```

### HTTP Request

`GET /postal-codes/:country/:code`

## Nearby Postal Codes

```shell
curl "https://cities-api.p.rapidapi.com/postal-codes/nearby?latitude=40.7128&longitude=-74.006" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

### HTTP Request

`GET /postal-codes/nearby`

# Airports

```shell
curl "https://cities-api.p.rapidapi.com/airports?country=US&q=John" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Search airports by country and name.

### HTTP Request

`GET /airports`

| Parameter | Description |
|-----------|-------------|
| `country` | ISO country code |
| `q` | Search by name |

# Seaports

```shell
curl "https://cities-api.p.rapidapi.com/seaports?country=NL" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Search seaports by country and name.

### HTTP Request

`GET /seaports`

# Capitals

```shell
curl "https://cities-api.p.rapidapi.com/capitals" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

List all world capital cities.

### HTTP Request

`GET /capitals`

# Unit Conversion

All API responses use metric units (km, m, km²). Use these endpoints to convert.

## Distance

```shell
curl "https://cities-api.p.rapidapi.com/convert/distance?value=100&from=km&to=mi" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "original": { "value": 100, "unit": "km" },
  "converted": { "value": 62.137119, "unit": "mi" }
}
```

Units: `km`, `mi`, `m`, `ft`, `nm` (nautical miles)

### HTTP Request

`GET /convert/distance`

## Elevation

```shell
curl "https://cities-api.p.rapidapi.com/convert/elevation?value=3000&from=m&to=ft" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Units: `m`, `ft`

### HTTP Request

`GET /convert/elevation`

## Area

```shell
curl "https://cities-api.p.rapidapi.com/convert/area?value=1000&from=sqkm&to=sqmi" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Units: `sqkm`, `sqmi`, `ha`, `acre`

### HTTP Request

`GET /convert/area`

## Temperature

```shell
curl "https://cities-api.p.rapidapi.com/convert/temperature?value=20&from=c&to=f" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Units: `c`, `f`, `k`

### HTTP Request

`GET /convert/temperature`

## Speed

```shell
curl "https://cities-api.p.rapidapi.com/convert/speed?value=100&from=kmh&to=mph" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Units: `kmh`, `mph`, `knots`, `ms`

### HTTP Request

`GET /convert/speed`

## Coordinates

```shell
curl "https://cities-api.p.rapidapi.com/convert/coordinates?value=40.7128,-74.006&from=dd&to=dms" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Convert between decimal degrees and degrees-minutes-seconds.

### HTTP Request

`GET /convert/coordinates`

# Reference

## Feature Classes

```shell
curl "https://cities-api.p.rapidapi.com/feature-classes" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

All feature classes with counts.

### HTTP Request

`GET /feature-classes`

## Feature Codes

```shell
curl "https://cities-api.p.rapidapi.com/feature-codes" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

All GeoNames feature codes.

### HTTP Request

`GET /feature-codes`

## Currencies

```shell
curl "https://cities-api.p.rapidapi.com/currencies" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

### HTTP Request

`GET /currencies`

## Languages

```shell
curl "https://cities-api.p.rapidapi.com/languages" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

### HTTP Request

`GET /languages`

## Get Language

```shell
curl "https://cities-api.p.rapidapi.com/languages/es" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "code": "es",
  "name": "Spanish",
  "nativeName": "Español",
  "family": "Indo-European",
  "countries": [
    { "code": "ES", "name": "Spain" },
    { "code": "MX", "name": "Mexico" }
  ],
  "countryCount": 21
}
```

### HTTP Request

`GET /languages/:code`

## Continents

```shell
curl "https://cities-api.p.rapidapi.com/continents" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

### HTTP Request

`GET /continents`

## Get Continent

```shell
curl "https://cities-api.p.rapidapi.com/continents/EU" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Get continent details with countries.

### HTTP Request

`GET /continents/:code`

## Phone Codes

```shell
curl "https://cities-api.p.rapidapi.com/phone-codes" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

### HTTP Request

`GET /phone-codes`

# Validation

## Validate Coordinates

```shell
curl "https://cities-api.p.rapidapi.com/validate/coordinates?latitude=40.7128&longitude=-74.006" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "valid": true,
  "latitude": 40.7128,
  "longitude": -74.006,
  "onLand": true,
  "country": { "code": "US", "name": "United States" },
  "nearestPlace": { "name": "New York City", "distance": 0.2 }
}
```

Check if coordinates are valid and on land.

### HTTP Request

`GET /validate/coordinates`

## Validate Postal Code

```shell
curl "https://cities-api.p.rapidapi.com/validate/postal-code/US/10001" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Check if a postal code exists.

### HTTP Request

`GET /validate/postal-code/:country/:code`

# Errors

> Error format:

```json
{
  "error": {
    "message": "Place not found",
    "status": 404
  }
}
```

| Code | Meaning |
|------|---------|
| 200 | Success (even if results are empty) |
| 400 | Bad Request — missing or invalid parameters |
| 401 | Unauthorized — invalid API key |
| 404 | Not Found — resource doesn't exist |
| 500 | Server Error |
| 504 | Timeout — query took too long |

**Note:** Empty results return `200` with an empty array (e.g., `{ "places": [] }`), not `404`.
