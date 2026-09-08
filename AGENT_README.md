# OceanEmbed — Agent Guide

You are working on **SIH 2026 PS#01**: reconstruct 3D subsurface ocean temperature (0–1000 m) across the North Indian Ocean from 2D satellite surface observations.

This notebook gives you all the data you need. It handles credentials, downloads, and caching. **You handle everything else.**

---

## Step 1 — Start the notebook

```bash
jupyter lab framework.ipynb
```

Run **every cell from top to bottom, once**. You will see five `✅` messages when it is ready:

```
✅ BLOCK 0 ready
✅ Copernicus Marine auth ready
✅ NASA EarthData auth ready
✅ BLOCK 2: Cache engine ready
✅ BLOCK 3: Data access functions ready
```

After that, write your experiment in **BLOCK 5** (the last cell).

---

## Step 2 — Get data

Seven functions are available. All share the same argument pattern:

```python
get_*(start, end, lat_min, lat_max, lon_min, lon_max)
```

where `start` and `end` are `"YYYY-MM-DD"` strings, and the four floats define a bounding box in degrees.

### Surface inputs (2D)

```python
sst = get_sst("2020-01-01", "2020-03-31", 5.0, 30.0, 45.0, 105.0)
# → xr.DataArray  dims: (time, latitude, longitude)  units: °C
# Source: Copernicus OSTIA  |  0.05° resolution  |  daily

sss = get_sss("2020-01-01", "2020-03-31", 5.0, 30.0, 45.0, 105.0)
# → xr.DataArray  dims: (time, latitude, longitude)  units: PSU
# Source: Copernicus SMAP/SMOS  |  0.125° resolution  |  daily

ssh = get_ssh("2020-01-01", "2020-03-31", 5.0, 30.0, 45.0, 105.0)
# → xr.DataArray  dims: (time, latitude, longitude)  units: m  (sea level anomaly)
# Source: Copernicus DUACS  |  0.25° resolution  |  daily

currents = get_currents("2020-01-01", "2020-03-31", 5.0, 30.0, 45.0, 105.0)
# → xr.Dataset  vars: u, v  units: m/s  dims: (time, latitude, longitude)
# Source: NASA OSCAR V2.0  |  0.25° resolution
# ⚠ CADENCE IS 5-DAY, NOT DAILY. You will get one time step per 5 days.

winds = get_winds("2020-01-01", "2020-03-31", 5.0, 30.0, 45.0, 105.0)
# → xr.Dataset  vars: uwnd, vwnd  units: m/s  dims: (time, latitude, longitude)
# Source: NASA CCMP V3.1  |  0.25° resolution  |  daily mean
```

### Training target (3D)

```python
glorys = get_glorys("2020-01-01", "2020-03-31", 5.0, 30.0, 45.0, 105.0)
# → xr.Dataset  vars: thetao (°C), so (PSU)
#               dims: (time, depth, latitude, longitude)
# Source: Copernicus GLORYS12  |  0.083° resolution  |  daily
#
# Depth: 36 native levels, from 0.494 m to 1062.440 m — ALL returned as-is.
# No depth selection. No interpolation. You get the raw coordinate.
# The 35 levels at or below 1000 m are included, plus the first level above
# (1062.440 m) so you are not cut off just below 1000 m.
```

### Evaluation reference (3D in-situ)

```python
argo = get_argo("2020-01-01", "2020-03-31", 5.0, 30.0, 45.0, 105.0)
# → xr.DataArray  units: °C  dims: (time, depth, latitude, longitude)
# Source: ARMOR3D (Copernicus) → INCOIS LAS → GLORYS fallback
# Native depth levels returned as-is. No selection. No interpolation.
```

---

## Step 3 — Use the data

The returned objects are standard xarray. Do whatever you want with them:

```python
import numpy as np

# Access a variable
temp = glorys["thetao"]   # shape: (time, depth, latitude, longitude)

# Check depth levels
print(glorys.depth.values)
# [  0.494   1.541   2.646 ...  902.339 1062.440]

# Stack into a numpy array
arr = temp.values   # float32, shape (T, D, H, W)

# Select a specific depth (you decide how)
surface = temp.sel(depth=0.494)         # exact match
near_100m = temp.isel(depth=10)         # by index

# Align OSCAR (5-day) to daily SST time axis if needed
daily_times = sst.time.values
currents_daily = currents.interp(time=daily_times, method="nearest")
```

---

## Caching — what you need to know

The framework caches everything on the internal SSD at:
```
/run/media/mohan-krishna/vamSHE/data-pool/cache/
```

**You do not need to think about it.** It works automatically:

| Situation | What happens |
|---|---|
| First call for a date range | Downloads from server, saves to SSD |
| Same call again | Returns from SSD instantly — no network |
| Wider date range than cached | Downloads only the missing dates, merges |
| Narrower date range than cached | Returns from SSD instantly, sliced |
| Smaller bbox than cached | Returns from SSD instantly, sliced |
| Larger bbox than cached | Downloads fresh for the new bbox |
| `force=True` passed | Ignores cache, re-downloads everything |

```python
# Inspect the cache
cache_list()

# Remove one variable from cache (if you need a clean re-download)
cache_invalidate("sst")

# Remove everything
cache_invalidate()
```

---

## Data notes — read these

**OSCAR cadence is 5-day.**
`get_currents(...)` returns one time step per ~5 days. If you need daily alignment, interpolate yourself — the framework does not do this for you.

**Native grids differ.**
Each dataset has its own spatial resolution (0.05° to 0.25°). The framework does not regrid. Alignment is your responsibility.

**GLORYS depth is native.**
The 36 depth values are not round numbers. They are GLORYS12 model levels. If your target output is at specific depths (e.g., 100 m), select or interpolate however your approach requires.

**Time is always `datetime64[ns]`.**
All returned objects have their time coordinate normalised to `datetime64[ns]`. You can safely use `pd.DatetimeIndex(da.time.values)`, `.sel(time=...)`, `.resample(time="1D")`, etc.

**NaNs are land masks.**
All datasets have `NaN` over land. Handle them before feeding to models.

---

## What this framework does NOT do

- No grid alignment or regridding
- No depth interpolation or selection
- No normalisation or scaling
- No preprocessing or feature engineering
- No model, no embeddings, no evaluation code

All of that is your job.

---

## Quick copy-paste start

```python
# Minimal experiment skeleton — paste into BLOCK 5 and modify

START    = "2020-01-01"
END      = "2020-03-31"
LAT_MIN  = 5.0
LAT_MAX  = 30.0
LON_MIN  = 45.0
LON_MAX  = 105.0

# Surface inputs
sst      = get_sst(START, END, LAT_MIN, LAT_MAX, LON_MIN, LON_MAX)
sss      = get_sss(START, END, LAT_MIN, LAT_MAX, LON_MIN, LON_MAX)
ssh      = get_ssh(START, END, LAT_MIN, LAT_MAX, LON_MIN, LON_MAX)
currents = get_currents(START, END, LAT_MIN, LAT_MAX, LON_MIN, LON_MAX)
winds    = get_winds(START, END, LAT_MIN, LAT_MAX, LON_MIN, LON_MAX)

# Training target
glorys   = get_glorys(START, END, LAT_MIN, LAT_MAX, LON_MIN, LON_MAX)
target   = glorys["thetao"]   # shape: (time, depth, lat, lon)

# Evaluation reference
argo     = get_argo(START, END, LAT_MIN, LAT_MAX, LON_MIN, LON_MAX)

print("SST:      ", sst.shape)
print("SSS:      ", sss.shape)
print("SSH:      ", ssh.shape)
print("Currents: ", currents["u"].shape, "— 5-day cadence")
print("Winds:    ", winds["uwnd"].shape)
print("GLORYS:   ", target.shape, "— depths:", target.depth.values[[0, -1]])
print("ARGO:     ", argo.shape)

# Your model / hypothesis goes here
```

---

## Function signatures (quick reference)

```
get_sst(start, end, lat_min, lat_max, lon_min, lon_max, force=False)
    → xr.DataArray (time, latitude, longitude)  °C

get_sss(start, end, lat_min, lat_max, lon_min, lon_max, force=False)
    → xr.DataArray (time, latitude, longitude)  PSU

get_ssh(start, end, lat_min, lat_max, lon_min, lon_max, force=False)
    → xr.DataArray (time, latitude, longitude)  m

get_currents(start, end, lat_min, lat_max, lon_min, lon_max, force=False)
    → xr.Dataset   (time, latitude, longitude)  u, v  m/s  [5-day]

get_winds(start, end, lat_min, lat_max, lon_min, lon_max, force=False)
    → xr.Dataset   (time, latitude, longitude)  uwnd, vwnd  m/s  [daily]

get_glorys(start, end, lat_min, lat_max, lon_min, lon_max, force=False)
    → xr.Dataset   (time, depth, latitude, longitude)  thetao °C, so PSU

get_argo(start, end, lat_min, lat_max, lon_min, lon_max, force=False)
    → xr.DataArray (time, depth, latitude, longitude)  °C

cache_list()
    → prints all cached chunks (variable, date range, size)

cache_invalidate(variable=None)
    → deletes cache entries; None deletes everything
```
