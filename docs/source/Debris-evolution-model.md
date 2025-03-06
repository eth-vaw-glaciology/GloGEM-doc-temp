# Debris evolution model: DEBRIS_MODEL.pro 

## Description
The `DEBRIS_MODEL` procedure simulates debris-covered glacier evolution over time by considering factors such as equilibrium line altitude (ELA), debris expansion, debris thickening, and pond formation. It models debris expansion across glacier surfaces based on elevation bands and historical mass balance trends. The model also accounts for the dynamic response of debris thickness and pond density over time.

The debris melt calculation is performed in a separate procedure `CALCULATEDEBRISMELT`, which is within Loop 14, in the mass balance calculation. The data this is based on is read in in the READ_SUPRAGLACIALDEBRIS procedure in loop 11. 

## Inputs
| Name                     | Type    | Description |
|--------------------------|---------|-------------|
| `ye`                     | Integer | Current year of the simulation. |
| `nb`                     | Integer | Number of elevation bands. |
| `step`                   | Float   | Elevation step size between bands. |
| `gl`                     | Array   | Glacier outline mask. |
| `noval`                  | Float   | Value representing no-data points. |
| `area`                   | Array   | Glacier area in each elevation band. |
| `ar_gl`                  | Float   | Glacier area fraction. |
| `ela`                    | Array   | Equilibrium Line Altitude over time. |
| `bal`                    | Array   | Mass balance for each elevation band. |
| `mb`                     | Array   | Mass balance over past years. |
| `elev`                   | Array   | Elevation values of bands. |
| `debris_expansion`       | String  | Flag ('y' or 'n') to enable debris expansion. Setting |
| `debris_seed_bands`      | Integer | Number of bands where debris seeding is possible. |
| `debris_seed_meters`     | Float   | Annual increase in elevation range where debris seeding occurs. |
| `debris_thickening`      | String  | Flag ('y' or 'n') to enable debris thickening. Setting |
| `debris_frac`            | Array   | Fraction of area covered by debris in each band. |
| `debris_thick`           | Array   | Debris thickness in each band. |
| `debris_ponddens`        | Array   | Density of ponds in each band. |
| `debris_pond_gradient`   | Float   | Gradient controlling pond expansion rate. |
| `debris_ponddens_max`    | Float   | Maximum allowable pond density. |
| `tran`                   | Array   | Transition periods. |
| `survey_year`            | Array   | Years of available glacier surveys. |
| `write_mb_elevationbands`| String  | Flag ('y' or 'n') to enable output writing. Setting |
| `debris_exp_gradient`    | Float   | Gradient controlling debris expansion rate. |
| `debris_initialband`     | Float   | Initial debris thickness for newly seeded bands. |
| `debris_red_factor`      | Array   | Reduction factor for debris-covered ice melt. |
| `debris_thick0`          | Array   | Initial debris thickness reference. |
| `elev_debthick`         | Array   | Output array for debris thickness over time. |
| `elev_debfrac`          | Array   | Output array for debris fraction over time. |
| `elev_debfactor`        | Array   | Output array for debris reduction factors. |
| `elev_pondarea`         | Array   | Output array for pond-covered area. |
| `g`                      | Array   |  |
| `gg`                     | Array   |  |

## Outputs
If `write_mb_elevationbands` is enabled, the following outputs are generated for each year:

- `elev_debthick(ye, i)`: Stores debris thickness at each elevation band.
- `elev_debfrac(ye, i)`: Stores the fraction of debris coverage.
- `elev_debfactor(ye, i)`: Stores debris melt reduction factor.
- `elev_pondarea(ye, i)`: Stores the total pond-covered area at each elevation band.

## Process Overview

1. **Equilibrium Line Altitude (ELA) Calculation:**
   - If the simulation is in its early years, ELA is determined based on the glacier area fraction.
   - ELA is computed as a decadal mean.

2. **Debris Expansion:**
   - Debris-covered glacier zones are extended based on mass balance trends.
   - New debris regions are seeded based on adjacent elevation bands.

3. **Pond Formation and Expansion:**
   - The model simulates pond expansion by redistributing pond densities.
   - Ponds can seed new bands under certain conditions.

4. **Debris Thickening:**
   - If enabled, debris thickness increases over time based on mass balance and elevation band properties.

5. **Output Storage:**
   - Relevant values for debris fraction, thickness, and pond coverage are saved for each year.

## Loop 
- after Loop 13 (hydrological years) has closed. 

