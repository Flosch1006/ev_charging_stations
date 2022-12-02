# Dataset Description

This document contains the descriptions of the four datasets used to develop the application "Charging Stations for Electrical Vehicles by Count of Registered Cars per ZIP Code". The two datasets used are the following:
- Deutschland: E-Ladesäulen
- Bestand an Kraftfahrzeugen und Kraftfahrzeuganhängern nach Gemeinden (FZ 3)
- Zuordnung PLZ Ort
- Second-level Administrative Divisions, Germany, 2015

The most up-to-date version was used for each dataset and where possible, a live connection to the data source was implemented. The charging station dataset is pretty up-to-date, given it was last updated on November 14th 2022. The registered vehicles dataset is only published on a yearly basis, so the last iteration available is the one from December 31st, 2021. This needs to be kept in mind and an update after the new data release is highly suggested.

## Deutschland: E-Ladesäulen
This dataset contains all charging stations that can be used to charge electrical vehicles that have been registered at the Bundesnetzagentur by November 14th 2022. The data is provided by the Bundesnetzagentur and was published under "Creative Commons Namensnennung 4.0 International Lizenz". The download is hosted by the "Rheinkreis Neuss" and available as .geojson, .xls, .csv, .kml, .json and .shp file. It can be found on [govdata.de](https://www.govdata.de/web/guest/suchen/-/details/deutschland-e-ladesaulenf635c).

This dataset is particularly interesting to me because electrical (individual) transportation will be an integral part of our way to move around for the foreseeable future. A well-developed charging station network is a key part to accelerate the adoption of electrical vehicles, and identifying gaps in the charging infrastructure can be used to act and improve the infrastructure where needed. 

The dataset contains the following columns:
- **Betreiber:** The legal entity that owns and runs the charging station as string
- **Anzahl Ladepunkte:** The count of available charging points at the location as integer with a range from 1 to 4
- **Anschlussleistung:** Connected load is the total electricity supply that is provided to a meter in kW as decimal
- **Steckertyp 1-4:** Current type of the given charging point specifying whether the point provides alternating current (AC) or direct current (DC) with additional specifications as string
- **P1-P4[kW]:** Connected load of specified charging point in kW as decimal
- **Kreis/kreisfreie Stadt:** District (third administrative layer) in which the charging station is located as string
- **Ort:** Municipality (fourth administrative layer) in which the charging station is located as string
- **Postleitzahl:** Zip Code in which the charging station is located as integer
- **Straße:** Name of the street in which the charging station is located as string
- **Hausnummer:** House number in which the charging station is located as string
- **Adresszusatz:** Additional information on the location in which the charging station is located as string
- **Inbetriebnahme:** Date the charging station was installed as string in the format YYYY-MM-DD
- **Koordinaten:** Latitude and longitude of the charging station as string in the format "49.76918,11.33907" with the first decimal being the latitude and the second decimal being the longitude
- **Normalladeeinrichtung:** Specifies whether the charging station is a regular charging station or a fast-charge charging station as string

The most important columns in this dataset are "Anzahl Ladepunkte", "Postleitzahl", "Inbetriebnahme" and "Normalladeeinrichtung". These will be used to combine this dataset with the other dataset using the zip code, as well as to provide means of drilling down into the data by date and type of charging station (fast vs. regular).

The challenge with this dataset is the data load. The free text field *Addresszusatz* contains lots of string formatting such as quotation marks, line breaks, etc. Therefore, the dataset will have to be loaded line by line rather than using the import function provided by the pandas package.

# Bestand an Kraftfahrzeugen und Kraftfahrzeuganhängern nach Gemeinden (FZ 3)
This dataset contains data on the number of registered motor vehicles at the Kraftfahrbundesamt in Germany by various administrative layers as of January 1st 2022. The data is consolidated at the Kraftfahrbundesamt from the various admission offices across the country. The data is provided by the Kraftfahrbundesamt as .xls file. It can be found on [govdata.de](https://www.govdata.de/web/guest/suchen/-/details/bestand-an-kraftfahrzeugen-und-kraftfahrzeuganhangern-nach-gemeinden-fz-3). For districts containing multiple zip codes, the count is summarized under the lowest zip code of the district.

This dataset is the second key part that is required to enable the desired application, as the count of registered cars can be used as a proxy for the demand for charging station. However, it should be noted that there is no distinguishing between electrical and fuel-based cars. Hence, I assume a rather uniform distribution of the share of electrical vehicles across the different regions of Germany.

The dataset contains the following columns:
- **Land:** State of Germany provided as string
- **Zulassungsbezirk:** District of Germany and its ID provided as string in the format "Name (ID)"
- **PLZ, Gemeinde:** Zip code and name of the municipality provided as string in the format "PLZ  Name"
- **Krafträder:** Count of registered motorcycles as integer
- **Personenkraftwagen insgesamt:** Total count of registered passenger cars as integer
- **Personenkraftwagen darunter gewerbliche Halter:** Count of registered passenger cars registered by a business as integer
- **Lastkraftwagen:** Count of registered trucks as integer
- **Zugmaschinen insgesamt:** Total count of registered tractors as integer
- **Zugmaschinen dar.land-/forstwirtschaftliche Zugmaschinen:** Count of registered tractors used for agricultural or forestry purposes as integer
- **Sonstige Kfz einschl. Kraftomnibusse:** Count of other types of motor vehicles including buses as integer
- **Kraftfahrzeuge insgesamt:** Total count of motor vehicles as integer
- **Kraftfahrzeuganhänger:** Count of motor vehicle trailers as integer

The most important columns in this dataset are "PLZ, Gemeinde" and "Personenkraftwagen insgesamt". The former will be used to join the data with the first dataset. The latter will be used as a proxy for the demand for electrical charging stations.

The challenge with this dataset is its general formatting, as it is available as a xls file that contains lots of formatting to make it look pretty. There are merged cells, empty columns, empty rows and summary rows, that have to be accounted for.

## Zuordnung PLZ Ort
This dataset is used as a mapping table to combine the aforementioned datasets. It contains a mapping from zip codes to administrative districts and is provided by Marco Schochow, a German software developer, on his website [suche-postleitzahl.org](https://www.suche-postleitzahl.org/downloads). The raw data is sourced from OpenStreetMap.
The dataset contains the following columns:
- **osm_id:** OpenStreetMap ID of the zip code area as integer
- **ags:** Amtlicher Gemeindeschlüssel (AGS) is a unique ID to identify an administrative entity as integer; consists of 8 digits with the first two digits denoting the state, the third digit denoting the governorate, the fourth, and fifth denoting the district and the last three digits denoting the local community
- **ort:** Name of the zip code area as string
- **plz:** Zip code as integer of length 5
- **landkreis:** Name of the administrative district as string
- **bundesland:** Name of the state

From this dataset, only the columns "ags" and "plz" will be used, as its purpose is to simply map the zip codes from the vehicle data to district IDs to join them to the charging station dataset.

The challenge with this dataset is that the *ags* columns contains the ID on municipality level. This has to be adjusted to extract the ID on district level.

## Second-level Administrative Divisions, Germany, 2015
This dataset is used to create the final visualization. It contains polygons for every administrative district in the form of geo-coordinates. It is provided by the California,  Berkeley, Museum of Vertebrate Zoology within their [Berkeley University of California Library GEODATA](https://geodata.lib.berkeley.edu/catalog/stanford-nz271ny2119). Its author is Robert J. Hijmans.

The dataset is a JSON dictionary of the following form:
```json
{
  "type": "FeatureCollection",
  "features": [list of features],
  "totalFeatures": 403,
  "numberMatched": 403,
  "numberReturned": 403,
  "timeStamp": "2022-11-21T17:15:44.251Z",
  "crs": {
    "type": "name",
    "properties": {
      "name": "urn:ogc:def:crs:EPSG::4326"
    }
  },
  "bbox": [
    5.86625051,
    47.27012253,
    15.04181671,
    55.05653
  ]
}
```
The *list of features* is a list of coordinate pairs, describing the border of the polygon and each having the following format:
```json
{
  "type": "Feature",
  "id": "nz271ny2119.1",
  "geometry": {
    "type": "MultiPolygon",
    "coordinates": [[list of coordinate pairs]]
  },
  "geometry_name": "geom",
  "properties": {
    "id_0": 86,
    "iso": "DEU",
    "name_0": "Germany",
    "id_1": 1,
    "name_1": "Baden-Württemberg",
    "id_2": 1,
    "name_2": "Alb-Donau-Kreis",
    "hasc_2": "DE.BW.AD",
    "ccn_2": 0,
    "cca_2": "08425",
    "type_2": "Landkreis",
    "engtype_2": "District",
    "nl_name_2": null,
    "varname_2": null
  },
  "bbox": [
    9.48915768,
    48.14228821,
    10.24140644,
    48.63175583
  ]
}
```
The *property* dictionary contains the following keys describing the polygon:
- **id_0:** Country ID as integer of length 2
- **iso:** Country ISO code as string of length 3
- **name_0:** Country name as string
- **id_1:** State ID as integer of length between 1 and 2
- **name_1:** State name as string 
- **id_2:** District ID as integer of length between 1 and 3
- **name_2:** District Name as string
- **hasc_2:** Third level hierarchical administrative subdivision code as string with form "AB.CD.EF" with the first two digits being the country, the middle two being the state and the last two being the district 
- **ccn_2:** NA
- **cca_2:** 5-digit district code as integer of length 5
- **type_2:** District type as string with "Landkreis", "Stadtkreis" and "Kreisfreie Stadt" as options
- **engtype_2:** English name of administrative layer type
- **nl_name_2:** NA
- **varname_2:** Alternative name for the district as string

The most important objects of this data frame are the *cca_2* ID as it will be used to join this data frame to the KPI and the *geometry* object as it contains the polygon data points that will be used to create the visualization.