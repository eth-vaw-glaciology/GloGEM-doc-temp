# Reading procedures

## Read geothermal/read firnicebatch

**Description** \
If one wants to model ice or firn temperatures in GloGEM, these procedures read in the corresponding data. If in settings "firnice temperature" equals "y(es)", the geothermal flux is read in. The geothermal heat flux is a global dataset at 0.5° resolution (geothermal flux.grid), derived from (xxxxx). The procedure reads in the gridded data and assigns coordinates. In addition, there is the option to read in a list of glaciers for which ice temperature profiles are available (xx) through the read firnicebatch, if the settings "firnice batch" equals "y" is true. The read in of the ice temperature profiles occurs xxxxx

**Inputs** \
read geothermal.pro:
| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `dir` | path | the directory where the data is stored, defined in the "settings/input" |
| `firnice_geotherm_flux` | variable | here the geothermal flux will be stored |

read firnicebatch.pro:
| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `dir` | path | the directory where the data is stored, defined in the "settings/input" |
| `firnice_batch_data1` | variable (numeric) | here the metadata of the measured ice profiles are stored: Elevation, RGI-ID and max-depth  |
| `firnice_batch_data2` | variable (text) | here the metadata of the measured ice profiles are stored: RGI-ID, measurement ID |

**Outputs**\
See description at input

**"Loop"**\
This procedure is called outside any of the loops

***
## Read regionbatch
**Description**\
This file is read in to translate the RGI regions to the GloGEM regions (some regions are subdivided due to their size). The input file defines the GloGEM regions to be run and this file is used to translate it to the corresponding RGI regions, needed for reading in data. 

**Inputs**
| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `dir` | path | the directory where the data is stored|
| `region_loop_data` | variable | read in data is written here: table containing the translation between GloGEM and RGI regions |

**Outputs**

**"Loop"** \
Before loop "regions" - information needed for starting this loop

