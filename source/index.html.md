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
    content: Cities API - The most comprehensive geographic data API with 85+ endpoints
---

# Introduction

```
╔═══════════════════════════════════════════╗
║                                           ║
║         Welcome to Cities API             ║
║                                           ║
║     5+ million cities worldwide           ║
║     85+ endpoints                         ║
║     Comprehensive geographic data         ║
║                                           ║
╚═══════════════════════════════════════════╝
```

Welcome to the **Cities API** — the most comprehensive geographic data API available.

With **85+ endpoints**, we provide everything you need for location-based applications:

| Feature | Description |
|---------|-------------|
| **City Data** | Search, filter, and explore 5+ million cities worldwide |
| **Country Intelligence** | Detailed country info including neighbors, stats, and memberships |
| **Time & Astronomy** | Timezones, local times, sun/moon rise and set times |
| **Travel Data** | Airports, seaports, climate zones, electrical standards |
| **Localization** | Currencies, languages, date formats, phone codes |
| **Geopolitical** | G7, G20, BRICS, NATO, OPEC, UN membership data |

**Why choose Cities API?**

- 3x more endpoints than competitors
- Unique features not found elsewhere (sun times, climate zones, electrical standards)
- All endpoints available on every plan — no feature restrictions
- Simple pricing based on usage, not artificial limits

# Authentication

```shell
curl "https://cities-api.p.rapidapi.com/cities" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch('https://cities-api.p.rapidapi.com/cities', {
  headers: {
    'X-RapidAPI-Key': 'YOUR_API_KEY',
    'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
  }
});
```

```python
import requests

response = requests.get(
    'https://cities-api.p.rapidapi.com/cities',
    headers={
        'X-RapidAPI-Key': 'YOUR_API_KEY',
        'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'
    }
)
```

All API requests require authentication via RapidAPI. Include these headers with every request:

| Header | Description |
|--------|-------------|
| `X-RapidAPI-Key` | Your RapidAPI subscription key |
| `X-RapidAPI-Host` | `cities-api.p.rapidapi.com` |

# Pagination

```json
{
  "data": [...],
  "hasMore": true,
  "nextPage": "/cities?country=US&offset=25&limit=25"
}
```

Most endpoints support pagination:

| Parameter | Default | Max | Description |
|-----------|---------|-----|-------------|
| `limit` | 25 | 100 | Results per page |
| `offset` | 0 | — | Results to skip |

# Cities

## Search Cities

