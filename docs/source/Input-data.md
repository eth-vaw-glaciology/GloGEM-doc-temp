# Input data

GloGEM relies on various input datasets essential for glacier modelling, including ice thickness, geodetic mass balance, and climate data. Below, we provide a list of the data(sets) required by the model, along with their storage locations as of March 18, 2025. To date, these are only accessible for ETH VAW members, but they will be stored on public servers in the (near) future.  

Explanations can be given by L.V.T. and A.V.D.E. and M.H.

## Climate data

GloGEM requires climate data for the past (the historical period) and the future. In the standard setup, the models uses reanalysis data for the past climate and global climate model (GCM) output for the future. 

In general, the climate data is stored in:
**/itet-stor/[username]/glogem/climatedata/**

In this folder, different data products are stored:

## Reanalysis (1 TB)

This folder contains all the data for the historical period. There are two options: daily and monthly. 

### ERA5Land (DAILY) (1950-2024)

**ERA5-Land** is a global **high-resolution** land surface dataset produced by the European Centre for Medium-Range Weather Forecasts (ECMWF) as part of the Copernicus Climate Change Service (C3S). It provides hourly data on land surface variables at a **9 km spatial resolution**, making it a valuable resource for studying climate and environmental changes. For GloGEM, we have prepared the data in daily format. 

Files to be directly used by GloGEM:

**/itet-stor/[username]/glogem/climatedata/reanalysis/daily/era5land/[regions]**

Raw files:

**/itet-stor/[username]/glogem/climatedata/reanalysis/daily/era5land/files/**

### GSWP3-W5E5 (DAILY) (1901-2019)

**GSWP3-W5E5** is a global meteorological forcing dataset used for land surface and climate impact modeling. It merges **GSWP3** (1901–2010) and **W5E5** (1979–2019), providing **low-resolution (~0.5°)** bias-corrected climate data. It includes temperature and precipitation. Widely used in ISIMIP and Earth system models, it supports research on climate change impacts, water cycles, and land surface processes.

Files to be directly used by GloGEM:

**/itet-stor/[username]/glogem/climatedata/reanalysis/daily/gswp3w5e5/[regions]**

Raw files:

**/itet-stor/[username]/glogem/climatedata/reanalysis/daily/gswp3w5e5/files/**

### Chelsa-W5E5 (DAILY) (1979-2016)

**CHELSA-W5E5** is a **high-resolution** climate dataset that combines CHELSA (Climatologies at High Resolution for the Earth’s Land Surface Areas) with the W5E5 (WATCH + WFDE5) meteorological forcing dataset. It provides bias-corrected, **high-resolution (up to 1 km)** climate data. The dataset covers key meteorological variables such as temperature and precipitation, offering improved spatial detail over standard reanalysis datasets. By merging CHELSA's fine-scale topographic corrections with W5E5’s historical consistency, it ensures more accurate climate information, especially in mountainous and complex terrain. It is widely used for biodiversity research, ecosystem modeling, and climate risk assessments.

Files to be directly used by GloGEM:

**/itet-stor/[username]/glogem/climatedata/reanalysis/daily/chelsaw5e5/[regions]**

Raw files:

**/itet-stor/[username]/glogem/climatedata/reanalysis/daily/chelsaw5e5/files/**

### Ch2018 (DAILY) (1981-XXXX)

**This dataset contains one series for the past and future.** The ****CH2018 Reanalysis Data**** is a climate dataset developed for Switzerland as part of the CH2018 Climate Change Scenarios. It provides ****high-resolution (daily, 2 km grid)**** reanalysis and projection data for temperature, precipitation, and other meteorological variables. The dataset is based on historical climate observations combined with reanalysis data and downscaled using regional climate models. It is specifically designed to assess climate change impacts in Switzerland, supporting research in hydrology, agriculture, and ecosystem studies. 

Files to be directly used by GloGEM:

**/itet-stor/[username]/glogem/climatedata/reanalysis/daily/CH2018/[models]**

Raw files:

