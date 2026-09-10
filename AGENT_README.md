# OceanEmbed — Agent Guide

You are working on **SIH 2026 PS#01**: reconstruct 3D subsurface ocean temperature (0–1000 m) across the North Indian Ocean from surface satellite observations.

This notebook gives you **10 datasets, ~242 GB pre-cached on a local SSD**. It handles credentials, downloads, and caching. **You handle everything else.**

All data is served **as-is** — native resolution, native temporal cadence, native depth levels. No interpolation, no regridding, no resampling. Raw.

---

## Step 1 — Start the notebook

```bash
jupyter lab framework.ipynb
```

Run **every cell from top to bottom, once**. You will see these messages when ready:

```
✅ BLOCK 0 ready
✅ Copernicus Marine auth ready
✅ NASA EarthData auth ready
✅ BLOCK 2: Cache engine ready
✅ BLOCK 3: Data access functions ready
```

Write your experiment in **BLOCK 5** (the last cell).

---

## Step 2 — Get data

### Surface inputs (2D gridded)

```python
sst = get_sst("2020-01-01", "2020-12-31", 5.0, 30.0, 45.0, 105.0)
# → xr.DataArray  (time, latitude, longitude)  °C
# Source: Copernicus OSTIA  |  0.05°  |  daily
# Cached: 2015–2020, NIO

sss = get_sss("2020-01-01", "2020-12-31", 5.0, 30.0, 45.0, 105.0)
# → xr.DataArray  (time, latitude, longitude)  PSU
# Source: Copernicus SMAP/SMOS  |  0.125°  |  daily

ssh = get_ssh("2020-01-01", "2020-12-31", 5.0, 30.0, 45.0, 105.0)
# → xr.DataArray  (time, latitude, longitude)  m  (sea level anomaly)
# Source: Copernicus DUACS  |  0.25°  |  daily

currents = get_currents("2020-01-01", "2020-12-31", 5.0, 30.0, 45.0, 105.0)
# → xr.Dataset  vars: u, v  units: m/s  (time, latitude, longitude)
# Source: NASA OSCAR V2.0  |  0.25°  |  daily
# Cached: 2015–2020, GLOBAL extent

winds = get_winds("2020-01-01", "2020-12-31", 5.0, 30.0, 45.0, 105.0)
# → xr.Dataset  vars: uwnd, vwnd, ws, nobs  (time, latitude, longitude)
# Source: NASA CCMP V3.1  |  0.25°
# ⚠ RAW 6-HOURLY — NOT resampled to daily. 1460 timesteps per year.
# 4 variables: uwnd, vwnd (vector), ws (scalar speed), nobs (obs count)
# Cached: 2015–2020, GLOBAL extent
# If you need daily: winds.resample(time="1D").mean()
```

### Training target (3D)

```python
glorys = get_glorys("2020-01-01", "2020-12-31", 5.0, 30.0, 45.0, 105.0)
# → xr.Dataset  vars: thetao (°C), so (PSU)
#               dims: (time, depth, latitude, longitude)
# Source: Copernicus GLORYS12  |  0.083° (1/12°)  |  daily
# 36 native depth levels from 0.494 m to 1062.440 m
# Cached: 2015–2020, NIO
```

### Evaluation reference — ARMOR3D (3D)

```python
argo = get_argo("2020-01-01", "2020-12-31", 5.0, 30.0, 45.0, 105.0)
# → xr.Dataset  var: to (°C)
#               dims: (time, depth, latitude, longitude)
# Source: Copernicus ARMOR3D  |  0.125°  |  daily
# Cached: 2015–2020, NIO
```

### INCOIS Argo products (3D, 10-day, local files)

```python
vam = get_incois_vam("2015-01-01", "2020-12-31")
# → xr.Dataset  vars: TEMP, TERR, SAL, SERR
#               dims: (time, ZAX, latitude, longitude)
# Source: INCOIS LAS  |  1°  |  10-day cadence
# Depth axis: ZAX — 24 levels [5, 10, 20, 30, 50, 75, 100, 125, 150, 200,
#   250, 300, 400, 500, 600, 700, 800, 900, 1000, 1200, 1400, 1600, 1800, 2000] m
# Coverage: 29.5°S–29.5°N, 30.5–119.5°E, 2004–2026
# Always instant (fully cached, single file, 1.7 GB)

mcc = get_incois_mccreary("2015-01-01", "2020-12-31")
# → xr.Dataset  vars: T_ANALYZED, T_MEAN, T_STDEV, T_RMSE, T_ROIOBS, T_BOXOBS,
#                      S_ANALYZED, S_MEAN, S_STDEV, S_RMSE, S_ROIOBS, S_BOXOBS
#               dims: (time, ZAX, latitude, longitude)
# Source: INCOIS LAS  |  1°  |  10-day cadence
# Same depth axis and spatial coverage as VAM.
# Coverage: 2001–2026 (4 cached chunks, merged seamlessly)
# Always instant (5.7 GB)
```

