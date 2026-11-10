# Dataset

`FloodPrediction_1949-2022_Final_Dataset.csv` has one row per weather station per month: 24,108 rows from 33 stations, covering 1948 to 2022.

## Sources

- Weather data for 1949 to 2013: Bangladesh Meteorological Department, taken from the dataset used in Gauhar, N., Das, S., and Moury, K. S. (2021), "Prediction of flood in Bangladesh using K-nearest neighbors algorithm", ICREST 2021.
- Weather data for 2014 to 2022: Bangladesh Meteorological Department.
- Flood labels: compiled by the authors from annual flood reports, newspapers, research papers and NGO reports (thesis section 3.2).

## Columns

| Column | Description | Unit |
|---|---|---|
| Sl | Row number (blank for some rows) | |
| Station_Names | Weather station name | |
| Year | Year | |
| Month | Month, 1 to 12 | |
| Max_Temp | Maximum temperature in the month | °C |
| Min_Temp | Minimum temperature in the month | °C |
| Rainfall | Total rainfall in the month | mm |
| Relative_Humidity | Relative humidity | % |
| Wind_Speed | Wind speed | m/s |
| Cloud_Coverage | Cloud cover | okta |
| Bright_Sunshine | Bright sunshine | hours per day |
| Station_Number | Station ID | |
| X_COR, Y_COR | Projected station coordinates | |
| LATITUDE, LONGITUDE | Station location | degrees |
| ALT | Station altitude | m |
| Period | Year and month combined, e.g. 1949.01 | |
| Flood? | 1 if a flood was recorded, 0 if recorded as no flood, blank if no record was found | |

## Labels

The `Flood?` column has 4,351 rows marked 1, 361 rows marked 0 and 19,396 blank rows. The notebook treats blanks as 0 (no flood). A blank means we found no flood record for that station and month, which is not quite the same as knowing no flood happened.

A small number of weather values are missing, at most 26 in any column.
