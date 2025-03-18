# Input data

GloGEM relies on various input datasets essential for glacier modelling, including ice thickness, geodetic mass balance, and climate data. Below, we provide a list of the data(sets) required by the model, along with their storage locations as of January 22, 2025. To date, these are only accessible for ETH VAW members, but they will be stored on public servers in the (near) future. 

Explanations can be given by L.V.T. and A.V.D.E.

## Climate data

GloGEM requires climate data, temperature and precipitation, to be run. 

## Climate data

GloGEM requires climate data for the past (the historical period) and the future. In the standard setup, the models uses reanalysis data for the past climate and global climate model (GCM) output for the future. In general, the climate data is stored in:

_**/scratch_net/vierzack04_fourth/mhuss/klima/**_

In this folder, different data products are stored:

## Reanalysis
### ERA5
### ERA5Land
### GSWP3-W5e5
### Chelsa-W5e5

## Future - GCM
### CMIP6
This folder contains CMIP6 **monthly data** of 17 GCMs. There are stored in .nc (raw data) and .mdi (modified format for the monthly version of GloGEM). 
### CMIP6_Daily
This folder contains CMIP6 **daily data** of 26 GCMs. There are stored in .dat files containing the temperature and precipitation for each grid cell of a specific region (e.g., Alaska) along with a timestamp starting from 1 January 1850 and running until 2100. 
### GMIP3
This folder contains CMIP6 **monthly data** of 5 GCMs that were used for the GMIP3 simulations. The data is directly provided in .mdi files. 
### long_CMIP6
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