```shell
curl "https://cities-api.p.rapidapi.com/cities?country=US&q=New" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

```javascript
const response = await fetch(
  'https://cities-api.p.rapidapi.com/cities?country=US&q=New',
  { headers: { 'X-RapidAPI-Key': 'YOUR_API_KEY', 'X-RapidAPI-Host': 'cities-api.p.rapidapi.com' }}
);
```

```python
response = requests.get(
    'https://cities-api.p.rapidapi.com/cities',
    params={'country': 'US', 'q': 'New'},
    headers={'X-RapidAPI-Key': 'YOUR_API_KEY', 'X-RapidAPI-Host': 'cities-api.p.rapidapi.com'}
)
```

> Response:

```json
{
  "data": [
    {
      "id": 5128581,
      "name": "New York City",
      "latitude": 40.71427,
      "longitude": -74.00597,
      "countryCode": "US",
      "population": 8804190,
      "timezone": "America/New_York"
    }
  ],
  "hasMore": true
}
```

Search and filter cities worldwide.

### HTTP Request

`GET /cities`

### Query Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `country` | Yes* | ISO country code (required with `q`) |
| `q` | No | Search query |
| `minPopulation` | No | Minimum population |
| `timezone` | No | Filter by timezone |

## Get City

```shell
curl "https://cities-api.p.rapidapi.com/cities/5128581" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "id": 5128581,
  "name": "New York City",
  "latitude": 40.71427,
  "longitude": -74.00597,
  "countryCode": "US",
  "population": 8804190,
  "elevation": 10,
  "timezone": "America/New_York"
}
```

Get detailed city information.

### HTTP Request

`GET /cities/:id`

## Nearby Cities

```shell
curl "https://cities-api.p.rapidapi.com/cities/5128581/nearby?radius=50" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "city": { "id": 5128581, "name": "New York City" },
  "nearby": [
    { "id": 5099836, "name": "Jersey City", "distance": 9.2 }
  ],
  "unit": "km"
}
```

Find cities within a radius.

### HTTP Request

`GET /cities/:id/nearby`

| Parameter | Default | Description |
|-----------|---------|-------------|
| `radius` | 50 | Radius in km |

## City Sun Times

```shell
curl "https://cities-api.p.rapidapi.com/cities/5128581/sun-times?date=2026-06-21" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "city": { "id": 5128581, "name": "New York City" },
  "date": "2026-06-21",
  "sun": {
    "sunrise": "05:25",
    "sunset": "20:31",
    "solarNoon": "12:58",
    "dayLength": "15:06"
  }
}
```

Calculate sunrise, sunset, and day length for any date.

### HTTP Request

`GET /cities/:id/sun-times`

| Parameter | Default | Description |
|-----------|---------|-------------|
| `date` | Today | YYYY-MM-DD format |

## City Climate Zone

```shell
curl "https://cities-api.p.rapidapi.com/cities/5128581/climate-zone" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "city": { "id": 5128581, "name": "New York City" },
  "climate": {
    "zone": "Cfa",
    "name": "Humid Subtropical",
    "description": "Hot, humid summers and mild winters"
  }
}
```

Get Köppen climate classification.

### HTTP Request

`GET /cities/:id/climate-zone`

## City Local Info

```shell
curl "https://cities-api.p.rapidapi.com/cities/5128581/local-info" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "city": "New York City",
  "country": "United States",
  "timezone": "America/New_York",
  "currentLocalTime": "2026-01-06T08:30:00-05:00",
  "currency": { "code": "USD", "symbol": "$" },
  "languages": ["en-US", "es-US"]
}
```

Get local info including time, currency, and languages.

### HTTP Request

`GET /cities/:id/local-info`

## City Antipode

```shell
curl "https://cities-api.p.rapidapi.com/cities/5128581/antipode" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "city": { "id": 5128581, "name": "New York City" },
  "antipode": {
    "latitude": -40.71427,
    "longitude": 105.99403,
    "onLand": false,
    "nearestCity": { "name": "Perth", "distance": 2847 }
  }
}
```

Find the opposite point on Earth.

### HTTP Request

`GET /cities/:id/antipode`

## City Hemisphere

```shell
curl "https://cities-api.p.rapidapi.com/cities/5128581/hemisphere" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "city": { "id": 5128581, "name": "New York City" },
  "hemisphere": { "northSouth": "Northern", "eastWest": "Western" }
}
```

Get hemisphere location.

### HTTP Request

`GET /cities/:id/hemisphere`

## City Hierarchy

```shell
curl "https://cities-api.p.rapidapi.com/cities/5128581/hierarchy" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "city": { "id": 5128581, "name": "New York City" },
  "hierarchy": {
    "country": { "code": "US", "name": "United States" },
    "region": { "code": "NY", "name": "New York" },
    "continent": { "code": "NA", "name": "North America" }
  }
}
```

Get administrative hierarchy.

### HTTP Request

`GET /cities/:id/hierarchy`

## Population Rank

```shell
curl "https://cities-api.p.rapidapi.com/cities/5128581/population-rank" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "city": { "id": 5128581, "name": "New York City", "population": 8804190 },
  "rank": { "country": 1, "continent": 4, "world": 24 }
}
```

Get population rankings.

### HTTP Request

`GET /cities/:id/population-rank`

## Nearby Airports

```shell
curl "https://cities-api.p.rapidapi.com/cities/5128581/airports" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "city": { "id": 5128581, "name": "New York City" },
  "airports": [
    { "name": "JFK International", "iataCode": "JFK", "distance": 19.2 },
    { "name": "LaGuardia", "iataCode": "LGA", "distance": 12.8 }
  ]
}
```

Find airports near a city.

### HTTP Request

`GET /cities/:id/airports`

## Distance Between Cities

```shell
curl "https://cities-api.p.rapidapi.com/cities/distance?from=5128581&to=2643743" \
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

