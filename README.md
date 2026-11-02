# TankBilligt ⛽🇩🇰

**Find Denmark's cheapest fuel, in real time.** A cross-platform mobile product — native iOS, native Android, and a production Node.js backend — with **2,000+ active users**.

![status](https://img.shields.io/badge/status-live-success)
![platform](https://img.shields.io/badge/platform-iOS%20%7C%20Android-blue)
![backend](https://img.shields.io/badge/backend-Node.js%20%C2%B7%20TypeScript%20%C2%B7%20PostgreSQL-informational)

> Designed, built, shipped, and operated solo — from data ingestion and the API to two native apps and the App Store / Google Play listings.

## What it does
- **Real-time fuel prices** aggregated from 9+ Danish & European chains (OK, Q8, F24, Goon, Uno-X, OIL!, Shell, Circle K, Lavpris…)
- **Cheapest-station finder** on an interactive map
- **Trip planner** that routes you past the best-value stations along the way
- Freemium: free core app + Premium subscription. No ads.

## Architecture
```mermaid
flowchart LR
  subgraph Clients
    iOS["iOS · SwiftUI + MapKit"]
    AND["Android · Jetpack Compose"]
  end
  iOS -->|REST / JWT| API
  AND -->|REST / JWT| API
  subgraph Backend["Backend · Railway"]
    API["Express + TypeScript API"]
    CRON["node-cron price scrapers"]
    API --> DB[("PostgreSQL · Prisma")]
    CRON --> DB
    CRON -->|fetch & normalize| SRC["Chain price feeds + Lavpris API"]
  end
  API -->|server-side receipt validation| APPLE["App Store Server API"]
  API -->|prod/dev alerts| DISC["Discord webhooks"]
```

## Engineering highlights
- **Server-side IAP validation** with Apple's App Store Server Library (`jose`-signed JWTs) — premium entitlements are verified on the backend, never trusted from the client.
- **Scheduled ingestion** via `node-cron` jobs that scrape and normalize prices from heterogeneous chain feeds (XML / JSON / XLSX / PDF parsers) into one unified Prisma schema.
- **Hardened API**: `helmet`, CORS, `express-rate-limit`, `zod` request validation, JWT auth, and structured `winston` logging.
- **Operational visibility**: Discord webhook alerting for prod and dev, Railway deploys, Docker Compose for local parity.
- **Two native clients** sharing one API: SwiftUI + MapKit (iOS 16+) and Jetpack Compose (Android), StoreKit 2, privacy-first TelemetryDeck analytics. No ads.

## Tech stack
| Layer | Technologies |
|---|---|
| iOS | SwiftUI, MapKit, StoreKit 2 |
| Android | Kotlin, Jetpack Compose |
| Backend | Node.js, Express, TypeScript, Prisma, PostgreSQL |
| Infra | Railway, Docker Compose, node-cron |
| Integrations | Apple App Store Server API, TelemetryDeck |

## Screenshots
<p>
  <img src="docs/screenshots/1-map.jpg" width="200" alt="Map of nearby stations with live prices">
  <img src="docs/screenshots/2-stations.jpg" width="200" alt="Station list sorted by price">
  <img src="docs/screenshots/3-price-history.jpg" width="200" alt="Price history and price alerts">
  <img src="docs/screenshots/4-carplay.jpg" width="200" alt="CarPlay map">
</p>

_App Store screenshots (Danish UI)._

---
© 2026 Alen Trgovcevic · AT Productions (CVR 46159640). Source code is private; this repository is a public architecture overview.