***
## Read regionalparams
**Description**\
Reading in of pre-defined regional parameters used for regional differentiating of mass balance processes. The file contains values for: 
* Region
* Sub-region (sub-division in GloGEM of large RGI regions)
* c_calving ([calving model according to Oerlemans and Nick, 2005](https://doi.org/10.3189/172756405781813023))
* Cprec0 (to match winter accumulation data)
* c1 tolerance (related to cprec)
* dPdz (precipitation gradient %/100m)
* t_off0 (start-value for temperature offset)
* t_offgrid (grid-based/iterative t off)
* P_threshold (minimum daily precipitation, mm/day)
* size_range (regional size range of glaciers to be evaluated).

 **Inputs**
| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `dir` | path | the directory where the data is stored|
| `dir_region` | path | the directory where the geodetic data is stored per RGI region |
| `reanalysis` | variable (setting) | Name of the chosen reanalysis dataset |
| `clim_subregion` | variable (setting) | Only relevant for when the selected GloGEM regio covers RGI region 10, 16, or 19 (these are subdivided) |
| `size_range_overwrite` | variable (setting) | setting that defines if the set size range of glaciers to be modelled in the regional parameter file should be overwritten by the manual settings |

**Outputs**
| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `c_calving` | variable | model parameter that xxxx |
| `c_prec` | variable | model parameter that xxxx |
| `c1_tolerance` | variable | model parameter indicating a range (min,max) related to cprec |
| `t_offset` | variable | model parameter that indicates the starting value for temperature offset |
| `toff_grid` | variable | model parameter that xxx |
| `toff_grid0` | variable | model parameter that xxx The same as toff grid? |
| `p_thres` | variable | model parameter that defines per region the threshold for characterizing the drizzle days, i.e. set the P to 0mm on those days |
| `size_range` | variable | model parameter that defines which glacier sizes are modelled - used? |

**"Loop"** \
Start (within) of loop regions

***
## Read geodetic
**Description**\
Here the geodetic mass balance data is read in, for which you define the source in the "input/settings". Currently, three options are available from [Hugonnet et al., 2021 (Nature)](https://www.nature.com/articles/s41586-021-03436-z): 1) 2000-2020, 2) 2000-2010, 3) 2010-2020. The data have been formatted in a table which contains: RGI-ID, Area, lat, lon, B (mass balance m. w. eq. using Rho = 850 kgm-2), errB, number indicating measured (1) or regional mean (2) taken. Next to the initial filtering (1/2), the code does a second filtering, detecting the glaciers that have a standard deviation from the regional mean larger than 2 standard deviation, unless the area is larger than 20 km2.

**Inputs**
| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `dir` | path | the directory where the data is stored|
| `dir_region` | path | the directory where the geodetic data is stored per RGI region |
| `region_loop_data` | table | output table from "read regionbatch" |
| `calibrate_glacierspecific_period` | variable (setting) | chosen period to use for the geodetic data (from input/settings) |
| `calimb_bn` | variable | 1D array to which the filtered and corrected geodetic mass balance data are written |

**Outputs**
| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `calimb_bn` | variable | 1D array with the filtered and corrected geodetic mass balance data |

**"Loop"** \
Before loop "Calibration phase" - preparing the calibration loop

***
## Read climatepast
**Description**\
Reads and processes historical climate data for a specified region and global reanalysis datasets or other gridded meteorological datasets (links). The reanalysis dataset is chosen in the "settings/input" and need to be in a specific format: year/month/DOY/decimal.time/temp(degC)/prec(mm/d)/temperature lapse rate. The header contains the lon,lat for the gridcell and the grid cell mean elevation. There is a file with P and T timeseries for each gridcell of the input meteorological gridded input data.\

Reanalysis products available: (1) ERA-5-Land (2) ERA -5-Reanalysis (3) Chelsea (4) .... For Switzerland, RhiresD and TabsD

**Inputs**
| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `dir_clim` | path | the directory where all the climate data is stored|
| `dir_region` | path | the directory in which the region is defined |
| `reanalysis` | variable (setting) | Name of the chosen reanalysis dataset |
| `gxs`,`gxy`  | variable | Grid coordinates (x,y). Takes it from the meteo data file name, specified by the grids that include glaciers  |
| `p_thres` | variable | Regional precipitation threshold to set "drizzle days" (days with little precipitation) to zero, e.g. 0.5 mm/d is 0 mm.d to avoid continuous precipitation |

**Outputs**
| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `temp` | variable | 1D array with the daily/monthly* temperature timeseries read from the input file [degrees] (length = nrow (input file)) |
| `prec_orig` | variable | 1D array with the daily/monthly* precipitation timeseries read from the input file [mm/t] (length = nrow (input file))|
| `prec` | variable | 1D array with the drizzle corrected daily/monthly* precipitation timeseries read from the input file [mm/t] and consequently corrected (length = nrow (input file))|
| `dtdz` | variable | 1D array with the lapse rates read in from input file [\delta degrees/100m] (length = nrow (input file)) |
| `ryear` | variable | 1D array with the years (length = nrow (input file))  |
| `rday` | variable | 1D array with the days (length = nrow (input file)) |
| `rmonth` | variable | 1D array with the monthts (length = nrow (input file)) |
| `hclim` | variable | mean elevation of the gridcell |

**"Loop"** \
In the grid loop

***
## Read gcmdata
**Description**\
Reads in the GCM data (future climate projections) according to the "GCM loop" by searching for the timeseries file that has coordinates closest to the current "past climate" file (rmid). The latitudes and longitudes from all GCM files are read from files "latitudes.dat", "longitudes.dat" giving the center coordinates and translated to the midpoint coordinate to determine which file to open for the precipitation and temperature timeseries. 

**Inputs**
| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `dir_clim` | path | the directory where all the climate data is stored|
| `GCMdata` | variable (setting) | type of GCM data, e.g. CMIP6daily, CMIP5|
| `dir_region` | path | the directory in which the region is defined |
| `GCM_model` | variable (setting) | Selection of the GCM models to be run |
| `GCM_rcp` | variable (setting) | Selection of the emission scenarios to be run, e.g. ssp126 |
| `gcms` | counter | value that indicates the loop number of the gcm loop |
| `rcps` | counter | value that indicates the loop number of the rcp loop |
| `rmid` | variable | defining the midpoint of the current (loop) past climate grid, used to look for the closest gcm point. Lat, Lon are calculated based on Lat0 and Lon0 and the gridstep|

**Outputs**
| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `tempgcm` | variable | 1D array with the daily/monthly* temperature timeseries read from the input file gcm [degrees] (length = nrow (input file)) |
| `precgcm` | variable | 1D array with the daily/monthly* precipitation timeseries read from the input file gcm [mm/t] (length = nrow (input file))|
| `gcm_year` | variable | 1D array with the years (length = nrow (input gcm file))|
| `gcm_month` | variable | 1D array with the months (length = nrow (input gcm file)) |
| `gcm_day` | variable | 1D array with the days (DOY) (length = nrow (input gcm file))  |

**"Loop"** \
Just before glacier loop, in end of "grid" loop

***
## Read hypsometry
**Description**\
The path to the glacier specific hypsometry file is defined just before the call of the read procedure. The procedure reads the hypsometry file, consisting of 10 m elevation bands for each glacier, for which the area is given, the thickness, the width, the length, the slope, the aspect, apparent mass balance (?), the basal stress and the shape factor. The procedure checks the consensus thickness data with the area from the "thick file" (source? probably consensus?), it checks: if the minimum elevation is not lower than -300m, and second if the area difference between consensus and HF2012 is not more than 50%. However, if one of these is true, it replaces the hypsometry files with the HF2012 file. If "advance" setting is true, and the number of bands (nb) is greater than three, then additional bands are created, making sure that the number of bands added fit between hmin and adv min. adv min is by default 10m. 

**Inputs**
| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `fn` | filename | filename of the hypsometry file |
| `gg` | variable | list of glaciers to be run|
| `g` | counter | indication which glacier to read in|
| `a_gl` | variable | area of the respective glacier |
| `nb` | variable | number of elevation bands for the respective glacier|
| `advance` | variable (setting) | setting indicating if glaciers can advance ("y") or not |
| `adv_calving` | variable (setting) | setting that indicates the terminal elevation of the glacier bed |
| `adv_addband` | variable (setting) | setting that initializes the number of bands that should be added |
| `hmin` | variable | the minimum elevation of the glacier from the "thick" file |

**Outputs**
| Name       | Type      | Content      |
|----------------|----------------|----------------|
| `da` | variable | 1D array with the daily/monthly* temperature timeseries read from the input file gcm [degrees] (length = nrow (input file)) |
| `adv_addband` | variable | the actual number of bands that are added |

**"Loop"** \
Within the calibration loop for the glacier specific loop
