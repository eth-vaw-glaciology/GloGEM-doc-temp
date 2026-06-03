# Glacier Retreat Model

```{note}
This section is under development.
```

The glacier retreat model updates glacier geometry (area and hypsometry) annually based on the computed mass balance. It is implemented in `procedures/processing/glacier_retreat.pro` and activated with `glacier_retreat = 'y'`.

## Description

GloGEM uses an empirical $\Delta h$-parameterisation (Huss et al., 2010) to redistribute ice thickness changes across elevation bands. The retreat scheme distinguishes between small and large glaciers via `dh_size` thresholds.

## Settings

| Setting | Default | Description |
|---------|---------|-------------|
| `glacier_retreat` | `'y'` | Activate glacier geometry change |
| `expon` | 2.0 | Valley shape exponent controlling area loss per band |
| `dh_size` | `[5, 20]` | Area thresholds [km²] between $\Delta h$ parameterisations |
| `redistribute_vplus` | `'y'` | Redistribute extreme elevation changes at the tongue |
| `advance` | `'y'` | Allow glacier advance (use with care — can cause instabilities) |

## Inputs

```{note}
Inputs and outputs table to be completed.
```

## Outputs

```{note}
Inputs and outputs table to be completed.
```
