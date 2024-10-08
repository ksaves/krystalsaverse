# GIS Portfolio

---

## Working with FCC Broadband Availability Data in Arizona Using R

The following code uses FCC Broadband Availability Data to determine the status (i.e., unserved, underserved, served) of broadband servicable locations (BSL). Arizona is the primary state for this analysis, however, this code can be used for any state!

The [_dyplr_](https://dplyr.tidyverse.org/) and [_tidyr_](https://tidyr.tidyverse.org/) packages are used to summarize the total number of unserved, underserved, and served locations within a particular geometry. The FCC Broadband Availability data is publicly available and can be downloaded at [_broadbandmap.fcc.gov_](https://broadband.fcc.gov). The dataset does not contain latitude/longitude information and cannot be mapped at the household level without a [CostQuest](https://www.costquest.com/resources/articles/broadband-policy/fcc-fabric-license-available-for-academic-broadband-research/) license. Instead, this analysis uses the H3 Hexagonal Grid and Census Block information included in the publicly available data. The [_tigris_](https://github.com/walkerke/tigris) package is used to directly download and use U.S. Census Bureau [TIGER/Line](https://www.census.gov/geographies/mapping-files/time-series/geo/tiger-line-file.html) spatial features (i.e., blocks, block groups, and counties). The [_h3jsr_](https://obrl-soil.github.io/h3jsr/) package is used to create [H3](https://h3geo.org/docs/core-library/overview/) Hexagonal Grids covering the entire state of Arizona. The [_ggplot2_](https://ggplot2.tidyverse.org/) package is used to map the percentage of served locations within a particular Census or H3 geometry. 

<img src="images/Map_Of_Percentage_Of_Served_Locations_BY_County.png" width="200" height="200" /> <img src="images/Map_Of_Percentage_Of_Served_Locations_BY_BlockGroup.png" width="200" height="200"/> <img src="images/Map_Of_Percentage_Of_Served_Locations_BY_Block.png" width="200" height="200"/> <img src="images/Map_Of_Percentage_Of_Served_Locations_BY_H3_Hexagonal_Grid.png" width="200" height="200"/>

View R Code here! [broadband_availability_data_in_R.md](https://github.com/ksaves/krystalsaverse/blob/master/broadband_availability_data_in_R.md)

Example below:

```r
# DETERMINE LOCATION STATUS
fcc_bsl_status <- fcc %>%
    mutate(num_status = if_else(low_latency == 0 | 
                                max_advertised_download_speed < 25 | 
                                max_advertised_upload_speed < 3 |
                                technology %in% c(0, 60, 61, 70), 0, # UNSERVED
                        if_else(low_latency == 1 & 
                                (between(max_advertised_download_speed, 25, 99) | 
                                between(max_advertised_upload_speed, 3, 19)) &
                                technology %in% c(10, 40, 50, 71, 72), 1, # UNDERSERVED
                        if_else(low_latency == 1 & 
                                max_advertised_download_speed >= 100 & 
                                max_advertised_upload_speed >= 20 &
                                technology %in% c(10, 40, 50, 71, 72), 2, NA)))) %>% # SERVED
      group_by(location_id, block_geoid, h3_res8_id) %>%
      summarise(status = as.character(max(num_status))) %>%
      ungroup() %>%
      mutate(status = if_else(status == 0, "unserved", 
                      if_else(status == 1, "underserved",
                      if_else(status == 2, "served", NA))))
```
<details open>
<summary>View County Maps (R Output)</summary>
<br>
<img src="images/Map_of_Percentage_of_Served_Locations_by_H3_Hexagonal_Grid_ Apache _County.png" width="200" height="200" /> <img src="images/Map_of_Percentage_of_Served_Locations_by_H3_Hexagonal_Grid_ Cochise _County.png" width="200" height="200" /> <img src="images/Map_of_Percentage_of_Served_Locations_by_H3_Hexagonal_Grid_ Coconino _County.png" width="200" height="200" /> <img src="images/Map_of_Percentage_of_Served_Locations_by_H3_Hexagonal_Grid_ Gila _County.png" width="200" height="200" /> <img src="images/Map_of_Percentage_of_Served_Locations_by_H3_Hexagonal_Grid_ Graham _County.png" width="200" height="200" /> <img src="images/Map_of_Percentage_of_Served_Locations_by_H3_Hexagonal_Grid_ Greenlee _County.png" width="200" height="200" /> <img src="images/Map_of_Percentage_of_Served_Locations_by_H3_Hexagonal_Grid_ La Paz _County.png" width="200" height="200" /> <img src="images/Map_of_Percentage_of_Served_Locations_by_H3_Hexagonal_Grid_ Maricopa _County.png" width="200" height="200" /> <img src="images/Map_of_Percentage_of_Served_Locations_by_H3_Hexagonal_Grid_ Mohave _County.png" width="200" height="200" /> <img src="images/Map_of_Percentage_of_Served_Locations_by_H3_Hexagonal_Grid_ Navajo _County.png" width="200" height="200" /> <img src="images/Map_of_Percentage_of_Served_Locations_by_H3_Hexagonal_Grid_ Pima _County.png" width="200" height="200" /> <img src="images/Map_of_Percentage_of_Served_Locations_by_H3_Hexagonal_Grid_ Pinal _County.png" width="200" height="200" /> <img src="images/Map_of_Percentage_of_Served_Locations_by_H3_Hexagonal_Grid_ Santa Cruz _County.png" width="200" height="200" /> <img src="images/Map_of_Percentage_of_Served_Locations_by_H3_Hexagonal_Grid_ Yavapai _County.png" width="200" height="200" />
      
</details>

<br>


---


## Walksheds and Bikesheds to Career Centers in Phoenix, Arizona

Exploring 5, 10, 15 minute walksheds and bikesheds to Career Centers in Phoenix, AZ

<img src="images/PosterPresentation_Walk_and_Bike_sheds_for_Phoenix.png"/>

<sub>Check out the [ArcGIS StoryMap](https://storymaps.arcgis.com/stories/1b23c0736c6140bebdc5611bc529a1d4)</sub>


---

## Pedestrian/Vehicle Accidents in Columbia, Missouri

(https://geography.missouri.edu/news/brown-bag-lecture-campuspedestrian-vehicle-accidents-held-zoom-wed-may-12)



Last Edit: 06/18/2024 11:31 AZ
<p style="font-size:11px">Page template forked from <a href="https://github.com/evanca/quick-portfolio">evanca</a></p>
<!-- Remove above link if you don't want to attibute -->
