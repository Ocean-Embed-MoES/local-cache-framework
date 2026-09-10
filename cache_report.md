# 📦 Cache Report — OceanEmbed Data Pool

> **Generated**: 2026-09-10
> **Cache root**: `/run/media/mohan-krishna/vamSHE/data-pool/cache/`
> **Index file**: `/run/media/mohan-krishna/vamSHE/data-pool/meta/cache_index.json`

---

## Summary

| # | Dataset | Product ID | Source | Resolution | Cadence | Spatial Extent | Period | Total Size |
|---|---------|-----------|--------|------------|---------|----------------|--------|------------|
| 1 | SST | `METOFFICE-GLO-SST-L4-REP-OBS-SST` | Copernicus OSTIA | 0.05° | Daily | NIO (5–30°N, 45–105°E) | 2015–2020 | **10.5 GB** |
| 2 | SSS | `cmems_obs-mob_glo_phy-sss_my_multi_P1D` | Copernicus SMAP/SMOS | 0.125° | Daily | NIO (5–30°N, 45–105°E) | 2015–2020 | **0.8 GB** |
| 3 | SSH | `c3s_obs-sl_glo_phy-ssh_my_twosat-l4-duacs-0.25deg_P1D` | Copernicus DUACS | 0.25° | Daily | NIO (5–30°N, 45–105°E) | 2015–2020 | **0.2 GB** |
| 4 | Currents | `OSCAR_L4_OC_FINAL_V2.0` | NASA OSCAR | 0.25° | Daily | Global | 2015–2020 | **36.3 GB** |
| 5 | Winds | `CCMP_WINDS_10M6HR_L4_V3.1` | NASA CCMP | 0.25° | **6-hourly** | Global | 2015–2020 | **73.4 GB** |
| 6 | GLORYS | `cmems_mod_glo_phy_my_0.083deg_P1D-m` | Copernicus GLORYS12 | 0.083° (1/12°) | Daily | NIO (5–30°N, 45–105°E) | 2015–2020 | **68.4 GB** |
| 7 | Argo (ARMOR3D) | `cmems_obs-mob_glo_phy_my_0.125deg_P1D-m` | Copernicus ARMOR3D | 0.125° | Daily | NIO (5–30°N, 45–105°E) | 2015–2020 | **24.3 GB** |
| 8 | INCOIS Argo VAM | `INCOIS_ARGO_10DAY_VAM` | INCOIS LAS | 1° | 10-day | 29.5°S–29.5°N, 30.5–119.5°E | 2004–2026 | **1.7 GB** |
| 9 | INCOIS Argo McCreary | `INCOIS_ARGO_10DAY_MCCREARY` | INCOIS LAS | 1° | 10-day | 29.5°S–29.5°N, 30.5–119.5°E | 2001–2026 | **5.7 GB** |
| 10 | ASCAT-C L2 Coastal | `ASCATC-L2-Coastal` | MetOp-C ASCAT (EUMETSAT) | 12.5 km (swath) | Per-pass | Swath (not gridded) | 2019-10 – 2020 | **21.0 GB** |

**Total cached: ~242 GB**

---

## Detailed Breakdown

### 1. SST — Sea Surface Temperature

| Year | Time Range | Timesteps | Cadence | Spatial Extent | Variables | Size |
|------|-----------|-----------|---------|----------------|-----------|------|
| 2015 | 2015-01-01 → 2015-12-31 | 365 | Daily | 5–30°N, 45–105°E | `analysed_sst` | 1,752 MB |
| 2016 | 2016-01-01 → 2016-12-31 | 366 | Daily | 5–30°N, 45–105°E | `analysed_sst` | 1,757 MB |
| 2017 | 2017-01-01 → 2017-12-31 | 365 | Daily | 5–30°N, 45–105°E | `analysed_sst` | 1,752 MB |
| 2018 | 2018-01-01 → 2018-12-31 | 365 | Daily | 5–30°N, 45–105°E | `analysed_sst` | 1,752 MB |
| 2019 | 2019-01-01 → 2019-12-31 | 365 | Daily | 5–30°N, 45–105°E | `analysed_sst` | 1,752 MB |
| 2020 | 2020-01-01 → 2020-12-31 | 366 | Daily | 5–30°N, 45–105°E | `analysed_sst` | 1,757 MB |

