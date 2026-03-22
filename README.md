# 🇮🇹 Italy National Disaster & Sensor Monitoring Dashboard

Real-time monitoring dashboard integrating **34 data sources** across all Italian regions — earthquakes, floods, rainfall, landslides, volcanic activity, tide gauges, and environmental sensors.

**[Live Dashboard →](https://radmehr33.github.io/italy-disaster-monitor/)**

---

## Data Sources

### 🏛️ National (7 sources)

| Source | Type | Data | Update |
|--------|------|------|--------|
| **INGV Earthquakes** | Seismic | M2+ events across Italy | Real-time (FDSN) |
| **INGV Volcanic (Etna/Stromboli)** | Volcanic seismicity | M0.5+ near Etna/Stromboli | Real-time |
| **INGV Volcanic (Vesuvio)** | Volcanic seismicity | M0+ near Vesuvio/Campi Flegrei | Real-time |
| **ISPRA Landslides (IFFI)** | Landslide inventory | 621,000+ mapped events | WFS |
| **ISPRA Flood Areas** | Flood mapping | Historical flood zones | WFS |
| **ISPRA Tide Gauges** | Coastal monitoring | 36 stations around Italian coast | Static |

### 🔵 Nord-Ovest (5 sources)

| Source | Type | Stations | Update |
|--------|------|----------|--------|
| **ARPA Piemonte Hydro** | River levels + alert status | 120+ | 15 min (ArcGIS) |
| **ARPA Piemonte Rain** | Rainfall 3h/24h live readings | 400+ | 15 min (ArcGIS) |
| **ARPA Piemonte Temp** | Air temperature | 200+ | GeoJSON |
| **ARPA Piemonte Snow** | Snow depth | 100+ | GeoJSON |
| **ARPA Piemonte Wind** | Wind speed | 100+ | GeoJSON |

### 🔵 Nord-Est (5 sources)

| Source | Type | Stations | Update |
|--------|------|----------|--------|
| **ARPA Lombardia** | Multi-sensor (hydro, rain, temp, wind) | 500+ | Real-time (Socrata) |
| **MeteoTrentino** | Weather stations + live per-station data | 355+ | 15 min (XML SOAP) |
| **Alto Adige/Südtirol** | Temperature, precipitation | 200+ | Real-time (Open Data Hub) |
| **ARPAE Emilia-Romagna** | Meteo observations | 300+ | Real-time (JSONL) |
| **OGS Seismic NE Italy** | NE Italy seismicity | M1+ events | Real-time (FDSN) |

### 🟡🟠🏝️ Centro, Sud & Isole (17 sources)

Configured and listed in the dashboard sidebar. These sources are pending API verification and will activate as their public endpoints become accessible:

ARPA Toscana, Umbria, Marche, Lazio, Abruzzo, Molise, Campania, Puglia, Basilicata, Calabria, Sicilia, Sardegna, ARPAL Liguria, ARPA VDA, ARPAV Veneto, ARPA FVG, ISPRA Venice Tides.

---

## Features

### Interactive Map
- Dark-themed Leaflet map centered on Italy
- Clustered markers with source-specific colors
- Click any station to see live readings and metadata
- Region filtering (All Italy, National, Nord-Ovest, Nord-Est, Centro, Sud, Isole)
- Toggle individual data sources on/off

### Alert Engine
Real-time threshold monitoring across all sensor types:

| Sensor | Threshold | Severity |
|--------|-----------|----------|
| Earthquake magnitude | M ≥ 4.0 | 🔴 CRITICAL |
| Earthquake magnitude | M ≥ 3.0 | 🟡 WARNING |
| Earthquake depth | < 5 km | 🟡 WARNING |
| River level | > 3.0 m | 🔴 CRITICAL |
| River level | > 2.0 m | 🟡 WARNING |
| Rainfall 3h | > 40 mm | 🔴 CRITICAL |
| Rainfall 24h | > 100 mm | 🔴 CRITICAL |
| Rainfall 24h | > 50 mm | 🟡 WARNING |
| Wind speed | > 90 km/h | 🔴 CRITICAL |
| Wind gusts | > 100 km/h | 🔴 CRITICAL |
| Temperature | > 40°C or < -25°C | 🔴 CRITICAL |
| Temperature | > 35°C or < -15°C | 🟡 WARNING |
| Snow depth | > 300 cm | 🔴 CRITICAL |

Each alert shows:
- **Exact measured value** vs threshold exceeded
- **Station name** and **location** (municipality, province)
- **River name** for hydrometric stations
- **Earthquake place description** from INGV

### Alert Notifications
- Pulsing red/yellow rings on map for critical/warning stations
- Alert banner at top of map
- Sound notifications (toggleable) for critical events
- Alerts tab with severity-sorted feed — click to fly to location

### Historical Data Charts
- **Piemonte stations** — 3-day time series (level, rain, temp, humidity, wind, snow)
- **INGV earthquakes** — 30-day magnitude bar chart for nearby events
- **MeteoTrentino** — Live 24h charts (temperature, rain, wind, humidity)
- **ARPA Lombardia** — 3-day sensor readings time series

### Auto-Refresh
All data sources refresh every 10 minutes automatically.

---

## APIs Used

| Provider | Endpoint | Format |
|----------|----------|--------|
| INGV | `webservices.ingv.it/fdsnws/event/1/query` | GeoJSON |
| ISPRA IdroGEO | `idrogeo.isprambiente.it/api/geoserver/wfs` | GeoJSON (WFS) |
| ARPA Piemonte | `webgis.arpa.piemonte.it/server/rest/services/` | GeoJSON (ArcGIS) |
| ARPA Piemonte | `utility.arpa.piemonte.it/api_realtime/` | JSON (REST) |
| ARPA Piemonte | `arpa.piemonte.it/rischi_naturali/data/tr/` | GeoJSON |
| ARPA Lombardia | `dati.lombardia.it/resource/nf78-nj6b.json` | JSON (Socrata) |
| MeteoTrentino | `dati.meteotrentino.it/service.asmx/` | XML (SOAP) |
| Open Data Hub BZ | `mobility.api.opendatahub.com/v2/flat/MeteoStation/` | JSON (REST) |
| ARPAE | `dati-simc.arpae.it/opendata/osservati/meteo/realtime/` | JSONL |

All endpoints are **public and free** — no authentication required.

---

## Tech Stack

- **Leaflet** + **Leaflet.markercluster** — Interactive map
- **Chart.js** — Historical data visualization
- **Vanilla JS** — No build tools, no framework, single HTML file
- **CARTO Dark** — Map tiles

---

## Quick Start

```bash
# Clone and open
git clone https://github.com/radmehr33/italy-disaster-monitor.git
open italy-disaster-monitor/index.html
```

Or visit the **[live version on GitHub Pages](https://radmehr33.github.io/italy-disaster-monitor/)**.

---

## Architecture

```
index.html (single file, ~67KB)
├── CSS — Dark theme, responsive layout
├── HTML — Header, sidebar (Sources/Alerts/Detail tabs), map, bottom bar
└── JavaScript
    ├── 34 source definitions with URLs and parser types
    ├── 10 specialized parsers (INGV, Piemonte, ISPRA, Lombardia, Trentino, Bolzano, ARPAE, etc.)
    ├── Alert engine — threshold evaluation across all stations
    ├── Historical data loaders — per-source API calls with chart rendering
    └── Auto-refresh loop (10 min interval)
```

---

## License

Data is provided by Italian public agencies under their respective open data licenses (CC-BY, CC0). This dashboard is an aggregation tool — always refer to official sources for critical decision-making.
