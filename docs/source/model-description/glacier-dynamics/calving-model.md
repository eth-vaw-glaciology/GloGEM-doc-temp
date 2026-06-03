# Calving Model

```{note}
This section is under development.
```

The calving model computes frontal ablation for marine-terminating glaciers. It is implemented in `procedures/processing/calving_model.pro` and activated with `frontal_ablation = 'y'`.

## Description

GloGEM follows the calving parameterisation of Oerlemans and Nick (2005), where calving flux depends on water depth at the terminus and glacier velocity.

## Settings

| Setting | Default | Description |
|---------|---------|-------------|
| `frontal_ablation` | `'y'` | Activate frontal ablation for marine-terminating glaciers |
| `alpha_f` | 0.7 | Calving parameter (read from regional file) |
| `calv_sep` | 1.25 | Threshold [m w.e.] separating calving flux into geometry change vs. direct break-off |
| `c_calving` | 2.4 | Calving parameter (read from regional file) |
| `length_corrfact` | 1.4 | Correction factor for glacier length |

## Inputs

```{note}
Inputs and outputs table to be completed.
```

## Outputs

```{note}
Inputs and outputs table to be completed.
```