> SST values stored as °C (converted from Kelvin by framework_loader.py).

---

### 2. SSS — Sea Surface Salinity

| Year | Time Range | Timesteps | Cadence | Spatial Extent | Variables | Size |
|------|-----------|-----------|---------|----------------|-----------|------|
| 2015 | 2015-01-01 → 2015-12-31 | 365 | Daily | 5–30°N, 45–105°E | `sos` | 140 MB |
| 2016 | 2016-01-01 → 2016-12-31 | 366 | Daily | 5–30°N, 45–105°E | `sos` | 141 MB |
| 2017 | 2017-01-01 → 2017-12-31 | 365 | Daily | 5–30°N, 45–105°E | `sos` | 140 MB |
| 2018 | 2018-01-01 → 2018-12-31 | 365 | Daily | 5–30°N, 45–105°E | `sos` | 140 MB |
| 2019 | 2019-01-01 → 2019-12-31 | 365 | Daily | 5–30°N, 45–105°E | `sos` | 140 MB |
| 2020 | 2020-01-01 → 2020-12-31 | 366 | Daily | 5–30°N, 45–105°E | `sos` | 141 MB |

---

### 3. SSH — Sea Surface Height (Sea Level Anomaly)

| Year | Time Range | Timesteps | Cadence | Spatial Extent | Variables | Size |
|------|-----------|-----------|---------|----------------|-----------|------|
| 2015 | 2015-01-01 → 2015-12-31 | 365 | Daily | 5–30°N, 45–105°E | `sla` | 35 MB |
| 2016 | 2016-01-01 → 2016-12-31 | 366 | Daily | 5–30°N, 45–105°E | `sla` | 35 MB |
| 2017 | 2017-01-01 → 2017-12-31 | 365 | Daily | 5–30°N, 45–105°E | `sla` | 35 MB |
| 2018 | 2018-01-01 → 2018-12-31 | 365 | Daily | 5–30°N, 45–105°E | `sla` | 35 MB |
| 2019 | 2019-01-01 → 2019-12-31 | 365 | Daily | 5–30°N, 45–105°E | `sla` | 35 MB |
| 2020 | 2020-01-01 → 2020-12-31 | 366 | Daily | 5–30°N, 45–105°E | `sla` | 35 MB |

---

### 4. Currents — Surface Ocean Currents (NASA OSCAR)

| Year | Time Range | Timesteps | Cadence | Spatial Extent | Variables | Size |
|------|-----------|-----------|---------|----------------|-----------|------|
| 2015 | 2015-01-01 → 2015-12-31 | 365 | Daily | Global (−90–90°, 0–360°) | `u`, `v` | 6,047 MB |
| 2016 | 2016-01-01 → 2016-12-31 | 366 | Daily | Global (−90–90°, 0–360°) | `u`, `v` | 6,063 MB |
| 2017 | 2017-01-01 → 2017-12-31 | 365 | Daily | Global (−90–90°, 0–360°) | `u`, `v` | 6,047 MB |
| 2018 | 2018-01-01 → 2018-12-31 | 365 | Daily | Global (−90–90°, 0–360°) | `u`, `v` | 6,047 MB |
| 2019 | 2019-01-01 → 2019-12-31 | 365 | Daily | Global (−90–90°, 0–360°) | `u`, `v` | 6,047 MB |
| 2020 | 2020-01-01 → 2020-12-31 | 366 | Daily | Global (−90–90°, 0–360°) | `u`, `v` | 6,063 MB |

> Ingested from local S3 download. Full global extent stored — no spatial cropping.

---

### 5. Winds — 10m Wind Vectors (NASA CCMP V3.1)