Calculate distance between cities.

### HTTP Request

`GET /cities/distance`

## Bearing

```shell
curl "https://cities-api.p.rapidapi.com/cities/bearing?from=5128581&to=2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "from": { "name": "New York City" },
  "to": { "name": "London" },
  "bearing": 51.2,
  "compassDirection": "NE"
}
```

Get compass bearing between cities.

### HTTP Request

`GET /cities/bearing`

## Midpoint

```shell
curl "https://cities-api.p.rapidapi.com/cities/midpoint?city1=5128581&city2=2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Find geographic midpoint between cities.

### HTTP Request

`GET /cities/midpoint`

## Reverse Geocode

```shell
curl "https://cities-api.p.rapidapi.com/cities/reverse?latitude=40.7128&longitude=-74.006" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Find nearest city to coordinates.

### HTTP Request

`GET /cities/reverse`

## Random Cities

```shell
curl "https://cities-api.p.rapidapi.com/cities/random?count=5" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Get random cities. Great for games and quizzes.

### HTTP Request

`GET /cities/random`

## City Extremes

```shell
curl "https://cities-api.p.rapidapi.com/cities/extremes?country=US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "extremes": {
    "northernmost": { "name": "Barrow" },
    "southernmost": { "name": "Hilo" },
    "highest": { "name": "Leadville", "elevation": 3094 },
    "mostPopulous": { "name": "New York City" }
  }
}
```

Find extreme cities (northernmost, highest, etc).

### HTTP Request

`GET /cities/extremes`

## Bounding Box

```shell
curl "https://cities-api.p.rapidapi.com/cities/bbox?north=41&south=40&east=-73&west=-75" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Find cities within a geographic box.

### HTTP Request

`GET /cities/bbox`

## Batch Lookup

```shell
curl "https://cities-api.p.rapidapi.com/cities/batch?ids=5128581,2643743,2988507" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Get multiple cities in one request.

### HTTP Request

`GET /cities/batch`

## Distance Matrix

```shell
curl "https://cities-api.p.rapidapi.com/cities/distance-matrix?ids=5128581,2643743,2988507" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Calculate distances between all pairs.

### HTTP Request

`GET /cities/distance-matrix`

## Route

```shell
curl "https://cities-api.p.rapidapi.com/cities/route?ids=5128581,2643743,2988507" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Calculate route through multiple cities.

### HTTP Request

`GET /cities/route`

# Countries

## List Countries

```shell
curl "https://cities-api.p.rapidapi.com/countries" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Get all countries.

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
  "code": "US",
  "name": "United States",
  "capital": "Washington",
  "population": 327167434,
  "area": 9629091,
  "currencyCode": "USD",
  "phoneCode": "1",
  "languages": "en-US,es-US",
  "drivingSide": "right"
}
```

Get country details.

### HTTP Request

`GET /countries/:code`

## Country Neighbors

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
    { "code": "IT", "name": "Italy" }
  ]
}
```

Get bordering countries.

### HTTP Request