### ASCAT-C L2 swath wind vectors

```python
ascatc = get_ascatc("2020-01-01", "2020-03-31")
# → xr.Dataset  (orbit, NUMROWS, NUMCELLS)
# Variables: wind_speed, wind_dir, model_speed, model_dir,
#            ice_prob, ice_age, wvc_quality_flag, bs_distance
# Coordinates: lat, lon (2D arrays per orbit pass)
# Source: EUMETSAT ASCAT MetOp-C  |  12.5 km swath
# ⚠ THIS IS L2 SWATH DATA — NOT GRIDDED.
# Each orbit is a separate satellite pass. lat/lon are 2D arrays.
# Coverage: 2019-10-22 to 2020-12-31, 6,191 orbit files, 21 GB

# Alternative: get file paths for manual loading
paths = get_ascatc("2020-01-01", "2020-01-31", as_list=True)
# → list[Path], one per orbit file
# Then load individually: xr.open_dataset(paths[0])
```

---

## Caching — what you need to know

**2015–2020 is fully pre-cached for all datasets.** Requests within this range hit instantly.

Cache lives on internal NVMe SSD: `/run/media/mohan-krishna/vamSHE/data-pool/cache/`

| Situation | What happens |
|---|---|
| Request within cached range | Returns from SSD instantly — no network |
| Wider date range than cached | Downloads only the missing dates, merges |
| Narrower date range | Returns from SSD instantly, sliced |
| Smaller bbox (inside cached bbox) | Returns from SSD, sliced |
| Larger bbox than cached | Downloads for the new bbox |
| `force=True` | Ignores cache, re-downloads |

```python
cache_list()                 # show all cached chunks
cache_invalidate("sst")      # remove one variable
cache_invalidate()           # remove everything
```

---

## Data notes — read these

**Winds are 6-hourly.**
`get_winds()` returns raw CCMP 6-hourly data with 4 variables. If you need daily, call `ds.resample(time="1D").mean()` yourself.

**Currents and winds are global.**
Cached at full global extent. If you only need NIO, slice with `.sel(latitude=slice(5,30), longitude=slice(45,105))`.

**Native grids differ.**
Each dataset has its own spatial resolution (0.05° to 1°). Grid alignment is your job.

**GLORYS depth is native.**
36 levels, not round numbers. Use `glorys.depth.values` to see them.

**INCOIS depth axis is `ZAX`, not `depth`.**
24 levels at round numbers (5, 10, 20, ..., 2000 m). Access as `ds.ZAX`.

**ASCAT-C is NOT gridded.**
L2 swath data — each file is one orbit. `lat` and `lon` are 2D arrays, not dimension coordinates. Cannot use `.sel()` for spatial slicing. This is for advanced usage.

**Time is always `datetime64[ns]`.**
All returned objects have normalised time. Safe for `.sel(time=...)`, `.resample()`, etc.

**NaNs are land/missing.**
Handle before feeding to models.

---

## Dataset summary table