| Year | Time Range | Timesteps | Cadence | Spatial Extent | Variables | Size |
|------|-----------|-----------|---------|----------------|-----------|------|
| 2015 | 2015-01-01 → 2015-12-31 | 1,460 | **6-hourly** | Global (−89.875–89.875°, 0.125–359.875°) | `uwnd`, `vwnd`, `ws`, `nobs` | 12,212 MB |
| 2016 | 2016-01-01 → 2016-12-31 | 1,464 | **6-hourly** | Global (−89.875–89.875°, 0.125–359.875°) | `uwnd`, `vwnd`, `ws`, `nobs` | 12,253 MB |
| 2017 | 2017-01-01 → 2017-12-31 | 1,460 | **6-hourly** | Global (−89.875–89.875°, 0.125–359.875°) | `uwnd`, `vwnd`, `ws`, `nobs` | 12,213 MB |
| 2018 | 2018-01-01 → 2018-12-31 | 1,460 | **6-hourly** | Global (−89.875–89.875°, 0.125–359.875°) | `uwnd`, `vwnd`, `ws`, `nobs` | 12,218 MB |
| 2019 | 2019-01-01 → 2019-12-31 | 1,460 | **6-hourly** | Global (−89.875–89.875°, 0.125–359.875°) | `uwnd`, `vwnd`, `ws`, `nobs` | 12,233 MB |
| 2020 | 2020-01-01 → 2020-12-31 | 1,464 | **6-hourly** | Global (−89.875–89.875°, 0.125–359.875°) | `uwnd`, `vwnd`, `ws`, `nobs` | 12,245 MB |

> Raw 6-hourly data — NO resampling to daily. All 4 variables preserved.

---

### 6. GLORYS — 3D Reanalysis (Training Target)

| Year | Time Range | Timesteps | Cadence | Spatial Extent | Variables | Size |
|------|-----------|-----------|---------|----------------|-----------|------|
| 2015 | 2015-01-01 → 2015-12-31 | 365 | Daily | 5–30°N, 45–105°E | `thetao`, `so` | 11,407 MB |
| 2016 | 2016-01-01 → 2016-12-31 | 366 | Daily | 5–30°N, 45–105°E | `thetao`, `so` | 11,438 MB |
| 2017 | 2017-01-01 → 2017-12-31 | 365 | Daily | 5–30°N, 45–105°E | `thetao`, `so` | 11,407 MB |
| 2018 | 2018-01-01 → 2018-12-31 | 365 | Daily | 5–30°N, 45–105°E | `thetao`, `so` | 11,407 MB |
| 2019 | 2019-01-01 → 2019-12-31 | 365 | Daily | 5–30°N, 45–105°E | `thetao`, `so` | 11,407 MB |
| 2020 | 2020-01-01 → 2020-12-31 | 366 | Daily | 5–30°N, 45–105°E | `thetao`, `so` | 11,438 MB |

> 0.083° (1/12°) resolution, 50 depth levels. Contains temperature (thetao) and salinity (so).

---

### 7. Argo — ARMOR3D (Copernicus)

| Year | Time Range | Timesteps | Cadence | Spatial Extent | Variables | Size |
|------|-----------|-----------|---------|----------------|-----------|------|
| 2015 | 2015-01-01 → 2015-12-31 | 365 | Daily | 5–30°N, 45–105°E | `to` | 4,048 MB |
| 2016 | 2016-01-01 → 2016-12-31 | 366 | Daily | 5–30°N, 45–105°E | `to` | 4,059 MB |
| 2017 | 2017-01-01 → 2017-12-31 | 365 | Daily | 5–30°N, 45–105°E | `to` | 4,048 MB |
| 2018 | 2018-01-01 → 2018-12-31 | 365 | Daily | 5–30°N, 45–105°E | `to` | 4,048 MB |
| 2019 | 2019-01-01 → 2019-12-31 | 365 | Daily | 5–30°N, 45–105°E | `to` | 4,048 MB |
| 2020 | 2020-01-01 → 2020-12-31 | 366 | Daily | 5–30°N, 45–105°E | `to` | 4,059 MB |

---

### 8. INCOIS Argo VAM — Variational Analysis Methodology

| Chunk | Time Range | Timesteps | Cadence | Spatial Extent | Depth Levels | Size |
|-------|-----------|-----------|---------|----------------|-------------|------|
| 1 | 2004-01-10 → 2026-07-30 | 813 | 10-day | 29.5°S–29.5°N, 30.5–119.5°E | 24 (5–2000 m) | 1,686 MB |

**Variables**: `TEMP` (temperature), `TERR` (temp error), `SAL` (salinity), `SERR` (sal error)

---

### 9. INCOIS Argo McCreary — Kessler-McCreary Methodology