`GET /countries/:code/neighbors`

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
  "electrical": {
    "voltage": 120,
    "frequency": 60,
    "plugTypes": ["A", "B"]
  }
}
```

Essential for travel apps — get voltage and plug types.

### HTTP Request

`GET /countries/:code/electrical`

## Localization

```shell
curl "https://cities-api.p.rapidapi.com/countries/localization/DE" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "country": { "code": "DE", "name": "Germany" },
  "localization": {
    "languages": ["de"],
    "currency": { "code": "EUR", "symbol": "€" },
    "numberFormat": { "decimalSeparator": ",", "thousandsSeparator": "." }
  }
}
```

Get localization settings.

### HTTP Request

`GET /countries/localization/:code`

## Format

```shell
curl "https://cities-api.p.rapidapi.com/countries/format/JP" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "country": { "code": "JP", "name": "Japan" },
  "format": {
    "drivingSide": "left",
    "dateFormat": "YYYY/MM/DD",
    "measurementSystem": "metric"
  }
}
```

Get formatting conventions.

### HTTP Request

`GET /countries/format/:code`

## Calling Codes

```shell
curl "https://cities-api.p.rapidapi.com/countries/calling/US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "country": { "code": "US" },
  "calling": {
    "countryCode": "+1",
    "format": "(XXX) XXX-XXXX"
  }
}
```

Get phone calling information.

### HTTP Request

`GET /countries/calling/:code`

## Major Cities

```shell
curl "https://cities-api.p.rapidapi.com/countries/major-cities/JP" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Get largest cities in a country.

### HTTP Request

`GET /countries/major-cities/:code`

## Landlocked Countries

```shell
curl "https://cities-api.p.rapidapi.com/countries/landlocked" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Get countries without coastline.

### HTTP Request

`GET /countries/landlocked`

## Island Countries

```shell
curl "https://cities-api.p.rapidapi.com/countries/islands" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Get island nations.

### HTTP Request

`GET /countries/islands`

## Largest Countries

```shell
curl "https://cities-api.p.rapidapi.com/countries/largest?by=area" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Get largest by area or population.

### HTTP Request

`GET /countries/largest`

## Smallest Countries

```shell
curl "https://cities-api.p.rapidapi.com/countries/smallest?by=area" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Get smallest by area or population.

### HTTP Request

`GET /countries/smallest`

# International Organizations

Access membership data for major international organizations.

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
  "fullName": "Group of Seven",
  "memberCount": 7,
  "data": [
    { "code": "US", "name": "United States" },
    { "code": "JP", "name": "Japan" }
  ]
}
```

### HTTP Request

`GET /countries/g7`

## G20 Members

```shell
curl "https://cities-api.p.rapidapi.com/countries/g20" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

World's largest economies.

### HTTP Request

`GET /countries/g20`

## BRICS Members

```shell
curl "https://cities-api.p.rapidapi.com/countries/brics" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Including 2024 expansion members.

### HTTP Request

`GET /countries/brics`

## OPEC Members

```shell
curl "https://cities-api.p.rapidapi.com/countries/opec" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Oil-exporting countries.

### HTTP Request

`GET /countries/opec`

## NATO Members

```shell
curl "https://cities-api.p.rapidapi.com/countries/nato-members" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

### HTTP Request

`GET /countries/nato-members`

## EU Members

```shell
curl "https://cities-api.p.rapidapi.com/countries/eu-members" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

European Union member states.

### HTTP Request

`GET /countries/eu-members`

## Schengen Area

```shell
curl "https://cities-api.p.rapidapi.com/countries/schengen" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Passport-free travel zone.

### HTTP Request

`GET /countries/schengen`

## Commonwealth

```shell
curl "https://cities-api.p.rapidapi.com/countries/commonwealth" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Commonwealth of Nations members.

### HTTP Request

`GET /countries/commonwealth`

## UN Members

```shell
curl "https://cities-api.p.rapidapi.com/countries/un-members" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

All 193 United Nations member states.

### HTTP Request

`GET /countries/un-members`

# Continents

## List Continents

```shell
curl "https://cities-api.p.rapidapi.com/continents" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "data": [
    { "code": "AF", "name": "Africa" },
    { "code": "AS", "name": "Asia" },
    { "code": "EU", "name": "Europe" }
  ]
}
```

### HTTP Request

`GET /continents`

## Get Continent

```shell
curl "https://cities-api.p.rapidapi.com/continents/EU" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

