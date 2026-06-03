# Melt Models

GloGEM offers two surface melt models selected via the `meltmodel` setting in `settings.pro`. Both are implemented in `procedures/processing/meltmodel.pro`.

## Surface type

Melt rates depend on surface type, tracked per elevation band:

| Code | Surface | Used for |
|------|---------|---------|
| `0` | Bare ice | `DDFice` or ice albedo |
| `1` | Snow | `DDFsnow` or snow albedo |
| `2` | Firn | `DDFfirn` or firn albedo |

---

## Model 1 — Temperature-index (degree-day) model

Selected with `meltmodel = '1'`. This is the default and computationally lightest option.

Melt for each elevation band is proportional to positive air temperature:

$$
M = \text{DDF}_{\text{surface}} \cdot T^+ \cdot \Delta t / 1000
$$

where $T^+$ is temperature above the melt threshold `T_melt` [°C], $\Delta t$ is the number of days in the time step, and the result is in m w.e.

Degree-day factors by surface type:

| Surface | DDF |
|---------|-----|
| Snow | `DDFsnow` |
| Firn | `(DDFsnow + DDFice) / 2` |
| Ice | `DDFice` |

`DDFsnow` and `DDFice` are calibration parameters; `DDFice` is constrained to equal `2 × DDFsnow` during calibration Phase 2 (default).

### Sub-monthly variability

For the monthly model, observed sub-monthly temperature variability can be superimposed onto the monthly mean temperature (setting `submonth_variability = 'y'`). This generates a synthetic daily temperature distribution for each month, from which positive degree days are accumulated. The mean of the distribution is preserved so no bias is introduced into the monthly mean.

---

## Model 3 — Simplified energy-balance model

Selected with `meltmodel = '3'`. Based on Oerlemans (2001). Requires potential solar radiation (see [Solar Radiation](../solar-radiation.md)).

Melt is driven by net shortwave radiation and a linear temperature term:

$$
M = \frac{\left[(1 - \alpha) \cdot SW + C_0 + C_1 \cdot T\right] \cdot 3600 \cdot 24 \cdot \Delta t}{1000 \cdot L_f}
$$

where:
- $\alpha$ is the surface albedo (snow, firn, or ice)
- $SW$ is the potential shortwave radiation [W m⁻²]
- $C_0$ is a constant offset term [W m⁻²]
- $C_1$ is the temperature sensitivity coefficient [W m⁻² °C⁻¹]
- $L_f$ is the latent heat of fusion [J kg⁻¹]
- $\Delta t$ is the number of days in the time step

Negative melt values are set to zero.

### Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `C0` | −45 | Constant energy-balance offset [W m⁻²] (calibrated) |
| `C1` | 10 | Temperature sensitivity [W m⁻² °C⁻¹] (calibrated) |
| `alb_snow` | 0.7 | Albedo of snow |
| `alb_firn` | 0.5 | Albedo of firn |
| `alb_ice` | 0.3 | Albedo of bare ice |

---

## Debris-covered ice

When `debris_supraglacial = 'y'`, melt of debris-covered ice is modified in `procedures/processing/calculatedebrismelt.pro`. A reduction factor is applied based on debris thickness and fraction, and melt over pond/ice-cliff areas is enhanced by `debris_pond_enhancementfactor`. See [Debris Model](../debris-model.md) for details.

---

## Parameters (Model 1)

| Parameter | Default | Description |
|-----------|---------|-------------|
| `meltmodel` | `'1'` | Melt model selection: `'1'` = DDF, `'3'` = energy balance |
| `DDFsnow` | 3 | Degree-day factor for snow [mm d⁻¹ °C⁻¹] (calibrated) |
| `DDFice` | 6 | Degree-day factor for ice [mm d⁻¹ °C⁻¹] (calibrated) |
| `T_melt` | 0 | Temperature threshold for melt [°C] |
| `submonth_variability` | `'y'` | Include sub-monthly temperature variability (monthly model only) |

## Outputs

| Variable | Description |
|----------|-------------|
| `mel` | Melt per elevation band per time step [m w.e.] |
| `smelt[ye]` | Area-averaged annual snowmelt [m w.e.] |
| `imelt[ye]` | Area-averaged annual ice/firn melt [m w.e.] |
| `snowmel` | Daily snowmelt per band (daily model only) |
| `icemel` | Daily ice/firn melt per band (daily model only) |