| Chunk | Time Range | Timesteps | Cadence | Spatial Extent | Depth Levels | Size |
|-------|-----------|-----------|---------|----------------|-------------|------|
| 1 | 2001-01-10 → 2007-12-30 | 252 | 10-day | 29.5°S–29.5°N, 30.5–119.5°E | 24 (5–2000 m) | 1,568 MB |
| 2 | 2008-01-10 → 2015-12-30 | 288 | 10-day | 29.5°S–29.5°N, 30.5–119.5°E | 24 (5–2000 m) | 1,792 MB |
| 3 | 2016-01-10 → 2023-12-30 | 288 | 10-day | 29.5°S–29.5°N, 30.5–119.5°E | 24 (5–2000 m) | 1,792 MB |
| 4 | 2023-12-30 → 2026-07-30 | 94 | 10-day | 29.5°S–29.5°N, 30.5–119.5°E | 24 (5–2000 m) | 585 MB |

**Variables**: `T_ANALYZED`, `T_MEAN`, `T_STDEV`, `T_RMSE`, `T_ROIOBS`, `T_BOXOBS`, `S_ANALYZED`, `S_MEAN`, `S_STDEV`, `S_RMSE`, `S_ROIOBS`, `S_BOXOBS`

---

### 10. ASCAT-C — MetOp ASCAT Level 2 Ocean Surface Wind Vectors Optimized for Coastal Ocean

| Item | Detail |
|------|--------|
| Product | `ASCATC-L2-Coastal` |
| Source | EUMETSAT / OSI SAF / KNMI (MetOp-C ASCAT) |
| Data type | **L2 swath** (not gridded) — one file per satellite pass |
| Resolution | 12.5 km cross-track |
| Date range | 2019-10-22 → 2020-12-31 |
| File count | 6,191 .nc files |
| Total size | 21,046 MB |
| Variables | `wind_speed`, `wind_dir`, `model_speed`, `model_dir`, `ice_prob`, `ice_age`, `wvc_quality_flag`, `bs_distance` |
| Dims | `NUMROWS` × `NUMCELLS` (varies per pass) |
| Coordinates | `lat`, `lon` (2D arrays per pass) |

> This is raw L2 swath data, not gridded like the other datasets. Each file is a single satellite overpass. Stored as-is in `cache/ascatc/<year>/`.

---

## Coverage vs Problem Statement Requirements

| Requirement | Required | Cached | Status |
|-------------|----------|--------|--------|
| SST (OSTIA, 0.05°, daily) | ✅ | 2015–2020, NIO, daily | ✅ Complete |
| SSS (SMAP/SMOS, 0.125°, daily) | ✅ | 2015–2020, NIO, daily | ✅ Complete |
| SSH/SLA (DUACS, 0.25°, daily) | ✅ | 2015–2020, NIO, daily | ✅ Complete |
| Currents (OSCAR, 0.25°) | ✅ | 2015–2020, Global, daily | ✅ Complete |
| Winds — CCMP (0.25°) | ✅ | 2015–2020, Global, 6-hourly | ✅ Complete |
| Winds — ASCAT-C L2 | ✅ | 2019-10 – 2020, swath | ⚠️ Partial (2019 Oct onward only) |
| GLORYS target (0.083°, daily) | ✅ | 2015–2020, NIO, daily | ✅ Complete |
| Gridded Argo (ARMOR3D) | ✅ | 2015–2020, NIO, daily | ✅ Complete |
| INCOIS Argo (LAS) | ✅ | 2001–2026, Indian Ocean, 10-day | ✅ Complete |
| Region: NIO (5–30°N, 45–105°E) | ✅ | All Copernicus = NIO, NASA = Global (superset) | ✅ Covered |
| Period: 2015–2020 | ✅ | All 6 years present (ASCAT-C partial) | ✅ Complete |

---

## Legacy / Test Entries

The cache also contains some older small entries from earlier test runs. These don't interfere with the main data:

- Small BoB-extent entries (9.5–25.5°N, 79.5–100.5°E) for SST, SSS, SSH, GLORYS (2019–2020)
- Tiny 3-day test entries (2020-01-01 → 2020-01-03) for multiple variables

These can be safely ignored or cleaned up if desired.
