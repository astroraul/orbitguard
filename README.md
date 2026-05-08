<div align="center">

# OrbitGuard AI

**Real-time micrometeoroid & orbital debris (MMOD) risk assessment for space operators.**

[![Live Demo](https://img.shields.io/badge/demo-online-00ccff?style=for-the-badge)](https://astroraul.github.io/orbitguard/)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue?style=for-the-badge)](LICENSE)
[![Built with Three.js](https://img.shields.io/badge/three.js-r128-black?style=for-the-badge&logo=three.js)](https://threejs.org/)
[![Data: CelesTrak](https://img.shields.io/badge/data-CelesTrak-orange?style=for-the-badge)](https://celestrak.org/)

[**Live demo →**](https://astroraul.github.io/orbitguard/)

</div>

---

## Overview

OrbitGuard AI is a browser-based situational awareness console for low-Earth-orbit (LEO) operators and analysts. It ingests live two-line-element (TLE) catalogs from CelesTrak, projects tracked objects onto an interactive 3D globe, and surfaces collision flux, recent breakup events, and per-asset threat scores in one dashboard.

The entire app runs client-side in a single static HTML file — no backend, no build step. Drop it on any web host (or open it locally) and it works.

## Features

- **3D LEO globe** — WebGL render of Earth with real-time satellite/debris positions propagated from CelesTrak TLEs.
- **MMOD flux calculator** — estimates impact flux for a user-defined orbit (altitude, inclination, area, mission duration) using a simplified ORDEM/MASTER-style model.
- **Breakup event tracker** — chronological feed of recent on-orbit fragmentation events with severity bands.
- **Asset inspector** — click any tracked object on the globe to see orbital elements, classification, and AI-derived threat score.
- **AI threat scoring** — heuristic scoring (`v3.1`) combining conjunction proximity, debris density at altitude, and recent breakup activity to produce a 0–100 risk index per asset.
- **Mission report export** — generates a printable risk briefing for the inspected asset or queried orbit.

## Quick start

### Run locally

```bash
git clone https://github.com/astroraul/orbitguard.git
cd orbitguard
# any static file server works, e.g.:
python3 -m http.server 8000
# then open http://localhost:8000
```

Or just double-click `index.html` — it's fully self-contained (Three.js and fonts are loaded from CDN).

### Deploy to GitHub Pages

The `main` branch root is already wired to GitHub Pages. Any push to `main` republishes the site. To enable on a fork:

1. **Settings → Pages**
2. **Source:** Deploy from a branch
3. **Branch:** `main` / `/ (root)`

## Tech stack

| Layer | Choice |
| --- | --- |
| Rendering | [Three.js](https://threejs.org/) r128 (WebGL) |
| Data | [CelesTrak](https://celestrak.org/) live TLE feeds |
| UI | Vanilla HTML / CSS / ES6 — no framework, no bundler |
| Typography | Orbitron + Share Tech Mono (Google Fonts) |
| Hosting | GitHub Pages (static) |

## Architecture

```
┌─────────────────────────────────────────────────┐
│                   index.html                    │
│  ┌──────────────┐  ┌──────────────┐             │
│  │ TLE fetcher  │→ │ SGP4 propag. │→  3D globe  │
│  └──────────────┘  └──────────────┘             │
│         │                  │                    │
│         ▼                  ▼                    │
│  ┌──────────────┐  ┌──────────────┐             │
│  │ Flux model   │  │ Threat score │→  Inspector │
│  └──────────────┘  └──────────────┘             │
└─────────────────────────────────────────────────┘
```

Single-file architecture by design — every state transition (data fetch, propagation tick, UI update) is observable in one source file, which is what space-ops users have asked for over a multi-bundle SPA.

## Data sources & accuracy

- **Catalog:** CelesTrak public TLE feeds. Update cadence depends on CelesTrak refresh (typically hours, not minutes).
- **Propagation:** SGP4 in JS. Accurate to ~1–3 km for short prediction windows; **not** suitable for operational conjunction screening — use 18 SDS or commercial CSpOC products for that.
- **Flux model:** simplified parametric fit. Use as a relative comparator across orbits, not as an absolute prediction. For mission-critical analysis use NASA ORDEM 3.x or ESA MASTER.

> **OrbitGuard is an awareness/education tool. Do not use its output for collision avoidance maneuver decisions.**

## Roadmap

- [ ] Conjunction screening against a user-uploaded ephemeris
- [ ] Configurable propagation horizon (current: fixed)
- [ ] Per-shell debris density heatmap layer
- [ ] Export TLE subsets matching a filter
- [ ] WebGPU renderer for >30k tracked objects

## Contributing

Issues and PRs welcome. See [`CONTRIBUTING.md`](CONTRIBUTING.md) for the workflow and [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) for community expectations. Security disclosures: [`SECURITY.md`](SECURITY.md).

## License

[MIT](LICENSE) © Mayan Space OÜ, Tallinn, Estonia.

CelesTrak data is provided by Dr. T.S. Kelso under [CelesTrak's terms](https://celestrak.org/about.php).
