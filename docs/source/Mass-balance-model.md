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

### Description
The below figure provides a schematic overview of GloGEM’s englacial temperature model, which calculates vertical temperature profiles per elevation band. 

![Schematic representation of the firn and ice temperature module implemented in GloGEM. The figure illustrates a flowline representation of an elevation band on the left, zooming in to provide a more detailed view of the computational grid and the heat fluxes considered in the model. The upper boundary temperatures are prescribed based on mean monthly surface air temperatures, while a geothermal heat flux is applied at the base.](images/GloGEM_firnicetemp_schematic.png)

Englacial temperature evolution is simulated through heat conduction, described by the following equation:

$$
\begin{equation}
\frac{\delta T_{i,m}}{\delta t} = \frac{1}{c_{h}\cdot\rho}\frac{\delta}{\delta z}\left(\kappa\frac{\delta T_{i,m}}{\delta z}\right)
\end{equation}
$$

where $c_{h}$ is the heat capacity of ice, $\kappa$ is the thermal conductivity and $\rho$ the firn/ice density. The temperature of the uppermost layer, $T_{i,m}$, is assumed to equal the monthly mean air temperature. To reduce computational, costs, the model's resolution decreases with depth where temperature fluctuations are assumed to be minimal: 1 m for the uppermost 10 m, 5 m for the next 50 m, and 20 m for the following 200 m. At the lower boundary, a geothermal heat flux is applied based on a gridded data product by Lucazeau (2019).

Refreezing and the associated latent heat transfer from percolating water follow the previously described scheme. However, before entering the ice, the water input is reduced, as ice is considered mostly impermeable. However, we assume that in heavily crevassed areas, permeability increases sufficiently to impact the englacial temperature, as fractures provide pathways for liquid water infiltration. Crevassed regions are approximated by calculating velocity gradients. In a first step, the horizontal velocity of ice, $u_i$, is calculated using the Shallow Ice Approximation:

$$
\begin{equation}
u_i = \frac{2A}{n+1} \left( \rho g \sin \theta_i \right)^n H_i^{n+1}
\end{equation}
$$

where $A$ is the flow rate factor, which depends on the ice temperature, and $n$ is the Glen's flow law exponent. The terms $\rho$ and $g$ denote the ice density and gravitational acceleration, respectively. The surface slope is given by $\sin \theta_i$, and $H_i$ represents the ice thickness. This formulation captures the deformation of ice under its own weight, driven by the surface slope and the thickness of the ice.

The velocity gradient $\nabla_{u}$ is computed using finite differences and subsequently normalized as follows:

$$
\begin{equation}
\nabla_{u,\text{normalized}} = \frac{\nabla_{u} - \min(\nabla_{u})}{\max(\nabla_{u}) - \min(\nabla_{u})}
\end{equation}
$$

Positive velocity gradients serve as a proxy for extension and, consequently, crevassing, which is then used to compute a permeability factor governing the amount of liquid water entering the ice. The step-by-step computations for Great Aletschgletscher are shown in this example figure, illustrating the progression from ice velocities to velocity gradients and ultimately to the derived permeability factor $P_{i}$. Note that the original factor, which ranges between 0 and 1, is halved, limiting the maximum permeability to 50%, as fully permeable ice is not expected.

![Glacier profiles displaying the computed outputs of the ice permeability model per elevation band for Great Aletschgletscher. From left to right: ice velocities, velocity gradients, and the resulting permeability Pice in percent.](images/perm_model_outputs.png)

### Inputs

| Name                          | Type            | Content  |  
|-------------------------------|-----------------|------------------------------------------------------|  
| `firn`                        | Numeric Array   | Represents the firn cover in each elevation band.  |  
| `sno`                         | Numeric Array   | Represents the snow cover or snow storage in each elevation band.  |  
| `tgs`                         | Numeric Array   | Contains surface air temperature.  |  
| `plg`                         | Numeric Array   | Precipitation array at each grid point.  |  
| `mel`                         | Numeric Array   | Ice melt array at each grid point (to be modified).  |  
| `fit_water`                   | Numeric Array   | Liquid water available from surface melt `mel` and precipitation `plg` at each grid point.  |  
| `firnice_maxdepth`            | Numeric         | Maximum depth for firn/ice profile.  |  
| `elev_firnicetemp`            | Numeric Array   | Stores englacial temperatures for predefined depths (2m, 10m, 50m, bedrock depth).  |  
| `firnice_profile_ind`         | Numeric Array   | Index array that specifies which elevation bands or grid points correspond to specific firn/ice profiles.  |  
| `firnice_profile`             | Numeric Array   | Array that specifies depths where firn/ice temperature profiles are being tracked.  |  
| `tran`                        | Numeric Array   | Time period of modeling.  |  
| `m`                           | Variable        | Current month.  |  
| `te_fit`                      | Numeric Array   | Temporal array holding computed englacial temperatures.  |  
| `tl_fit`                      | Numeric Array   | Computed englacial temperatures.  |  
| `firnice_batch`               | Boolean         | Enables firnice batch mode (run for all glaciers in batch file).  |  
| `firnice_write`               | String Array    | Contains flags to enable writing firn/ice data to output.  |  
| `Lh_rf`                       | Physical Constant | Latent heat of fusion per unit volume [J m-3].  |  
| `rf_dt`                       | Variable        | Time step [in seconds].  |  
| `rf_dsc`                      | Variable        | Scaling factor that increases the temporal resolution of the refreezing model.  |  
| `slope`                       | Numeric Array   | Surface slope of the glacier per elevation band.  |  
| `thick`                       | Numeric Array   | Ice thickness per elevation band.  |  
| `ye`                          | Integer         | Year index for `imelt` storage.  |  
| `kice`                        | Physical Constant | Thermal conductivity of ice.  |  
| `kair`                        | Physical Constant | Thermal conductivity of air.  |  
| `cice`                        | Physical Constant | Heat capacity of ice.  |  
| `cair`                        | Physical Constant | Heat capacity of air.  |  
| `gl`                          | Numeric Array   | Array of size `nb` (number of elevation bands) tracking nodata values.  |  
| `geothermal_flux`             | Numeric Array   | Geothermal heat flux applied at the base of the glacier (one value per glacier).  |  
| `ice_permeability`            | Boolean         | Enables/disables percolation of water into the ice & reduction of water input based on the velocity gradient model.  |  
| `firn_permeability`           | Boolean         | Enables/disables percolation of water into the firn (sets `fit_water` to 0 if `y`).  |  
| `fit_layers`                  | Numeric Array   | Number of vertical layers with given thickness set to [10, 10, 10].  |  
| `fit_dz`                      | Numeric Array   | 2D array that stores the thickness of each layer.  |  
| `fit_dens`                    | Numeric Array   | Reference density profile, shifted depending on snow/firn situation.  |  

### Outputs
- **Englacial temperatures at defined depths (2m, 10m, 50m, bedrock)**  
  - `elev_firnicetemp(0)` : englacial temperature record for all elevation bands of a specific glacier at 2m depth
  - `elev_firnicetemp(1)` : englacial temperature record for all elevation bands of a specific glacier at 10m depth
  - `elev_firnicetemp(2)` : englacial temperature record for all elevation bands of a specific glacier at 50m depth
  - `elev_firnicetemp(3)` : englacial temperature record for all elevation bands of a specific glacier at the bedrock
- **Englacial temperature profiles**  
  - `firnice_profile_ind(0,j)`: englacial temperature profile per elevation band for all dephts within the vertical computational grid.

## Adapting snow reservoir

## Catchment discharge

## Adapting surface types

## Storing day variables
