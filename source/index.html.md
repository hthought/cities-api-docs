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

## Search places

```shell
curl "https://cities-api.p.rapidapi.com/places?q=New&country=US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places?q=New&country=US',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places',
    params={
        'q': 'New',
        'country': 'US'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Search all geographic features with flexible filtering

### HTTP Request

`GET /places`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `q` | string | No |  |
| `country` | string | No |  |
| `class` | string | No |  |
| `featureCode` | string | No |  |
| `excludeCodes` | string | No |  |
| `minPopulation` | string | No |  |
| `maxPopulation` | string | No |  |
| `minElevation` | string | No |  |
| `maxElevation` | string | No |  |
| `limit` | string | No |  |
| `offset` | string | No |  |

## Reverse geocoding

```shell
curl "https://cities-api.p.rapidapi.com/places/reverse?latitude=40.7128&longitude=-74.006&radius=50" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/reverse?latitude=40.7128&longitude=-74.006&radius=50',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/reverse',
    params={
        'latitude': '40.7128',
        'longitude': '-74.006',
        'radius': '50'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Find the nearest place to coordinates

### HTTP Request

`GET /places/reverse`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `latitude` | string | Yes |  |
| `longitude` | string | Yes |  |
| `radius` | string | No |  |

## Find nearby places

```shell
curl "https://cities-api.p.rapidapi.com/places/nearby?latitude=40.7128&longitude=-74.006&radius=50&limit=25" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/nearby?latitude=40.7128&longitude=-74.006&radius=50&limit=25',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/nearby',
    params={
        'latitude': '40.7128',
        'longitude': '-74.006',
        'radius': '50',
        'limit': '25'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Find all places near given coordinates

### HTTP Request

`GET /places/nearby`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `latitude` | string | Yes |  |
| `longitude` | string | Yes |  |
| `radius` | string | No |  |
| `limit` | string | No |  |
| `class` | string | No |  |
| `featureCode` | string | No |  |

## Calculate distance

```shell
curl "https://cities-api.p.rapidapi.com/places/distance?from=5128581&to=2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/distance?from=5128581&to=2643743',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/distance',
    params={
        'from': '5128581',
        'to': '2643743'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Calculate distance between two places

### HTTP Request

`GET /places/distance`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `from` | string | Yes |  |
| `to` | string | Yes |  |

## Search places in bounding box

```shell
curl "https://cities-api.p.rapidapi.com/places/bbox?north=41&south=40&east=-73&west=-75&limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/bbox?north=41&south=40&east=-73&west=-75&limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/bbox',
    params={
        'north': '41',
        'south': '40',
        'east': '-73',
        'west': '-75',
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /places/bbox`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `north` | string | Yes |  |
| `south` | string | Yes |  |
| `east` | string | Yes |  |
| `west` | string | Yes |  |
| `limit` | string | No |  |
| `offset` | string | No |  |
| `minPopulation` | string | No |  |
| `class` | string | No |  |
| `featureCode` | string | No |  |
| `excludeCodes` | string | No |  |

## Get sun times for coordinates

```shell
curl "https://cities-api.p.rapidapi.com/places/sun-times?lat=40.7128&lon=-74.006&date=2026-06-21" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/sun-times?lat=40.7128&lon=-74.006&date=2026-06-21',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/sun-times',
    params={
        'lat': '40.7128',
        'lon': '-74.006',
        'date': '2026-06-21'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /places/sun-times`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `lat` | string | Yes |  |
| `lon` | string | Yes |  |
| `date` | string | No |  |

## Find places along a route

```shell
curl "https://cities-api.p.rapidapi.com/places/along-route?from=5128581&to=2643743&radius=50&limit=25" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/along-route?from=5128581&to=2643743&radius=50&limit=25',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/along-route',
    params={
        'from': '5128581',
        'to': '2643743',
        'radius': '50',
        'limit': '25'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /places/along-route`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `from` | string | Yes |  |
| `to` | string | Yes |  |
| `radius` | string | No |  |
| `limit` | string | No |  |
| `offset` | string | No |  |
| `minPopulation` | string | No |  |
| `class` | string | No |  |
| `featureCode` | string | No |  |
| `excludeCodes` | string | No |  |

## Get extreme places

```shell
curl "https://cities-api.p.rapidapi.com/places/extremes?type=example&country=US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/extremes?type=example&country=US',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/extremes',
    params={
        'type': 'example',
        'country': 'US'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /places/extremes`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `type` | string | No |  |
| `country` | string | No |  |
| `limit` | string | No |  |
| `order` | string | No |  |
| `class` | string | No |  |
| `featureCode` | string | No |  |

## Calculate geographic midpoint

```shell
curl "https://cities-api.p.rapidapi.com/places/midpoint?ids=5128581,2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/midpoint?ids=5128581,2643743',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/midpoint',
    params={
        'ids': '5128581,2643743'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /places/midpoint`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `ids` | string | Yes |  |

## Calculate bearing between places

```shell
curl "https://cities-api.p.rapidapi.com/places/bearing?from=5128581&to=2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/bearing?from=5128581&to=2643743',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/bearing',
    params={
        'from': '5128581',
        'to': '2643743'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /places/bearing`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `from` | string | Yes |  |
| `to` | string | Yes |  |

## Find places in same timezone

```shell
curl "https://cities-api.p.rapidapi.com/places/same-timezone?id=5128581&limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/same-timezone?id=5128581&limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/same-timezone',
    params={
        'id': '5128581',
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /places/same-timezone`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes |  |
| `limit` | string | No |  |
| `offset` | string | No |  |
| `minPopulation` | string | No |  |

## Find places by currency

```shell
curl "https://cities-api.p.rapidapi.com/places/by-currency?code=US&limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/by-currency?code=US&limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/by-currency',
    params={
        'code': 'US',
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /places/by-currency`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | Yes |  |
| `limit` | string | No |  |
| `offset` | string | No |  |
| `minPopulation` | string | No |  |

## Get current time at coordinates

```shell
curl "https://cities-api.p.rapidapi.com/places/time-at?latitude=40.7128&longitude=-74.006" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/time-at?latitude=40.7128&longitude=-74.006',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/time-at',
    params={
        'latitude': '40.7128',
        'longitude': '-74.006'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Get current time at specific geographic coordinates

### HTTP Request

`GET /places/time-at`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `latitude` | string | Yes |  |
| `longitude` | string | Yes |  |

## Calculate route between waypoints

```shell
curl "https://cities-api.p.rapidapi.com/places/route?ids=5128581,2643743&start=example&end=example" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/route?ids=5128581,2643743&start=example&end=example',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/route',
    params={
        'ids': '5128581,2643743',
        'start': 'example',
        'end': 'example'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /places/route`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `ids` | string | Yes |  |
| `start` | string | No |  |
| `end` | string | No |  |
| `roundTrip` | string | No |  |

## Compare two places

```shell
curl "https://cities-api.p.rapidapi.com/places/compare?ids=5128581,2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/compare?ids=5128581,2643743',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/compare',
    params={
        'ids': '5128581,2643743'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /places/compare`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `ids` | string | Yes |  |

## Get place by ID

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/5128581',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/5128581',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Get detailed information about a specific place

### HTTP Request

`GET /places/{id}`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes |  |

## Get antipode

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/antipode" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/5128581/antipode',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/5128581/antipode',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Get the antipode (opposite point on Earth) for a place

### HTTP Request

`GET /places/{id}/antipode`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes |  |

## Get hemisphere information

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/hemisphere" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/5128581/hemisphere',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/5128581/hemisphere',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Get hemisphere and geographical position information

### HTTP Request

`GET /places/{id}/hemisphere`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes |  |

## Find nearby places

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/nearby?radius=50&limit=25" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/5128581/nearby?radius=50&limit=25',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/5128581/nearby',
    params={
        'radius': '50',
        'limit': '25'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Find places near a specific place

### HTTP Request

`GET /places/{id}/nearby`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `radius` | string | No |  |
| `limit` | string | No |  |
| `class` | string | No |  |
| `featureCode` | string | No |  |

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes |  |

## Get sun times

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/sun-times?date=2026-06-21" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/5128581/sun-times?date=2026-06-21',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/5128581/sun-times',
    params={
        'date': '2026-06-21'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Get sunrise, sunset, and day length information

### HTTP Request

`GET /places/{id}/sun-times`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `date` | string | No |  |

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes |  |

## Get local information

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/local-info" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/5128581/local-info',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/5128581/local-info',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Get timezone, currency, phone code, and language information

### HTTP Request

`GET /places/{id}/local-info`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes |  |

## Get local time

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/local-time" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/5128581/local-time',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/5128581/local-time',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Get the current local time at a place

### HTTP Request

`GET /places/{id}/local-time`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes |  |

## Get UTC offset

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/utc-offset" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/5128581/utc-offset',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/5128581/utc-offset',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Get UTC offset and timezone information

### HTTP Request

`GET /places/{id}/utc-offset`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes |  |

## Get climate zone

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/climate-zone" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/5128581/climate-zone',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/5128581/climate-zone',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /places/{id}/climate-zone`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes |  |

## Get metro area

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/metro-area?radius=50" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/5128581/metro-area?radius=50',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/5128581/metro-area',
    params={
        'radius': '50'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Get metro area information including nearby populated places and aggregated population statistics

### HTTP Request

`GET /places/{id}/metro-area`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `radius` | string | No |  |

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes |  |

## Get shipping hub information

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/shipping-hub" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/5128581/shipping-hub',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/5128581/shipping-hub',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Analyzes if a place is a shipping hub based on nearby ports, airports, and population

### HTTP Request

`GET /places/{id}/shipping-hub`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes |  |

## Get parent place

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/parent" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/5128581/parent',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/5128581/parent',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /places/{id}/parent`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes |  |

## Get child places

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/children?class=P&featureCode=PPL" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/5128581/children?class=P&featureCode=PPL',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/5128581/children',
    params={
        'class': 'P',
        'featureCode': 'PPL'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /places/{id}/children`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `class` | string | No |  |
| `featureCode` | string | No |  |
| `excludeCodes` | string | No |  |
| `limit` | string | No |  |
| `offset` | string | No |  |

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes |  |

## Get alternate names

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/names?language=es" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/5128581/names?language=es',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/5128581/names',
    params={
        'language': 'es'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /places/{id}/names`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `language` | string | No |  |

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes |  |

## Get population rank for place

```shell
curl "https://cities-api.p.rapidapi.com/places/5128581/population-rank" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/places/5128581/population-rank',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/places/5128581/population-rank',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /places/{id}/population-rank`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes |  |

# Countries

## List all countries

```shell
curl "https://cities-api.p.rapidapi.com/countries?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |
| `continent` | string | No |  |
| `sort` | string | No |  |

## List landlocked countries

```shell
curl "https://cities-api.p.rapidapi.com/countries/landlocked?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/landlocked?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/landlocked',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/landlocked`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |
| `continent` | string | No |  |
| `sort` | string | No |  |

## List island nations

```shell
curl "https://cities-api.p.rapidapi.com/countries/islands?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/islands?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/islands',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/islands`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |
| `continent` | string | No |  |
| `sort` | string | No |  |

## Get country postal format

```shell
curl "https://cities-api.p.rapidapi.com/countries/format/US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/format/US',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/format/US',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/format/{code}`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | Yes |  |

## Get country names in all languages

```shell
curl "https://cities-api.p.rapidapi.com/countries/localization/US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/localization/US',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/localization/US',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/localization/{code}`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | Yes |  |

## Get country calling code

```shell
curl "https://cities-api.p.rapidapi.com/countries/calling/US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/calling/US',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/calling/US',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/calling/{code}`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | Yes |  |

## Get major cities in country

```shell
curl "https://cities-api.p.rapidapi.com/countries/major-cities/US?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/major-cities/US?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/major-cities/US',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/major-cities/{code}`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |
| `continent` | string | No |  |
| `sort` | string | No |  |

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | Yes |  |

## Get largest countries by area

```shell
curl "https://cities-api.p.rapidapi.com/countries/largest?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/largest?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/largest',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/largest`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |
| `continent` | string | No |  |
| `sort` | string | No |  |

## Get smallest countries by area

```shell
curl "https://cities-api.p.rapidapi.com/countries/smallest?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/smallest?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/smallest',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/smallest`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |
| `continent` | string | No |  |
| `sort` | string | No |  |

## Get countries by currency

```shell
curl "https://cities-api.p.rapidapi.com/countries/by-currency/US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/by-currency/US',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/by-currency/US',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/by-currency/{code}`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | Yes |  |

## Get countries by language

```shell
curl "https://cities-api.p.rapidapi.com/countries/by-language/US?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/by-language/US?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/by-language/US',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/by-language/{code}`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |
| `continent` | string | No |  |
| `sort` | string | No |  |

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | Yes |  |

## Get countries by driving side

```shell
curl "https://cities-api.p.rapidapi.com/countries/driving-side/left?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/driving-side/left?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/driving-side/left',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/driving-side/{side}`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |
| `continent` | string | No |  |
| `sort` | string | No |  |

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `side` | string | Yes |  |

## Get country by TLD

```shell
curl "https://cities-api.p.rapidapi.com/countries/by-tld/.de" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/by-tld/.de',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/by-tld/.de',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/by-tld/{tld}`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tld` | string | Yes |  |

## Get countries by phone code

```shell
curl "https://cities-api.p.rapidapi.com/countries/by-phone-code/US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/by-phone-code/US',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/by-phone-code/US',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/by-phone-code/{code}`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | Yes |  |

## List EU member countries

```shell
curl "https://cities-api.p.rapidapi.com/countries/eu-members?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/eu-members?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/eu-members',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/eu-members`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |
| `continent` | string | No |  |
| `sort` | string | No |  |

## List Schengen Area countries

```shell
curl "https://cities-api.p.rapidapi.com/countries/schengen?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/schengen?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/schengen',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/schengen`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |
| `continent` | string | No |  |
| `sort` | string | No |  |

## List NATO member countries

```shell
curl "https://cities-api.p.rapidapi.com/countries/nato-members?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/nato-members?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/nato-members',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/nato-members`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |
| `continent` | string | No |  |
| `sort` | string | No |  |

## List G7 countries

```shell
curl "https://cities-api.p.rapidapi.com/countries/g7?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/g7?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/g7',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/g7`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |
| `continent` | string | No |  |
| `sort` | string | No |  |

## List G20 countries

```shell
curl "https://cities-api.p.rapidapi.com/countries/g20?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/g20?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/g20',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/g20`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |
| `continent` | string | No |  |
| `sort` | string | No |  |

## List BRICS countries

```shell
curl "https://cities-api.p.rapidapi.com/countries/brics?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/brics?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/brics',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/brics`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |
| `continent` | string | No |  |
| `sort` | string | No |  |

## List OPEC countries

```shell
curl "https://cities-api.p.rapidapi.com/countries/opec?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/opec?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/opec',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/opec`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |
| `continent` | string | No |  |
| `sort` | string | No |  |

## List Commonwealth countries

```shell
curl "https://cities-api.p.rapidapi.com/countries/commonwealth?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/commonwealth?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/commonwealth',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/commonwealth`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |
| `continent` | string | No |  |
| `sort` | string | No |  |

## List UN member countries

```shell
curl "https://cities-api.p.rapidapi.com/countries/un-members?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/un-members?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/un-members',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/un-members`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |
| `continent` | string | No |  |
| `sort` | string | No |  |

## Compare two countries

```shell
curl "https://cities-api.p.rapidapi.com/countries/compare?codes=US,JP,DE" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/compare?codes=US,JP,DE',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/compare',
    params={
        'codes': 'US,JP,DE'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/compare`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `codes` | string | Yes |  |

## Get country by code

```shell
curl "https://cities-api.p.rapidapi.com/countries/US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/US',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/US',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/{code}`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | Yes |  |

## Get electrical standards for country

```shell
curl "https://cities-api.p.rapidapi.com/countries/US/electrical" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/US/electrical',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/US/electrical',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/{code}/electrical`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | Yes |  |

## Get neighboring countries

```shell
curl "https://cities-api.p.rapidapi.com/countries/US/neighbors" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/US/neighbors',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/US/neighbors',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/{code}/neighbors`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | Yes |  |

## Get country statistics

```shell
curl "https://cities-api.p.rapidapi.com/countries/US/stats" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/countries/US/stats',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/countries/US/stats',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /countries/{code}/stats`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | Yes |  |

# Timezones

## List all timezones

```shell
curl "https://cities-api.p.rapidapi.com/timezones?country=US&limit=25" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/timezones?country=US&limit=25',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/timezones',
    params={
        'country': 'US',
        'limit': '25'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Get a paginated list of all timezones, optionally filtered by country code

### HTTP Request

`GET /timezones`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `country` | string | No |  |
| `limit` | string | No |  |
| `offset` | string | No |  |

## Get timezone by ID

```shell
curl "https://cities-api.p.rapidapi.com/timezones/5128581" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/timezones/5128581',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/timezones/5128581',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Get detailed information about a specific timezone, including current time. The ID can use hyphens instead of slashes (e.g., America-New_York)

### HTTP Request

`GET /timezones/{id}`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes |  |

## Get timezone at coordinates

```shell
curl "https://cities-api.p.rapidapi.com/timezone-at?latitude=40.7128&longitude=-74.006" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/timezone-at?latitude=40.7128&longitude=-74.006',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/timezone-at',
    params={
        'latitude': '40.7128',
        'longitude': '-74.006'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Find the timezone at a specific geographic location by finding the nearest city with timezone information

### HTTP Request

`GET /timezone-at`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `latitude` | string | Yes |  |
| `longitude` | string | Yes |  |

## Calculate time difference between places

```shell
curl "https://cities-api.p.rapidapi.com/time-difference?from=5128581&to=2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/time-difference?from=5128581&to=2643743',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/time-difference',
    params={
        'from': '5128581',
        'to': '2643743'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Calculate the time difference between two places, showing their current local times and UTC offsets

### HTTP Request

`GET /time-difference`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `from` | string | Yes |  |
| `to` | string | Yes |  |

# Postal Codes

## List all postal codes

```shell
curl "https://cities-api.p.rapidapi.com/postal-codes?country=US&q=New" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/postal-codes?country=US&q=New',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/postal-codes',
    params={
        'country': 'US',
        'q': 'New'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Get a paginated list of postal codes for a specific country, with optional search prefix

### HTTP Request

`GET /postal-codes`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `country` | string | No |  |
| `q` | string | No |  |
| `limit` | string | No |  |
| `offset` | string | No |  |

## Find nearby postal codes

```shell
curl "https://cities-api.p.rapidapi.com/postal-codes/nearby?latitude=40.7128&longitude=-74.006&radius=50&limit=25" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/postal-codes/nearby?latitude=40.7128&longitude=-74.006&radius=50&limit=25',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/postal-codes/nearby',
    params={
        'latitude': '40.7128',
        'longitude': '-74.006',
        'radius': '50',
        'limit': '25'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Find postal codes near a geographic location, sorted by distance

### HTTP Request

`GET /postal-codes/nearby`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `latitude` | string | Yes |  |
| `longitude` | string | Yes |  |
| `radius` | string | No |  |
| `limit` | string | No |  |

## Lookup postal code

```shell
curl "https://cities-api.p.rapidapi.com/postal-codes/US/US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/postal-codes/US/US',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/postal-codes/US/US',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Get detailed information about a specific postal code in a country. A single postal code may cover multiple places.

### HTTP Request

`GET /postal-codes/{country}/{code}`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `country` | string | Yes |  |
| `code` | string | Yes |  |

# Airports

## List all airports

```shell
curl "https://cities-api.p.rapidapi.com/airports?country=US&q=New" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/airports?country=US&q=New',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/airports',
    params={
        'country': 'US',
        'q': 'New'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Get a paginated list of airports worldwide, with optional filters by country and search query

### HTTP Request

`GET /airports`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `country` | string | No |  |
| `q` | string | No |  |
| `limit` | string | No |  |
| `offset` | string | No |  |

# Seaports

## List all seaports

```shell
curl "https://cities-api.p.rapidapi.com/seaports?country=US&q=New" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/seaports?country=US&q=New',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/seaports',
    params={
        'country': 'US',
        'q': 'New'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Get a paginated list of seaports, harbors, and marinas worldwide, with optional filters by country and search query

### HTTP Request

`GET /seaports`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `country` | string | No |  |
| `q` | string | No |  |
| `limit` | string | No |  |
| `offset` | string | No |  |

# Capitals

## List capital cities

```shell
curl "https://cities-api.p.rapidapi.com/capitals?continent=example&limit=25" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/capitals?continent=example&limit=25',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/capitals',
    params={
        'continent': 'example',
        'limit': '25'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Get a paginated list of capital cities worldwide, ordered by population. Optional continent filter.

### HTTP Request

`GET /capitals`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `continent` | string | No |  |
| `limit` | string | No |  |
| `offset` | string | No |  |

# Unit Conversion

All API responses use metric units (km, m, km²). Use these endpoints to convert.

## Convert distance units

```shell
curl "https://cities-api.p.rapidapi.com/convert/distance?value=100&from=5128581&to=2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/convert/distance?value=100&from=5128581&to=2643743',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/convert/distance',
    params={
        'value': '100',
        'from': '5128581',
        'to': '2643743'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Convert between distance units: km, mi, nm, m, ft

### HTTP Request

`GET /convert/distance`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `value` | string | Yes |  |
| `from` | string | Yes |  |
| `to` | string | Yes |  |

## Convert elevation units

```shell
curl "https://cities-api.p.rapidapi.com/convert/elevation?value=100&from=5128581&to=2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/convert/elevation?value=100&from=5128581&to=2643743',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/convert/elevation',
    params={
        'value': '100',
        'from': '5128581',
        'to': '2643743'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Convert between elevation units: m, ft

### HTTP Request

`GET /convert/elevation`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `value` | string | Yes |  |
| `from` | string | Yes |  |
| `to` | string | Yes |  |

## Convert area units

```shell
curl "https://cities-api.p.rapidapi.com/convert/area?value=100&from=5128581&to=2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/convert/area?value=100&from=5128581&to=2643743',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/convert/area',
    params={
        'value': '100',
        'from': '5128581',
        'to': '2643743'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Convert between area units: sqkm, sqmi, ha, acre

### HTTP Request

`GET /convert/area`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `value` | string | Yes |  |
| `from` | string | Yes |  |
| `to` | string | Yes |  |

## Convert temperature units

```shell
curl "https://cities-api.p.rapidapi.com/convert/temperature?value=100&from=5128581&to=2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/convert/temperature?value=100&from=5128581&to=2643743',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/convert/temperature',
    params={
        'value': '100',
        'from': '5128581',
        'to': '2643743'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Convert between temperature units: c (Celsius), f (Fahrenheit), k (Kelvin)

### HTTP Request

`GET /convert/temperature`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `value` | string | Yes |  |
| `from` | string | Yes |  |
| `to` | string | Yes |  |

## Convert speed units

```shell
curl "https://cities-api.p.rapidapi.com/convert/speed?value=100&from=5128581&to=2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/convert/speed?value=100&from=5128581&to=2643743',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/convert/speed',
    params={
        'value': '100',
        'from': '5128581',
        'to': '2643743'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Convert between speed units: kmh, mph, knots, ms

### HTTP Request

`GET /convert/speed`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `value` | string | Yes |  |
| `from` | string | Yes |  |
| `to` | string | Yes |  |

## Convert coordinate formats

```shell
curl "https://cities-api.p.rapidapi.com/convert/coordinates?value=100&from=5128581&to=2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/convert/coordinates?value=100&from=5128581&to=2643743',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/convert/coordinates',
    params={
        'value': '100',
        'from': '5128581',
        'to': '2643743'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Convert between decimal degrees (dd) and degrees-minutes-seconds (dms) formats

### HTTP Request

`GET /convert/coordinates`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `value` | string | Yes |  |
| `from` | string | Yes |  |
| `to` | string | Yes |  |

# Reference

## List all feature codes

```shell
curl "https://cities-api.p.rapidapi.com/feature-codes?class=P" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/feature-codes?class=P',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/feature-codes',
    params={
        'class': 'P'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /feature-codes`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `class` | string | No |  |

## List all feature classes

```shell
curl "https://cities-api.p.rapidapi.com/feature-classes" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/feature-classes',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/feature-classes',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /feature-classes`

## List all phone codes

```shell
curl "https://cities-api.p.rapidapi.com/phone-codes?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/phone-codes?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/phone-codes',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /phone-codes`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |

## List all currencies

```shell
curl "https://cities-api.p.rapidapi.com/currencies?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/currencies?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/currencies',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /currencies`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |

## List all languages

```shell
curl "https://cities-api.p.rapidapi.com/languages?limit=25&offset=0" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/languages?limit=25&offset=0',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/languages',
    params={
        'limit': '25',
        'offset': '0'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /languages`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | string | No |  |
| `offset` | string | No |  |

## List all continents

```shell
curl "https://cities-api.p.rapidapi.com/continents" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/continents',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/continents',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /continents`

## Get continent by code

```shell
curl "https://cities-api.p.rapidapi.com/continents/US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/continents/US',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/continents/US',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /continents/{code}`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | Yes |  |

## Get language by code

```shell
curl "https://cities-api.p.rapidapi.com/languages/US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/languages/US',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/languages/US',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

### HTTP Request

`GET /languages/{code}`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | Yes |  |

# Validation

## Validate coordinates

```shell
curl "https://cities-api.p.rapidapi.com/validate/coordinates?latitude=40.7128&longitude=-74.006" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/validate/coordinates?latitude=40.7128&longitude=-74.006',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/validate/coordinates',
    params={
        'latitude': '40.7128',
        'longitude': '-74.006'
    },
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Validate geographic coordinates and check if they are on land

### HTTP Request

`GET /validate/coordinates`

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `latitude` | string | Yes |  |
| `longitude` | string | Yes |  |

## Validate postal code format

```shell
curl "https://cities-api.p.rapidapi.com/validate/postal-code/US/US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/validate/postal-code/US/US',
  {
    headers: {
      'X-RapidAPI-Key': 'YOUR_API_KEY',
      'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
  }
);
const data = await response.json();
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/validate/postal-code/US/US',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
data = response.json()
```

Check if a postal code exists for a given country

### HTTP Request

`GET /validate/postal-code/{country}/{code}`

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `country` | string | Yes |  |
| `code` | string | Yes |  |