**/itet-stor/[username]/glogem/climatedata/reanalysis/daily/CH2018/files/**

### ERA5 (MONTHLY) (1950-2019)

**ERA5** is a global atmospheric reanalysis dataset produced by the European Centre for Medium-Range Weather Forecasts (ECMWF) under the Copernicus Climate Change Service (C3S). It provides hourly climate and weather data at a **31 km spatial resolution**, covering the period from **1950 to the present**. ERA5 includes a wide range of atmospheric, land, and oceanic variables such as temperature, precipitation, wind, humidity, and radiation. It is based on data assimilation from satellite and in-situ observations, combined with a numerical weather prediction model. The dataset is widely used in climate research, weather analysis, hydrology, renewable energy, and environmental monitoring. Compared to its predecessor (ERA-Interim), For GloGEM, we have ERA5 data avialable at **monthly resolution**. 

Please note that the monthly climate data is stored in a different structure as its size is much smaller. 

Files to be directly used by GloGEM:

**/itet-stor/[username]/glogem/climatedata/reanalysis/monthly/ERA5/[regions]**

Raw files:

**/itet-stor/[username]/glogem/climatedata/reanalysis/monthly/ERA5/files/**




## Future

### CMIP6 - ISIMIP3b (DAILY)

The ISIMIP3b framework utilizes five **GCMs from CMIP6** to provide climate projections for impact assessments. These models include **GFDL-ESM4** (developed by NOAA, strong in ocean and land-atmosphere interactions), **IPSL-CM6A-LR** (from France, focused on cloud physics and tropical climate simulations), **UKESM1-0-LL** (from the UK Met Office, known for high climate sensitivity and Earth system interactions), **MRI-ESM2-0** (from Japan, with advanced aerosol interactions and monsoon simulation), and **MPI-ESM1-2-HR** (from Germany, known for accurate representation of large-scale atmospheric circulation). These models cover a broad range of climate sensitivities and regional climate responses, ensuring robust climate impact projections. They are widely used for studying hydrology, agriculture, biodiversity, and extreme weather events in ISIMIP3b.

Files to be directly used by GloGEM:

**/itet-stor/[username]/glogem/climatedata/future/daily/cmip6isimip3b/[regions]**

Raw files:

**/itet-stor/[username]/glogem/climatedata/future/daily/cmip6isimip3b/files/**

### CMIP6 Daily (DAILY)

This folder contains CMIP6 **daily data** of 26 GCMs. There are stored in .dat files containing the temperature and precipitation for each grid cell of a specific region (e.g., Alaska) along with a timestamp starting from 1 January 1850 and running until 2100. 

Files to be directly used by GloGEM:

**/itet-stor/[username]/glogem/climatedata/future/daily/cmip6/[regions]**

Raw files:

**/itet-stor/[username]/glogem/climatedata/future/daily/cmip6/files/**










### CMIP6 Monthly

This folder contains CMIP6 **monthly data** of 17 GCMs. There are stored in .nc (raw data) and .mdi (modified format for the monthly version of GloGEM). 


### GMIP3 Monthly

This folder contains CMIP6 **monthly data** of 5 GCMs that were used for the GMIP3 simulations. The data is directly provided in .mdi files. 

### CMIP6 LONG Monthly

This folder contains CMIP6 **monthly data** of 11 GCMs. There are stored in .mdi (modified format for the monthly version of GloGEM).









## Geometric data

 Grids are located -> 

### RGIv6.0

**'_vierzack03_second/mhuss/DEMs_global_'**

### RGIv7.0

**_'/scratch_net/iceberg_second/mhuss/RGIv7.0/dem'_**

**_'/scratch_net/iceberg_second/mhuss/RGIv7.0/glmask'_**

**_'/scratch_net/iceberg_second/mhuss/RGIv7.0/bands (files/results/grids)'_**


## Ice thickness
Each glacier requires an elevation-band file (~ flowline) which provides the initial ice thickness and area of each elevation band. The standard procedure in GloGEM to date is to use the "consensus estimate" of Farinotti et al. (2019) as starting point. This data is stored in:

**_'/scratch_net/iceberg_second/mhuss/global_thickness/rgi60/bands_consensus2019/'_**

However, for some glaciers, the consensus data provides unrealistic results. Therefore, for these specific glaciers, GloGEM will not use the consensus estimate of the ice thickness, but the Huss and Farinotti (2012) ice thickness estimate. The data is stored in:

_**'/scratch_net/iceberg_second/mhuss/global_thickness/rgi60/bands_HF2012/'**_

## Debris cover

**_'/scratch_net/iceberg_second/mhuss/global_thickness/rgi60/debris/'_**

## Hypsometry

The hypsometry file is stored in the same file as the ice thickness of the consensus estimate. 

**_'/scratch_net/iceberg_second/mhuss/global_thickness/rgi60/bands_consensus2019/'_**


