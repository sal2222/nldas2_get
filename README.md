# nldas2_get

RMarkdown files

## 1. `lat_lon`
Input: "data/centroid_coords.rds"
Output: "output/centroids.rds"

- Convert `sf` centroid coordinates to lon/lat in dataframe
- Recode long installation names

## 2. `nldasR_query`
Output: "output/nldas_8.rds"

- Interface with NASA Data Rods (Time series) services managed by Goddard Earth Sciences Data and Information Services Center (GES DISC)
- Available NLDAS 2 bands: Provides: datetime, APCPsfc, DLWRFsfc, DSWRFsfc, PEVAPsfc, SPFH2m, TMP2m, UGRD10m, VGRD10m
- Surface Pressure band not available through `hydro1.gesdisc.eosdis.nasa.gov` server
- Run queries for 24 sites from 1990-2021 with `nldas2_primary_forcing_rods` function (https://rdrr.io/github/lawinslow/nldasR/src/R/nldas2_primary_forcing_rods.R)
- 3 locations returned "Request failed [404]" error due to masked cell in server (Pensacola, San Diego, Parris Island), added 0.05 degrees to lat and lon (less than one grid square)
- Compiles batched data into single dataframe (separated due to server errors)

## 3. `compare_nldas`
- QA/QC step
- Compare site temperatures with previously compiled data

## 4. `gldasR_query_pressure`
Output: "/output/gldas_pressure.rds" (back-up surface pressure data)

- Extract and compile 3-hourly surface pressure from GLDAS_NOAH025_3H_v2.0 (1990-2000) and GLDAS_NOAH025_3H_v2.1 (2000-2021)
- Adapted `nldas2_primary_forcing_rods` function for GLDAS calls
- Back-up and QA/QC for NLDAS 2 surface pressure from Google Earth Engine script extraction

## 5. `get_surface_pressure_earth_engine1`
Output: Google Drive "pressure_decade/pressure2000.csv", "pressure_decade/pressure2010.csv", "pressure_decade/pressure2020.csv"

- Copy of Javascript code (not R) used in the Earth Engine code editor to extract hourly surface pressure band for 24 points
- 3 to 10 hours each for by-decade tasks to execute (32-year task failed after 12 hours)

## 6. `surf_press_from_gee`
Input: Google Drive "pressure_decade/pressure2000.csv", "pressure_decade/pressure2010.csv", "pressure_decade/pressure2020.csv"
Output:

- transform and compile NLDAS 2 surgace pressures from GEE


