# Methodical Procedure for Application Development
This document contains the methodical approach used to develop the application. It is structured in chapters that match the chapters in application.ipynb. It can be used to get further information on the code if the comments in application.ipynb do not suffice.
## 1. Data Load
- load required packages
### 1.1 Charging Stations
- load charging station data directly from the opendata portal of Rhein-Kreis-Neuss found via govdata.de
- argument "*dtype = {...}*" is used to specify the expected data format for each column
### 1.2 Registered Vehicles
- load vehicle registration data from opendata portal from Kraftfahrtbundesamt
### 1.3 Mapping Data: PLZ to Administrative Region
- load mapping data linking PLZ and Administrative Region
### 1.4 Second-level Administrative Divisions, Germany, 2015
- read geojson file containing district shapes from University of California, Berkeley shape file database:<br>*See README file in data folder for further explanation on the implementation of the data load.*
## 2. Data Cleaning
This chapter outlines the steps taken to bring the raw data into a usable format given the context of the application
### 2.2 Charging Stations
- copy raw data to working df
- replace column names with english names
- drop irrelevant columns
- summarize data into stations_summary:<br>*The cleaned data has to be aggregated on zip code area level. Therefore, it is being grouped by the zip code and then aggregated using "sum" as aggregation operator.*
- convert index to column:<br>*As a result of the aggregation, the zip code has been moved to the index column. As this might make things more complicated further down the road when merging and plotting the data, the index column is copied to a regular column.*
- rename and reset index:<br>*To avoid ambiguity in the data frame between the index column and the newly created column, the index column is being reset and renamed.*
### 2.3 Registered Vehicles 
- copy raw data to working df:<br>*To avoid having to load the raw dataset from the website over and over again, the raw data frame is copied to a working version*
- remove empty first column:<br>*As the raw data is a rather messy Excel file, some rows, and columns need to be removed.*
- drop first rows containing no usable data
- drop last rows containing no usable data:<br>*The last few rows contain summarized data on various levels and need to be removed.*
- reset index:<br>*After removing many rows, the index is reset to ensure working with a cleanly indexed data frame.*
- assign proper english column names
- drop irrelevant columns
- fill NaN values:<br>*As the original data has the format of a pivot table without repeating column labels for the columns "state" and "district_raw", those labels need to be inserted manually.*
- remove summary rows:<br>*The columns "municipality" and "state" contain summary rows spread throughout the data frame. These need to be identified, and their index needs to be stored to remove them*
  - define column to be searched (municipality)
  - loop through all the rows and append row index do drop list
  - define column to be searched (state)
  - loop through all the rows and append row index do drop list
  - iterate through droppable list and execute drop
- reset index
- fix values mismatching shape file (Trier & Eisenach):<br>*Data inconsistencies between the files were identified by running the application and noticing that several polygons were not plotted at all. A root cause analysis was executed. Here, two cities (Eisenach & Trier) were incorrectly incorporated into the surrounding districts, while they should be their own district. The correct district_ids were assigned to the two cities.*
- extract district_id:<br>*So far, the district column was an additive column consisting of the name of the district and the ID added in brackets. This column has to be split up into two columns.*
- drop irrelevant columns
- reorder columns
- replace "." & "-" value with 0:<br>*As there were different notations used for 0 registered vehicles in the dataset, these need to be replaced with proper zero values.*
- assign correct column types
- summarize number of cars per district_id:<br>*The cleaned data has to be aggregated on district level. Therefore, it is being grouped by the district_id and then aggregated using "sum" as aggregation operator.*
- convert index to column:<br>*As a result of the aggregation, the district_id has been moved to the index column. As this might make things more complicated further down the road when merging and plotting the data, the index column is copied to a regular column.*
- rename and reset index:<br>*To avoid ambiguity in the data frame between the index column and the newly created column, the index column is being reset and renamed.*
### 2.3 Mapping Data: PLZ to Administrative Region
- copy raw data to working df:<br>*To avoid having to load the raw dataset from the website over and over again, the raw data frame is copied to a working version*
- assign proper column names
- drop irrelevant columns
- fix values mismatching shape file (Eisenach):<br>*Similar to the adjustments made to the vehicle data, the mapping data also has to be adjusted to ensure all zip codes and district_ids can be properly linked to the polygon data. In this particular case, there was one zip code that was registered as its own district, which doesn't exist in the polygon data. The zip code was mapped to the correct district_id.*
- strip last 3 digits of ags code:<br>*The mapping data contains the ags code on community level. The last three digits containing the community information need to be dropped for the ags code to contain district level information.*
- remove duplicates created by truncation of district_id:<br>*The truncation above created duplicate records that now have to be removed.*
### 2.4 Second-level Administrative Divisions
- fix values mismatching vehicle & station data (Göttingen & Osterode am Harz):<br>*The last inconsistencies between the datasets need to be removed to ensure every district is plotted properly. Göttingen & Osterode am Harz were assigned different district_ids in the mapping file than in the polygon file, which is addressed here. The fix was executed in the polygon dataset*
## 3. Data Merge
- join mapping data with stations_summary using zip:<br>*Before the station data can be joined with the vehicle data, it has to be enriched with district_id information that comes in via the mapping data frame.*
- summarize on district level
- join with vehicle data using district_id
## 4. KPI Computation
- calculate number of stations per 1000 cars per district:<br>*To ensure a more easily readable format of the desired indicator, the value is calculated per 1000 cars.*
## 5. Visualization
This chapter outlines the steps taken to visualize the KPI calculated above.
### 5.1 Visualization Preparation
- add KPI to districts data frame:<br>*The KPI is added to the data frame containing the polygon information using the district_id and the cca_2.*
### 5.2 Visualization Creation
- define chart configuration
- add plot:
  - apply classification scheme:<br>*The classification scheme "quantiles" is chosen to classify the data points into buckets containing equal amounts of data points because there are some rather steep outliers on the positive end which would make a continuous color coding hard to interpret.*
  - apply split into 10 buckets to classification:<br>*The data points are classified into buckets, each containing 10% of the available data points, to ensure a nice level of detail.*
  - select color schema
  - set legend to "on"
  - configure legend
  - configure polygon borderlines
- remove grid lines
- add title
- add caption
- report successful visualization