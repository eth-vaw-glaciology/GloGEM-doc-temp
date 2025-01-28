# Writing procedures

## Write hypsometry-evolution 

_Not yet a procedure. Find the lines of code [here](https://docs.google.com/document/d/11gXNjeMalXpsZxf1M3aUowcWwOAnUfIh9J5BeRxEh3s/edit?tab=t.0)_

**Description**

This procedure writes out the hypsometry per glacier (per elevation band) into three separate files: The hypsometry itself `hypso_RGIid.dat`, the glacier volume `volume_RGIid.dat` and the cummulative temperature at the glacier surface `temp_RGIid.dat`.

**Inputs**

| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `nb` | variable | Number of elevation bands per glacier |
| `bed_elev` | variable | Bedrock elevation of the elevation band |
| `thick_ini` | variable | Initial ice thickness of the elevation band |
| `hypso_file` | 3D array | 3 dimensional array to temporarily store all the hypsometry data (elev, area, vol & cummulative temperatures) |
| `chypso` | variable | Iterator, iterating over the number of years of the modelling period |

File units:
| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `9` | unit | Area |
| `34` | unit | Volume |
| `35` | unit | Cummulative temperature |

**Outputs**

| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `hypso_RGIid.dat` | DAT-file | Stores the hypsometry per glacier (area per glacier band) |
| `volume_RGIid.dat` | DAT-file | Stores the volume per glacier band |
| `temp_RGIid.dat` | DAT-file | Stores the cummulative temperatures per glacier band |

The output is stored in the `/hypsometry` folder within your defined output directory.

## Write calibration file

_Not yet a procedure. Find the lines of code [here](https://docs.google.com/document/d/11gXNjeMalXpsZxf1M3aUowcWwOAnUfIh9J5BeRxEh3s/edit?tab=t.p4v7dsrpyr1f)_

**Description**

This writing procedure is only executed when you are in calibration mode. The procedure writes out 3 different calibration files containing the results of the calibration procedures.

**Inputs**

| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `flag` | variable | Bolean, 0 before calibration and 1 after successful calibration |
| `meltmodel` | variable | Defining the meltmodel 1 (Degree-day Model), 2 (Hock Model 2003) 3 (Oerlemans Model 2001) |
| `id` | array | Containing all RGI IDs of the glaciers that are being modelled |
| `gg` | array | Containing all glaciers within a specific grid cell |
| `g` | iterator | Iterator, iterating over all glaciers within a grid cell (gg) |
| `mb` | array | Containing the annual mass balance per glacier (integrated over entire glacier) |
| `wb` | array | Containing the winter mass balance per glacier (integrated over entire glacier) |
| `area1` | variable | Containing the total area of a glacier |
| `ela` | array | Containing the annual equilibrium line altitude (ELA) per glacier |
| `dbdz` | array | Containing the annual mass balance gradient values per glacier |
| `btongue` | array | Containing the minimum annual mass balance per glacier (glacier tongue) |
| `DDFsnow` | calibration parameter | Degree-day factor of snow |
| `DDFice` | calibration parameter | Degree-day factor of ice |
| `c_prec` | calibration parameter | Precipitation correction factor |
| `t_offset` | calibration parameter | Temperature offset per grid cell |
| `C0` | calibration parameter | Parameter $C_{0}$ of the Oerlemans Model |
| `C1` | calibration parameter | Parameter $C_{1}$ of the Oerlemans Model |
| `alb_ice` | calibration parameter | Albedo of ice $\alpha_{ice}$ |
| `alb_snow` | calibration parameter | Albedo of snow $\alpha_{snow}$ |
| `calimb_p0` | array | Array containing the starting years of the calibration period per glacier |
| `calimb_p1` | array | Array containing the end years of the calibration period per glacier |
| `ccj` | iterator | Iterator used to select the `calimb_p0` and `calimb_p1` per glacier |
| `target_spec` | variable | Target mass balance value of the calibration per glacier |
| `pp` | array | Containing all the years of the defined calibration period |
| `gx` | iterator | To scan over x dimension of the grid of the region |
| `gy` | iterator | To scan over y dimension of the grid of the region |

File units:
| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `3` | unit | For all calibration parameters & general charactersitics of the glacier |
| `4` | unit | For the temperature offset of each grid cell in the region |
| `50` | unit | For the file containing glacier specific calibration performance results |

**Outputs**

There are three different output files created in this procedure, they are structured as follows:

| Component              | Description                                                        | Example                     |
|------------------------|--------------------------------------------------------------------|-----------------------------|
| `calibrate`/`toff`     | Prefix indicating content of file (calibration/temperature offset) | `calibrate`                 |
| `[Meltmodel]`          | Placeholder for the meltmodel used                                 | `m1` or `m2`                |
| `cID`                  | Separator and identifier prefix for calibration period ID          | `cID`                       |
| `[CalibrationPeriodId]`| Placeholder for the specific calibration period ID                 | `8` or `9`                  |
| `[RGIRegion]`          | Placeholder for the RGI (Randolph Glacier Inventory) region number | `centraleurope`             |
| `[Subset]`             | Placeholder for the specific catchment or glacier identifier       | `Alps_g5km2`                |
| `[overview_glspec]`    | Suffix used for calibration performance file                       | `overview_glspec`           |

**Example filenames**:  
`calibrate_m1_cID9_centraleurope_Alps_g5km2.dat`   -> _containing the calibration parameters & general characteristics_.  
`calibrate_m1_cID9_Alps_g5km2_overview_glspec.dat` -> _created since model allows glacier specific calibration_.  
`toff_m1_cID9_centraleurope_Alps_g5km2.dat`        -> _containing temperature offset per grid cell._.  

## Write results files

**Description**
Procedure that writes the model results for all desired output variables.

**Inputs**

| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `outf_names` | list | List of strings containing the names of all the files you want to create |
| `areas` | array | Containing the annual value of the glacier area |
| `volumes` | array | Annual value of the glacier volume |
| `mb` | array | Annual mean specific mass balance of the glacier |
| `wb` | array | Annual mean specific winter balance of the glacier |
| `smelt` | array | Mean annual surface snow melt over the glacier |
| `imelt` | array | Mean annual surface ice melt over the glacier |
| `accum` | array | Mean annual surface accumulation of the glacier |
| `rain` | array | Mean annual amount of rain over the glacier |
| `ela` | array | Annual equilibrium line altitude |
| `aar` | array | Annual accumulation area ratio |
| `refre` | array | Mean annual amount of refrozen liquid water |
| `hmin_g` | array | Annual lowest elevation of the glacier |
| `flux_calv` | array | Mean annual calving flux of the glacier |
| `discharge` | array | Total (daily/monthly) discharge of the initial glaciated area |
| `discharge_gl` | array | Total (daily/monthly) discharge of the glacier |
| `accday` | array | Daily cummulative accumulation |
| `rainday` | array | Daily cummulative rain |
| `snowmeltday` | array | Daily cummulative snowmelt |
| `icemeltday` | array | Daily cummulative icemelt |
| `refrday` | array | Daily cummulative refrozen liquid water |
| `snowlineday` | array | Daily snowline altitude |
| `id` | array | Containing all RGI IDs of the glaciers that are being modelled |
| `gg` | array | Containing all glaciers within a specific grid cell |
| `g` | iterator | Iterator, iterating over all glaciers within a grid cell (gg) |
| `years` | variable | Total number of years of the modelling period (e.g. 30 years) |
| `y` | array | Containing years of the current model run (`tran(0)`+`years` -> 1980-2000) |
| `time_resolution` | string | String that defines differentiates between "daily" & "monthly" |
| `format_of` | array | Specifies the format for each column of data when reading or writing data. E.g. `f10.3` floating point number with 10 characters and 3 decimals |

**Outputs**
 
All the input variables are written out as .dat output files. The filenames have been defined in `outf_names`.

**Example filenames**:  
`Areas.dat`.  
`Volumes.dat`.  
...

Note that files ending with `*_day.dat` store daily data. Future versions will have also files ending with `*_mon.dat` for monthly data.

## Write elevation band file

**Description**
Procedure that writes out model results for each elevation band. These files are needed for GloGEMflow (Zekollari et al. 2019). The debris module was and resulting files were added by Compagno et al. 2022.

**Inputs**

| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `dir_data` | string | Predefined directory of the ice thickness file |
| `region` | string | RGI region |
| `id` | array | Containing all RGI IDs of the glaciers that are being modelled |
| `gg` | array | Containing all glaciers within a specific grid cell |
| `g` | iterator | Iterator, iterating over all glaciers within a grid cell (gg) |
| `thick_ini` | array | Initial ice thickness per elevation band |
| `elev_bmb` | array | Annual mass balance per elevation band |
| `elev_bwb` | array | Annual winter balance per elevation band |
| `elev_refr` | array | Annual refreezing per elevation band |
| `elev_debthick` | array | Annual debris thickness per elevation band |
| `elev_debfrac` | array | Annual debris fraction per elevation band |
| `elev_debfactor` | array | Annual debris factor per elevation band |
| `elev_pondarea` | array | Annual area of subraglacial lakes per elevation band |
| `elev_mbsens` | array | Annual mass balance sensitivity per elevation band |
| `years` | variable | Total number of years of the modelling period (e.g. 30 years) |
| `elev0` | array | Initial elevation per elevation band |

File units:
| Name       | Type      | Content      | Corresponding directory |
|----------------|----------------|----------------|----------------|
| `8` | unit | Mass balance per elevation band | `*mb_elevation/belev.dat` |  
| `40` | unit | Refreezing per elevation band | `*refr_elevation/refrelev.dat` | 
| `41` | unit | Debris thickness per elevation band | `*debris_elevation/debthick.dat` | 
| `42` | unit | Fractional covering by debris per elevation band | `*debris_elevation/debfrac.dat` | 
| `43` | unit | Degree to which the debris is affecting the mass balance | `*debris_elevation/debfactor.dat` | 
| `44` | unit | Annual area of subraglacial lakes per elevation band | `*debris_elevation/pondarea.dat` | 
| `44` | unit | Mass balance sensitivity | `*debris_elevation/mbsensitivity.dat` | 

**Outputs**

All elevation band data is stored in separate .dat files. You can find an example filename and directory below.

**Example filenames (incl. directory)**:  
`/scratch_net/iceberg_second/mhuss/Alaska/debris_elevation/debfrac_5000.dat`

## Write firnice-temperature

**Description**
Procedure that writes out all the data generated by the firnice-temperature module. This module is optional.

**Inputs**

| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `elev_firnicetemp` | 3D array | 3 dimensional array storing the ice temperature at different depths (0=1m, 1=10m, 2=50m, 3=bedrock) |
| `firnice_profile` | array | Containing the monthly temperature profile of a elevation band |
| `elev0` | array | Initial elevation per elevation band |
| `years` | variable | Total number of years of the modelling period (e.g. 30 years) |

File units:
| Name | Type | Content                        |
|------|------|--------------------------------|
| `45` | unit |  1 m ice temperature           |  
| `46` | unit | 10 m ice temperature           | 
| `47` | unit | 50 m ice temperature           | 
| `48` | unit | Ice temperature at the bedrock | 
| `51` | unit | Ice temperature profile | 

**Outputs**

Outputs the ice temperature data for the modelled glaciers and saves stores it in 5 separat files stored in the `/firnice_temperature` directory: 

The first for files contain data for all elevation bands of a specific glacier, while the `/temp_ID`file stores the monthly ice temperature profile of a given elevation band (stemming from the `firnicetemperature_batch.dat`) refering to icetemperature borehole data from the glenglat database [(Jacquemart & Welty, 2025)](https://doi.org/10.5194/essd-2024-249)

1. `/temp_1m_` Stores the monthly ice temperature at 1 m depth
2. `/temp_10m_` Stores the monthly ice temperature at 10 m depth
3. `/temp_50m_` Stores the monthly ice temperature at 50 m depth
4. `/temp_bedrock_` Stores the monthly ice temperature at the bedrock
5. `/temp_ID` Stores the monthly temperature profile for a certain elevation band usually referring to a glenglat borehole ID

## Write main file and meteo file

**Description**

Procedure that writes out the meteo file per glacier containing all the input meteo data that has been used. This procedure has never been used and might be removed in future versions.

**Inputs**

| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `volume0` | variable | Initial glacier volume |
| `area_iniconst` | array | Initial area of each elevation band for a specific glacier (not affected by glacier advance) |
| `area1` | variable | Total final area of a specific glacier after advance or retreat |
| `id` | array | Containing all RGI IDs of the glaciers that are being modelled |
| `gg` | array | Containing all glaciers within a specific grid cell |
| `g` | iterator | Iterator, iterating over all glaciers within a grid cell (gg) |
| `latitudes` | array | Containing the latitudes of all the glaciers in a specific grid cell |
| `longitudes` | array | Containing the longitudes of all the glaciers in a specific grid cell |

**Outputs**

Outputs a file containing volume and area differences for a each glacier that has been modelled. THe data is stored in a directory following this structure:

`dirres+dir_region+subpath+long_GCM+sub_region+cc+'.dat`

_Note that this writing procedure might be deleted in future versions at it is not regulary used._

## Write calibration phase statistics file

**Description**

This procedure is executed during calibration mode. After a model run is finished, it writes out several files containing the calibration phase statistics. These files may include information on parameter ranges needed for convergence (e.g., the ranges of `c_prec`, the precipitation factor in the mass balance model).

_This procedure needs to be recoded as it doesn't use units and thus requires all directories, such as `dir_region`, to be passed as arguments. This change will be implemented in future versions. Therefore, it will be documented once this work has been completed._

**Inputs**

**Outputs**

## Write file for volume below sea-level

**Description**

Procedure that writes out a file containing the volume below sea level per glacier. This is mostly important fore tidewater glaciers. 

**Inputs**

| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `tran` | array | Containing the start and end year of the modelling period e.g. `[1980,2030]` |
| `years` | variable | Total number of years of the modelling period (e.g. 30 years) |
| `vol_bz` | array | Containing the annual volume below sea level per glacier |

File units:
| Name | Type | Content                            |
|------|------|------------------------------------|
| `7`  | unit | Volume below sea-level in $km^{3}$ |  
| `33` | unit | Frontal ablation in $Gt/a$         |

_Currently this procedure only writes out the volume below sea-level but it also closes the unit 33 and could thus potentially also save the calving flux in Gt/a. To be validated._

**Outputs**

Outputs a file containing the volume below sea-level per glacier (important for tidewater glacier). The data is stored in a directory following this structure:

`dirres+dir_region+subpath+long_GCM+sub_region+cc+'_SLE_volbz.dat`

**Example filename**:  

`/scratch_net/vierzack05/jabeer/GloGEM_output/r6_daily/CentralEurope/files/SINGLE/centraleurope_SLE_volbz.dat`

# Zipping files

**Description**

This procedure is only executed in case the user wants to write out the annual hypsometry for all glaciers (`write_hypsometry_files` is set to `y`). It zips the resulting `/hypsometry` folder to store disc space.

**Inputs**

| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `meltmodel` | variable | Defining the meltmodel 1 (Degree-day Model), 2 (Hock Model 2003) 3 (Oerlemans Model 2001) |
| `reanalysis_direct` | string | Boolean string defining whether the reanalysis directory is `/PAST` being used, can take the value `y` or `n` |
| `dirres` | string | Your predefined output directory string (currently defined in `input.pro`) |
| `dir_region` | string | Folder for RGI region that is being modelled e.g. `CentralEurope` |
| `region` | string | Region directory string e.g. `lowlatitudes` |

**Outputs**

Outputs two files containing the zipped hypsometry data and will remove the `/hypsometry` folder to save space on disc.

**Example filename**:  
`*/hypsometry.zip`