# Climate Data

GloGEM requires gridded near-surface air temperature and precipitation for both the historical (reanalysis) and future (GCM) periods. All climate files are stored on the VAW shared storage under:

```
/itet-stor/<username>/glogem/climatedata/
```

---

## Reanalysis data (historical forcing)

Reanalysis data provides the reference climate for the calibration period and is used as the basis for bias-correcting GCM output.

### Storage path

```
climatedata/reanalysis/<time_resolution>/<reanalysis>/<region>/
```

| File | Contents |
|------|----------|
| `clim_<region>.mdi` | Gridded monthly (or daily) temperature and precipitation fields |
| `variability_<region>.mdi` | Daily within-month temperature anomaly patterns (required when `submonth_variability = 'y'`) |
| `tgrad_<region>.mdi` | Monthly vertical temperature gradient fields |

Each file is a custom binary format with a plain-text header (`.mdi`) followed by the data arrays. The header contains grid dimensions, time vector, and coordinate arrays.

### Available reanalysis products

| Setting value | Product | Time resolution |
|---------------|---------|----------------|
| `era5` | ERA5 (ECMWF) | Daily (default) |
| `ERA5` | ERA5 (ECMWF) | Monthly (auto-selected) |
| `era5land` | ERA5-Land | Daily |
| `chelsaw5e5` | CHELSA-W5E5 | Daily |
| `ch2018` | CH2018 | Daily |
| `gswp3w5e5` | GSWP3-W5E5 | Daily |

The active product is set with `reanalysis` in `config.pro` or `settings.pro`.

### Variables

| Variable | Description |
|----------|-------------|
| `rtemp` | Air temperature per grid cell per time step (°C) |
| `rprec` | Precipitation per grid cell per time step (mm d⁻¹) |
| `relev` | Grid cell reference elevation (m a.s.l.) |
| `rtg` | Monthly vertical temperature gradient (°C m⁻¹) |
| `rvariab` | Daily temperature anomaly patterns within each month |

---

## GCM data (future forcing)

GCM projections are bias-corrected against the reanalysis reference and used for future simulations.

### Storage path

```
climatedata/future/<time_resolution>/<GCM_data>/<region>/<GCM_model>/
    <GCM_model>_<SSP>_<experiment>_<region>.mdi
```

For extended runs to 2300, `long_` is prepended to `<GCM_data>`:

```
climatedata/future/<time_resolution>/long_<GCM_data>/<region>/...
```

### Available CMIP6 models

| Model |
|-------|
| BCC-CSM2-MR |
| CAMS-CSM1-0 |
| CESM2 |
| CESM2-WACCM |
| EC-Earth3 |
| EC-Earth3-Veg |
| FGOALS-f3-L |
| GFDL-ESM4 |
| INM-CM4-8 |
| INM-CM5-0 |
| MPI-ESM1-2-HR |
| MRI-ESM2-0 |
| NorESM2-MM |

### Available SSPs

`ssp119`, `ssp126`, `ssp245`, `ssp370`, `ssp534-over`, `ssp585`

### Variables

| Variable | Description |
|----------|-------------|
| `gcm_temp` | GCM air temperature (°C) |
| `gcm_prec` | GCM precipitation (mm d⁻¹) |
| `gcm_elev` | GCM grid cell reference elevation (m a.s.l.) |

---

## Bias correction

Raw GCM output is systematically offset from the reanalysis reference. GloGEM corrects these biases before applying GCM data to the glacier model. Bias correction is implemented in `procedures/processing/bias_correction_delta.pro` and `bias_correction_qm.pro`, and is activated with `bias_correction = 'y'`.

The correction is computed over an overlap period shared by both the reanalysis and the GCM historical run, set by `rea_eval` (default: 1991–2020).

### Method 1 — Delta method (default)

Selected with `bias_correction_method = 1`. For each calendar month, additive temperature and multiplicative precipitation biases are computed from the overlap period:

$$
\Delta T_m = \overline{T_{\text{GCM},m}} - \overline{T_{\text{rea},m}}
$$

$$
f_{P,m} = \frac{\overline{P_{\text{GCM},m}}}{\overline{P_{\text{rea},m}}}
$$

These monthly offsets are then applied to the full GCM time series. A temperature variability scaling factor is also computed:

$$
\sigma_m = \frac{\sigma(T_{\text{rea},m})}{\sigma(T_{\text{GCM},m})}
$$

To prevent unrealistic corrections in high-latitude or data-sparse regions, extreme biases can be clamped via `min_tempbias` and `min_precbias`.

### Method 2 — Quantile mapping

Selected with `bias_correction_method = 2`. Available for daily temperature only. Builds empirical cumulative distribution functions (CDFs) at 1001 quantile levels for:

- Reanalysis historical period
- GCM historical overlap period
- GCM future period

The transfer function maps each GCM future quantile to the corresponding reanalysis quantile, preserving the full distribution shape rather than only correcting the mean.

### Settings

| Setting | Default | Description |
|---------|---------|-------------|
| `bias_correction` | `'y'` | Apply bias correction to GCM data |
| `bias_correction_method` | 1 | Bias correction method: 1 = delta, 2 = quantile mapping |
| `rea_eval` | `[1991, 2020]` | Reference period (years) for bias computation |
| `min_tempbias` | — | Minimum allowed temperature bias (°C); clamps extreme high-latitude corrections |
| `min_precbias` | — | Minimum allowed precipitation bias factor |
