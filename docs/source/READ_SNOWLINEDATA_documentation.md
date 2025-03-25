# Snowline calibration


# Snowline: `READ_SNOWLINEDATA.pro`

## Description

This procedure reads observed snowline data from a region-specific file and extracts the metadata and numerical values necessary for calibration of glacier-specific snowline models. It identifies the appropriate snowline data file based on the provided region and calibration period, then reads and parses the content into structured arrays.

The procedure performs the following key tasks:

- Constructs the snowline data filename based on input parameters.
- Reads and parses metadata and numerical content from the file.
- Splits each line of data into components representing year, day of year (DOY), snowline elevation, standard deviation.
- Extracts glacier ID and calibration periods for model fitting.
- Populates output arrays with year, DOY, snowline observations, and their uncertainty.

These outputs are used in subsequent glacier modeling routines to calibrate and validate snowline-based mass balance models.

---

## Inputs

| Name                          | Type           | Content                                                                 |
|-------------------------------|----------------|-------------------------------------------------------------------------|
| `dir`                         | String         | Path to the base directory containing snowline data subfolders.         |
| `dir_region`                  | String Array   | Array mapping regions to directory names.                               |
| `region_loop_data`            | String Array   | 2D array containing region-specific metadata and codes.                 |
| `calibrate_glacierspecific_period` | String    | Calibration period in format `"YYYY_YYYY"` used to locate the correct file. |

---

## Outputs

| Name                    | Type           | Content                                                                 |
|-------------------------|----------------|-------------------------------------------------------------------------|
| `year_obs`              | Numeric Array  | Array of observation years extracted from snowline data.                |
| `doy_obs`               | Numeric Array  | Day-of-year values for each observation.                                |
| `snowlineday_obs`       | Numeric Array  | Observed snowline elevation values.                                     |
| `snowline_stdev_obs`    | Numeric Array  | Standard deviation of observed snowline values.                         |
| `calisnow_p0`           | Numeric Array  | Calibration start year for each entry.                                  |
| `calisnow_p1`           | Numeric Array  | Calibration end year (exclusive) for each entry.                        |
| `calisnow_gid`          | String Array   | Extracted glacier IDs from the identifier strings.                      |

---

## Process Overview

1. **File Identification**
   - The procedure matches `region_loop_data` against `dir_region` to determine the correct region code.
   - It then constructs the filename using the region code and calibration period.

2. **File Parsing**
   - Reads the snowline data file, skipping the header line.
   - Parses each line of snowline data into components including:
     - Glacier ID
     - Snowline elevation
     - Standard deviation
     - Observation year
     - Day of year (DOY)

3. **Data Extraction**
   - Extracts the glacier ID from the string identifier in each row.
   - Populates output arrays with relevant snowline observation data.


