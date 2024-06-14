
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

<details open>
<summary>Sections:</summary>
<ul><li>1. Import the FCC Broadband Availability Data</li>
<li>2. Combine FCC Technology Files</li>
<li>3. Explore Dataset</li>
<li>4. Determine Location Status (i.e., unserved, underserved, served)</li>
<li>5. Count Locations and Status per Census Geometry</li>
<li>6. Optional: Export Table Data</li>
<li>7. Create Choropleth Maps Using _ggplot2_</li>
<li>8. Count Locations and Status per H3 Hexagonal Grid</li>
<li>9. Create Choropleth Maps of H3 Status Using _ggplot2_</li>
<li>10. Map the Percentage of Served Locations per H3 for all Counties</li></ul>
</details>
<br>

#### 1. Import the FCC Broadband Availability Data

```r
# INSTALL PACKAGES
install.packages("dplyr", "tidyr", "readr")

# LOAD PACKAGES
library(dplyr)
library(tidyr)
library(readr)

# VIEW PACKAGE HELP
?readr
?dplyr
?tidyr
```
Download FCC BDC Data. Steps Below:
1. Go to the [FCC National Broadband Map](https://broadbandmap.fcc.gov/data-download)
2. In the FCC portal, Select State, and download all fixed technologies.
3. Unzip files

```r
# PRINT CURRENT WORKING DIRECTORY
getwd()

# UPDATE WORKING DIRECTORY TO FOLDER LOCATION WHERE FCC CSV FILES ARE SAVED
setwd("C:/") # CODE: INSERT FILE PATH IN PARENTHESES

# OPTIONAL: # MANUALLY SET WORKING DIRECTORY
# STEPS: IN R, GO TO: SESSION > SET WORKING DIRECTORY > CHOOSE DIRECTORY > SELECT LOCATION WHERE FCC FILES ARE SAVED 

# IMPORT FCC CSV FILES
cable <- read_csv("bdc_04_Cable_fixed_broadband_D23_14may2024.csv")
copper <- read_csv("bdc_04_Copper_fixed_broadband_D23_14may2024.csv")
fiber <- read_csv("bdc_04_FibertothePremises_fixed_broadband_D23_14may2024.csv")
GSO_sat <- read_csv("bdc_04_GSOSatellite_fixed_broadband_D23_14may2024.csv")
LBR_FW <- read_csv("bdc_04_LBRFixedWireless_fixed_broadband_D23_14may2024.csv")
L_FW <- read_csv("bdc_04_LicensedFixedWireless_fixed_broadband_D23_14may2024.csv")
NGSO_sat <- read_csv("bdc_04_NGSOSatellite_fixed_broadband_D23_14may2024.csv")
other <- read_csv("bdc_04_Other_fixed_broadband_D23_14may2024.csv")
Un_FW <- read_csv("bdc_04_UnlicensedFixedWireless_fixed_broadband_D23_14may2024.csv")
```
<br>

### 2. Combine FCC Technology Files

```r
# BIND ALL ROWS
fcc <- bind_rows(cable, copper, fiber, GSO_sat, LBR_FW, 
                 L_FW, NGSO_sat, other, Un_FW)

# OPTIONAL: CLEAN UP ENVIRONMENT
rm(cable, copper, fiber, GSO_sat, LBR_FW, 
   L_FW, NGSO_sat, other, Un_FW)
```