### HTTP Request

`GET /continents/:code`

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
curl "https://cities-api.p.rapidapi.com/timezones/America%2FNew_York" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "id": "America/New_York",
  "gmtOffset": -5,
  "dstOffset": -4,
  "currentTime": "2026-01-06T08:30:00-05:00"
}
```

### HTTP Request

`GET /timezones/:id`

## Timezone at Location

```shell
curl "https://cities-api.p.rapidapi.com/timezone-at?latitude=40.7128&longitude=-74.006" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Get timezone for any coordinates.

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
  "from": { "name": "New York City", "localTime": "08:30" },
  "to": { "name": "London", "localTime": "13:30" },
  "difference": { "hours": 5, "description": "London is 5 hours ahead" }
}
```

### HTTP Request

`GET /time-difference`

# Airports

```shell
curl "https://cities-api.p.rapidapi.com/airports?country=US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

### HTTP Request

`GET /airports`

# Seaports

```shell
curl "https://cities-api.p.rapidapi.com/seaports?country=NL" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

### HTTP Request

`GET /seaports`

# Capitals

```shell
curl "https://cities-api.p.rapidapi.com/capitals" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

World capital cities.

### HTTP Request

`GET /capitals`

# Currencies

## List Currencies

```shell
curl "https://cities-api.p.rapidapi.com/currencies" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

### HTTP Request

`GET /currencies`

# Languages

## List Languages

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
  "countries": [
    { "code": "ES", "name": "Spain" },
    { "code": "MX", "name": "Mexico" }
  ]
}
```

### HTTP Request

`GET /languages/:code`

# Postal Codes

## List Postal Codes

```shell
curl "https://cities-api.p.rapidapi.com/postal-codes?country=US" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

### HTTP Request

`GET /postal-codes`

## Lookup Postal Code

```shell
curl "https://cities-api.p.rapidapi.com/postal-codes/US/10001" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "postalCode": "10001",
  "placeName": "New York",
  "region": "New York",
  "latitude": 40.7484,
  "longitude": -73.9967
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

# Validation

## Validate Postal Code

```shell
curl "https://cities-api.p.rapidapi.com/validate/postal-code/US/10001" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

> Response:

```json
{
  "valid": true,
  "country": "US",
  "postalCode": "10001"
}
```

### HTTP Request

`GET /validate/postal-code/:country/:code`

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
  "onLand": true,
  "country": { "code": "US", "name": "United States" },
  "nearestCity": { "name": "New York City", "distance": 0.2 }
}
```

### HTTP Request

`GET /validate/coordinates`

# Comparison

## Compare Cities

```shell
curl "https://cities-api.p.rapidapi.com/compare/cities?ids=5128581,2643743" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Compare multiple cities side by side.

### HTTP Request

`GET /compare/cities`

## Compare Countries

```shell
curl "https://cities-api.p.rapidapi.com/compare/countries?codes=US,JP,DE" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

Compare multiple countries.

### HTTP Request

`GET /compare/countries`

# Reference

## Feature Codes

```shell
curl "https://cities-api.p.rapidapi.com/feature-codes" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: cities-api.p.rapidapi.com"
```

GeoNames feature codes for filtering.

### HTTP Request

`GET /feature-codes`

# Errors

| Code | Meaning |
|------|---------|
| 200 | Success |
| 400 | Bad Request |
| 401 | Unauthorized |
| 404 | Not Found |
| 429 | Rate Limited |
| 500 | Server Error |

> Error format:

```json
{
  "error": { "message": "City not found", "status": 404 }
}
```

# Rate Limits

| Tier | Requests/Day | Rate |
|------|--------------|------|
| Basic | 1,500 | 1,000/hr |
| Pro | 35,000 | 10/sec |
| Ultra | 60,000 | 20/sec |
| Mega | 100,000 | 30/sec |
