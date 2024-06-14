
## WORKING WITH FCC BROADBAND AVAILABILITY DATA IN ARIZONA USING R. 

The following code uses FCC broadband availability data to determine the status (i.e., unserved, underserved, served) of broadband servicable locations (BSL). 
Arizona is the primary state of analysis, however, this code can be used for any state with Census and BDC data!
Sections of the code where these changes can be made have been annotated. 

The _dyplr_ and _tidyr_ packages are used to summarize the total number of unserved, underserved, and served locations within a particular geometry. 
The FCC Broadband Availability data is publicly available and can be downloaded here [broadbandmap.fcc.gov](https://broadband.fcc.gov). 
The dataset does not contain latitude/longitude information and cannot be mapped at the household level without a CostQuest license. 
Instead, this analysis uses the H3 Hexagonal Grid and Census Block information included in the publicly available data. 
The _tigris_ package is used to directly download and use U.S. Census Bureau TIGER/Line spatial features (i.e., blocks, block groups, and counties). T
he _h3jsr_ package is used to create H3 Hexagonal Grids covering the entire state of Arizona. 

The _ggplot2_ package is used to map the percentage of served locations within a particular Census or H3 geometry. 

Sections:
- 1. IMPORT THE FCC BROADBAND AVAILABILITY DATA
- 2. COMBINE FCC TECHNOLOGY FILES
- 3. EXPLORE DATASET
- 4. DETERMINE LOCATION STATUS (I.E., UNSERVED, UNDERSERVED, SERVED)
- 5. COUNT LOCATIONS AND STATUS PER CENSUS GEOMETRY
- 6. OPTIONAL: EXPORT TABLE DATA
- 7. CREATE CHOROPLETH MAPS USING 'GGPLOT2' 
- 8. COUNT LOCATIONS AND STATUS PER HEX BIN
- 9. CREATE CHOROPLETH MAPS OF HEX STATUS USING 'GGPLOT2'
- 10. MAP PERCENTAGE OF SERVED LOCATIONS IN H3 FOR ALL COUNTIES
