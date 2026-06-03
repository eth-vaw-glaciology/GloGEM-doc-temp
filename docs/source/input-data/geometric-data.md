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

One row per glacier, 19 columns including:

| Column | Variable | Description |
|--------|----------|-------------|
| 0–1 | `lon`, `lat` | Glacier centroid coordinates (°) |
| 2 | `a_gl` | Glacier area (km²) |
| 3 | `volume_ini` | Initial ice volume (km³) |
| 6 | `hmin` | Minimum surface elevation (m a.s.l.) |
| 8 | `hmed` | Median surface elevation (m a.s.l.) |
| 18 | `survey_year` | RGI inventory survey year |

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

| Column index | Variable | Description |
|-------------|----------|-------------|
| 0 | Surface elevation | Band mid-point elevation (m a.s.l.) |
| 1 | Ice thickness | Mean ice thickness in band (m) |
| 2 | Slope | Mean surface slope (°) |
| 3 | Area | Band area (km²) |
| 4–11 | Additional geometry | Width, length, bed elevation, and other band properties |

Elevation bands are spaced at 10 m vertical intervals. Bands are ordered from the lowest (terminus) to the highest (headwall) elevation.

---

## Regional parameter file

Region-specific calibration parameters (calving coefficient, precipitation correction, lapse rate, temperature offset) are stored in a single table per reanalysis product:

### Storage path

```
/itet-stor/<username>/glogem/data/regional_parameters_<reanalysis>.dat
```

One row per RGI region (and sub-region where applicable). Read by `procedures/read/read_regionalparams.pro` at the start of each regional run.

---

## Geothermal heat flux

Required only when the englacial temperature model is active (`firnice_temperature = 'y'`).

### Storage path

```
/itet-stor/<username>/glogem/data/geothermal_flux.grid
```

Global gridded geothermal heat flux in standard ESRI ASCII raster format. Values are stored in mW m⁻² and converted to W m⁻² on read.

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
