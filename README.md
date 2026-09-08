# OceanEmbed — Local Data Cache Framework

**SIH 2026 PS#01** | North Indian Ocean Subsurface Temperature Reconstruction

A single Jupyter notebook that gives any AI agent zero-friction access to all 7 ocean datasets needed for the OceanEmbed problem statement. No credential management, no download logic, no cache management — just call the function and get data.

---

## Quick Start

```bash
jupyter lab framework.ipynb
```

Run **BLOCK 0 → BLOCK 4** once per session. Write your experiment in **BLOCK 5**.

---

## Data Functions

All functions share the same signature pattern:

```python
get_*(start, end, lat_min, lat_max, lon_min, lon_max, force=False)
```

| Function | Returns | Source | Resolution | Cadence |
|---|---|---|---|---|
| `get_sst(...)` | `DataArray` (time, lat, lon) °C | Copernicus OSTIA | 0.05° | daily |
| `get_sss(...)` | `DataArray` (time, lat, lon) PSU | Copernicus SMAP/SMOS | 0.125° | daily |
| `get_ssh(...)` | `DataArray` (time, lat, lon) m | Copernicus DUACS | 0.25° | daily |
| `get_currents(...)` | `Dataset` u, v m/s | NASA OSCAR V2.0 | 0.25° | **5-day** ⚠ |
| `get_winds(...)` | `Dataset` uwnd, vwnd m/s | NASA CCMP V3.1 | 0.25° | daily mean |
| `get_glorys(...)` | `Dataset` thetao, so | Copernicus GLORYS12 | 0.083° | daily |
| `get_argo(...)` | `DataArray` °C | ARMOR3D → INCOIS → GLORYS | 0.125° | daily |

`get_glorys` and `get_argo` return **all native depth levels from 0.494 m to 1062.440 m** (36 levels) — no selection, no interpolation applied.

---

## Caching

All data is cached on the internal NVMe SSD at `/run/media/mohan-krishna/vamSHE/data-pool/`.

- **Full hit** — same or narrower request than what's cached → instant, no network
- **Partial hit** — wider time range → only the missing dates are downloaded and merged
- **Smaller bbox** — if the requested bbox fits inside a cached chunk's bbox → hit
- **Larger bbox** — treated as a miss for that bbox, re-downloaded

```python
cache_list()                 # show all cached chunks
cache_invalidate("sst")      # remove one variable
cache_invalidate()           # remove everything
```

---

## Known Dataset Quirks (handled internally, for reference)

| Dataset | Quirk | Fix |
|---|---|---|
| OSCAR | Native dim order `(time, lon, lat)` | Positional transpose |
| OSCAR | `cftime.DatetimeJulian` time | Normalised to `datetime64[ns]` |
| OSCAR | Non-dim coord `lat` alongside dim `latitude` | Positional detection |
| CCMP | `cftime` breaks `resample()` | Normalise time **before** resample |
| SSS | Spurious `depth=1` dim | `squeeze("depth")` |

---

## Storage Layout

```
/run/media/mohan-krishna/vamSHE/data-pool/
├── cache/  sst/  sss/  ssh/  currents/  winds/  glorys/  argo/
├── meta/   cache_index.json
└── tmp/    (partial downloads, moved on success)
```

---

## What This Does NOT Do

No preprocessing, no grid alignment, no interpolation, no embeddings, no model code, no evaluation metrics. That's entirely the agent's job.
