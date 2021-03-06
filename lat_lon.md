---
title: "lat_lon"
output: 
  html_document:
   keep_md: true
editor_options: 
  chunk_output_type: console
---



Input: "data/centroid_coords.rds"
Output: "output/centroids.Rmd"


## Coordinates of base shapefile centroids

```r
# Load geometry and centroids for 30 installations

base_geometries <- read_rds(file = "data/centroid_coords.rds")

# Centroid X, Y coordinates
coordinates <- st_coordinates(base_geometries$centroid) %>% as_tibble()

# lat, lon of centroids

centroids <- cbind(base_geometries, coordinates) %>% 
  dplyr::select(-state_terr, -geometry, -centroid) %>% 
  rename(lat = Y,
         lon = X) %>%
  as_tibble() %>% 
  dplyr::filter(!site_name %in% c("Fort Huachuca","Fort Carson","Fort Lewis", "Lackland AFB",
                                  "West Point Mil Reservation","Fort Drum")) %>%
  mutate(site_name = recode(site_name, `Fort Benning GA` = "Fort Benning",
                            `Naval Medical Center Portsmouth` = "NMC Portsmouth",
                            `NTC and Fort Irwin` = "Fort Irwin",
                            `Twentynine Palms Main Base` = "Twentynine Palms",
                            `MCRD Beaufort Parris Island` = "MCRD Parris Island",
                            `Fort Sam Houston` = "Joint Base San Antonio"))

# write_rds(centroids, "output/centroids.rds")
```


Tested Hydroshare data rods: not available for hourly pressure    
    
 NLDAS-2 "Data Rod" Query URLs
https://help.hydroshare.org/apps/data-rods-explorer/
https://apps.hydroshare.org/apps/data-rods-explorer/run-tests/
https://github.com/gespinoza/datarodsexplorer/blob/master/docs/DREUserGuide.md#fig16


Example link:
https://hydro1.gesdisc.eosdis.nasa.gov/daac-bin/access/timeseries.cgi?variable=NLDAS:NLDAS_FORA0125_H.002:APCPsfc&type=asc2&location=GEOM:POINT(-96.0%2C%2039.0)&startDate=1979-01-02T23&endDate=2022-03-10T00

Bands: DSWRF, SPFH2m, TMP2m, UGRD10m, VGRD10m, 
  Surface pressure not available???: PRESsfc

Test Benning temp

NASA Data Request:
https://hydro1.gesdisc.eosdis.nasa.gov/daac-bin/access/timeseries.cgi?variable=NLDAS:NLDAS_FORA0125_H.002:TMP2m&type=asc2&location=GEOM:POINT(-84.8012, 32.3996)&startDate=1989-12-31T00&endDate=2022-01-01T00


Pressure: https://disc.gsfc.nasa.gov/information/tools?title=Hydrology%20Data%20Rods: Psurf_f_inst	 

https://hydro1.gesdisc.eosdis.nasa.gov/daac-bin/access/timeseries.cgi?variable=NLDAS:NLDAS_NOAH0125_H:Psurf_f_inst&type=asc2&location=GEOM:POINT(-84.8012, 32.3996)&startDate=1989-12-31T00&endDate=2022-01-01T00

----------------------------------------