| # | Function | Return type | Dims | Resolution | Cadence | Extent | Period |
|---|---|---|---|---|---|---|---|
| 1 | `get_sst(...)` | DataArray | (T, lat, lon) | 0.05° | daily | NIO | 2015–2020 |
| 2 | `get_sss(...)` | DataArray | (T, lat, lon) | 0.125° | daily | NIO | 2015–2020 |
| 3 | `get_ssh(...)` | DataArray | (T, lat, lon) | 0.25° | daily | NIO | 2015–2020 |
| 4 | `get_currents(...)` | Dataset | (T, lat, lon) | 0.25° | daily | Global | 2015–2020 |
| 5 | `get_winds(...)` | Dataset | (T, lat, lon) | 0.25° | **6-hourly** | Global | 2015–2020 |
| 6 | `get_glorys(...)` | Dataset | (T, D, lat, lon) | 0.083° | daily | NIO | 2015–2020 |
| 7 | `get_argo(...)` | Dataset | (T, D, lat, lon) | 0.125° | daily | NIO | 2015–2020 |
| 8 | `get_incois_vam(...)` | Dataset | (T, ZAX, lat, lon) | 1° | 10-day | Indian Ocean | 2004–2026 |
| 9 | `get_incois_mccreary(...)` | Dataset | (T, ZAX, lat, lon) | 1° | 10-day | Indian Ocean | 2001–2026 |
| 10 | `get_ascatc(...)` | Dataset | (orbit, rows, cells) | 12.5 km | per-pass | Swath | 2019-10 to 2020 |

**Total cached: ~242 GB**

---

## Function signatures (quick reference)

```
# ── Gridded (start, end, bbox required) ──────────────────────────────────
get_sst(start, end, lat_min, lat_max, lon_min, lon_max, force=False)       → DataArray
get_sss(start, end, lat_min, lat_max, lon_min, lon_max, force=False)       → DataArray
get_ssh(start, end, lat_min, lat_max, lon_min, lon_max, force=False)       → DataArray
get_currents(start, end, lat_min, lat_max, lon_min, lon_max, force=False)  → Dataset
get_winds(start, end, lat_min, lat_max, lon_min, lon_max, force=False)     → Dataset
get_glorys(start, end, lat_min, lat_max, lon_min, lon_max, force=False)    → Dataset
get_argo(start, end, lat_min, lat_max, lon_min, lon_max, force=False)      → Dataset

# ── INCOIS (optional date slicing, no bbox) ──────────────────────────────
get_incois_vam(start=None, end=None)                                       → Dataset
get_incois_mccreary(start=None, end=None)                                  → Dataset

# ── ASCAT-C L2 swath (optional date filter) ──────────────────────────────
get_ascatc(start=None, end=None, as_list=False)                            → Dataset or list[Path]

# ── Cache utilities ──────────────────────────────────────────────────────
cache_list()
cache_invalidate(variable=None)
```

---

## Quick copy-paste start

```python
# Paste into BLOCK 5 and modify

START    = "2020-01-01"
END      = "2020-12-31"
LAT_MIN  = 5.0
LAT_MAX  = 30.0
LON_MIN  = 45.0
LON_MAX  = 105.0

# Surface inputs (2D)
sst      = get_sst(START, END, LAT_MIN, LAT_MAX, LON_MIN, LON_MAX)
sss      = get_sss(START, END, LAT_MIN, LAT_MAX, LON_MIN, LON_MAX)
ssh      = get_ssh(START, END, LAT_MIN, LAT_MAX, LON_MIN, LON_MAX)
currents = get_currents(START, END, LAT_MIN, LAT_MAX, LON_MIN, LON_MAX)
winds    = get_winds(START, END, LAT_MIN, LAT_MAX, LON_MIN, LON_MAX)

# Training target (3D)
glorys   = get_glorys(START, END, LAT_MIN, LAT_MAX, LON_MIN, LON_MAX)

# Evaluation (3D)
argo     = get_argo(START, END, LAT_MIN, LAT_MAX, LON_MIN, LON_MAX)

# INCOIS (local files, instant)
vam      = get_incois_vam(START, END)
mcc      = get_incois_mccreary(START, END)

# ASCAT-C L2 swath (per-orbit, advanced)
ascatc   = get_ascatc(START, END)

print("SST:      ", sst.shape)
print("SSS:      ", sss.shape)
print("SSH:      ", ssh.shape)
print("Currents: ", currents["u"].shape)
print("Winds:    ", winds["uwnd"].shape, "— 6-hourly!")
print("GLORYS:   ", glorys["thetao"].shape)
print("ARGO:     ", argo["to"].shape)
print("INCOIS VAM:", vam["TEMP"].shape)
print("INCOIS MCC:", mcc["T_ANALYZED"].shape)
```

---

## What this framework does NOT do

- No grid alignment or regridding
- No temporal resampling (winds stay 6-hourly, INCOIS stays 10-day)
- No depth interpolation or selection
- No normalisation or scaling
- No preprocessing or feature engineering
- No model, no embeddings, no evaluation code

All of that is your job.
