```{note}
This part of the documentation is still under development.
```
# Mass balance

## Accumulation

## Melt

## Debris: CALCULATEDEBRISMELT.pro

### Description 
This procedure models the effect of supraglacial debris on glacier melt. If the setting `debris_supraglacial` is set to `"y"`, the procedure calculates the melt reduction due to debris cover by reading the debris thickness and debris fraction. It identifies debris-covered ice where the surface type is `"ice"`, the temperature exceeds the melting threshold, and debris exists.  

The procedure then assigns a debris reduction factor based on predefined debris thickness categories and adjusts melt rates accordingly. It accounts for:
- **Debris-covered ice** (reduced melt based on `debris_type_red`)  
- **Bare ice** (melting normally)  
- **Ponded/cliff areas** (enhanced melting based on `debris_pond_enhancementfactor`)  

The melt calculations are summed and stored in `imelt(ye)`, normalized by the glacier area. If the `time_resolution` setting is `"daily"`, the computed melt values are stored in `icemel(ii)`.  

### Inputs 
| Name                          | Type       | Content  |  
|-------------------------------|-----------|------------------------------------------------------|  
| `debris_supraglacial`         | String    | Flag to enable/disable debris influence on melt (`"y"` or `"n"`).  Setting |  
| `sur`                         | Numeric Array | Surface classification array (0 = ice, other values for snow/firn).  |  
| `tg`                          | Numeric Array | Temperature array at each grid cell.  |  
| `t_melt`                      | Numeric    | Temperature threshold for melting.  |  
| `debris_thick`                | Numeric Array | Debris thickness at each grid point.  |  
| `debris_frac`                 | Numeric Array | Fraction of surface covered by debris.  |  
| `debris_type_th`              | Numeric Array | Reference debris thickness values for classification.  |  
| `debris_type_red`             | Numeric Array | Reduction factors corresponding to `debris_type_th`.  |  
| `debris_ponddens`             | Numeric Array | Fraction of surface covered by melt ponds or cliffs.  |  
| `debris_pond_enhancementfactor` | Numeric | Enhancement factor for pond/cliff melt.  |  
| `mel`                         | Numeric Array | Ice melt array at each grid point (to be modified).  |  
| `imelt`                       | Numeric Array | Integrated melt accumulation per year.  |  
| `ye`                          | Integer    | Year index for `imelt` storage.  |  
| `ar_gl`                       | Numeric    | Total glacier area (for normalization).  |  
| `area`                        | Numeric Array | Area of each grid cell.  |  
| `time_resolution`             | String    | Temporal resolution of the model (`"daily"` or other). Setting |  
| `icemel`                      | Numeric Array | Stores computed melt values if `time_resolution` is `"daily"`.  |  
| `write_mb_elevationbands`     | String    | Flag to store debris reduction factors for mass balance calculations (`"y"` or `"n"`). Setting  |  

### Outputs 
- **Melt calculations adjusted for debris cover**  
  - `imelt(ye)`: Updated cumulative melt array for the given year, normalized by glacier area.  
  - `icemel(ii)`: Daily melt values if `time_resolution` is `"daily"`.  
- **Debris reduction factors (optional)**  
  - `debris_red_factor(ii)`: Stores assigned reduction factors for mass balance elevation bands if `write_mb_elevationbands` is `"y"`.  

### Loop
Within loop 14, meltmodel '1'

## Refreezing

## Firnicetemperature

Firnice temperatures of each layer within GloGEM are computed on the basis of heat conduction

$$
\frac{\delta T_{i,m}}{\delta t} = \frac{1}{c_{h}\cdot\rho}\frac{\delta}{\delta z}\left(\kappa\frac{\delta T_{i,m}}{\delta z}\right)
$$

## Adapting snow reservoir

## Catchment discharge

## Adapting surface types

## Storing day variables
