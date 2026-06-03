# Geometric Data

GloGEM's geometric input consists of the glacier inventory, per-glacier hypsometry and ice thickness, and supraglacial debris observations. All geometric data are stored on the VAW shared storage under:

```
/itet-stor/<username>/glogem/geometricdata/rgiv<version>/
```

---

## Glacier inventory batch file

For each RGI region, a batch file lists all glaciers with their key inventory attributes. This file drives the glacier loop and provides area, volume, and coordinate metadata.

### Storage path

```
geometricdata/rgiv<version>/bands/files/thick_<region>.dat
```

### Contents

One row per glacier (semicolon-delimited ID, then 19 space-separated values):

| Column (0-based) | Variable | Unit | Description |
|-----------------|----------|------|-------------|
| ID | — | — | RGI glacier ID |
| 0 | `lon_gl` | (°) | Glacier centroid longitude |
| 1 | `lat_gl` | (°) | Glacier centroid latitude |
| 2 | `a_gl` | (km²) | Glacier area |
| 3 | `volume_ini` | (km³) | Initial ice volume |
| 4 | THICK_mean | (m) | Mean ice thickness |
| 5 | THICK_max | (m) | Maximum ice thickness |
| 6 | `hmin` | (m a.s.l.) | Minimum surface elevation |
| 7 | ELEV_max | (m a.s.l.) | Maximum surface elevation |
| 8 | `hmed` | (m a.s.l.) | Median surface elevation |
| 9 | LENGTH | (km) | Glacier length |
| 10 | SLOPE_avg | (°) | Average surface slope |
| 11 | SLOPE_band | (°) | Average band slope |
| 12 | TAU_avg | (bar) | Average basal shear stress |
| 13 | TAU_min | (bar) | Minimum basal shear stress |
| 14 | TAU_max | (bar) | Maximum basal shear stress |
| 15 | SF_avg | (—) | Average shape factor |
| 16 | SF_min | (—) | Minimum shape factor |
| 17 | R_V | (—) | Volume ratio |
| 18 | `survey_year` | (—) | RGI inventory survey year |

---

## Hypsometry files

Each glacier is represented by an elevation-band file derived from the RGI outline and a consensus ice thickness dataset.

### Storage path

```
geometricdata/rgiv<version>/bands/bands_consensus2019/<region>/<RGI_ID>.dat
```

If the primary file contains implausible thickness values (below −300 m) or if the file area disagrees with the inventory by more than 50%, GloGEM automatically falls back to the Huss and Farinotti (2012) dataset:

```
geometricdata/rgiv<version>/bands/bands_HF2012/<region>/<RGI_ID>.dat
```

### File format

Plain-text file with a 5-line header followed by one row per elevation band. Each row contains 12 columns:

| Column index | Variable | Unit | Description |
|-------------|----------|------|-------------|
| 0 | Band-ID | (—) | Sequential band index |
| 1 | Elev_start | (m a.s.l.) | Lower boundary of elevation band |
| 2 | Elev_end | (m a.s.l.) | Upper boundary of elevation band |
| 3 | Area | (km²) | Band area |
| 4 | Thickness | (m) | Mean ice thickness |
| 5 | Width | (m) | Mean glacier width |
| 6 | Length | (m) | Band length |
| 7 | Slope | (°) | Mean surface slope |
| 8 | Aspect | (—) | Dominant aspect class |
| 9 | b_app | (m a⁻¹) | Apparent mass balance |
| 10 | Basal_stress | (bar) | Basal shear stress |
| 11 | Shape_factor | (—) | Valley shape factor |

Elevation bands are 10 m wide (Elev_end − Elev_start = 10 m). Bands are ordered from the lowest (terminus) to the highest (headwall) elevation. Columns 9–11 use −9.0 as a no-data fill value where observations are unavailable.

---

## Debris observations

Per-glacier supraglacial debris properties are stored alongside the hypsometry data. They are read when `debris_supraglacial = 'y'`.

### Storage path

```
geometricdata/rgiv<version>/bands/debris/<region>/debris_<RGI_ID>.dat
geometricdata/rgiv<version>/bands/debris/<region>/factor_<RGI_ID>.dat
```

If no debris file exists for a glacier, all debris properties default to zero (no debris) and the melt reduction factor defaults to 1 (no reduction).

### `debris_<RGI_ID>.dat` — per-band debris properties

Plain-text file with a 5-line header, one row per elevation band, 8 columns:

| Column index | Variable | Description |
|-------------|----------|-------------|
| 4 | `debris_frac` | Debris coverage fraction per band (0–1) |
| 5 | `debris_thick` | Mean debris thickness per band (m) |
| 6 | `debris_mf` | Melt reduction factor (0–1; 1 = no debris) |
| 7 | `debris_ponddens` | Supraglacial pond and ice-cliff density per band (0–1) |

### `factor_<RGI_ID>.dat` — Östrem-curve lookup table

Provides the debris-thickness-dependent melt reduction factor (Östrem curve) for the glacier. Three columns:

| Column index | Variable | Description |
|-------------|----------|-------------|
| 1 | `debris_type_th` | Debris thickness levels (m) |
| 2 | `debris_type_red` | Corresponding melt reduction factors (0–1) |

This lookup table is used in `procedures/processing/calculatedebrismelt.pro` to scale sub-debris melt as a function of local debris thickness.
