# Potential Solar Radiation

```{note}
This section is under development.
```

Potential solar radiation is required for melt model 3 (simplified energy balance). It is computed in `procedures/processing/potential_solarradiation.pro` when `meltmodel = '3'` is set. This procedure can be considered as a preprossessing step.


## Description

The procedure computes clear-sky potential shortwave radiation for each elevation band as a function of:

- Geographic location (latitude, longitude)
- Elevation
- Slope and aspect of the glacier surface
- Solar declination (computed for the mid-point of each month)

The result is a monthly radiation field `sw_rad[nb, 12]` (W m⁻²) used in the energy-balance melt calculation.

## Settings

| Setting | Description |
|---------|-------------|
| `meltmodel = '3'` | Activates the solar radiation calculation |
| `decl_sun` | Solar declination for each month (mid-month) — defined in `settings.pro` |
| `asp_class` | Aspect classes [0, 45, 90, 135, 180°] |

## Inputs and outputs


sw_rad, which is a monthly radiation field. This field is a double-precision array with dimensions nb (number of elevation bands) by 12 (for each month of the year), and its values are in Watts per square meter (W m⁻²). It represents the clear-sky potential shortwave radiation for each elevation band.
| Variable | Description |
|---------|-------------|
| `sw_rad` | Represent the clear-sky potential shortwave radiation for each elevation band =>Monthly radiation field (W m⁻²); Array dimensions: number of elevation bands by 12 (for each month)|

