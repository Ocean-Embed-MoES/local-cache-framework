# OceanEmbed — Local Data Cache Framework

**SIH 2026 PS#01** | North Indian Ocean Subsurface Temperature Reconstruction

10 ocean datasets, ~242 GB pre-cached on local NVMe SSD. Zero-friction data access for AI agents.

## Quick Start

```bash
jupyter lab framework.ipynb
```

Run BLOCK 0 → BLOCK 4 once. Write experiment in BLOCK 5. See `AGENT_README.md` for full agent guide.

## Datasets

| # | Function | Source | Resolution | Cadence | Extent | Size |
|---|---|---|---|---|---|---|
| 1 | `get_sst(...)` | Copernicus OSTIA | 0.05° | daily | NIO | 10.5 GB |
| 2 | `get_sss(...)` | Copernicus SMAP/SMOS | 0.125° | daily | NIO | 0.8 GB |
| 3 | `get_ssh(...)` | Copernicus DUACS | 0.25° | daily | NIO | 0.2 GB |
| 4 | `get_currents(...)` | NASA OSCAR V2.0 | 0.25° | daily | Global | 36 GB |
| 5 | `get_winds(...)` | NASA CCMP V3.1 | 0.25° | **6-hourly** | Global | 73 GB |
| 6 | `get_glorys(...)` | Copernicus GLORYS12 | 0.083° | daily | NIO | 68 GB |
| 7 | `get_argo(...)` | Copernicus ARMOR3D | 0.125° | daily | NIO | 24 GB |
| 8 | `get_incois_vam(...)` | INCOIS LAS | 1° | 10-day | Indian Ocean | 1.7 GB |
| 9 | `get_incois_mccreary(...)` | INCOIS LAS | 1° | 10-day | Indian Ocean | 5.7 GB |
| 10 | `get_ascatc(...)` | MetOp-C ASCAT | 12.5 km swath | per-pass | Swath | 21 GB |

All data served **as-is** — no interpolation, no regridding, no resampling.
