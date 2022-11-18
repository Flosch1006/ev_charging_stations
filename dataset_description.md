# Dataset Description

This document contains the descriptions of the two datasets used to develop the application "Charging Stations for Electrical Vehicles by Count of Registered Cars per ZIP Code". The two datasets used are the following:
- Deutschland: E-Ladesäulen
- Bestand an Kraftfahrzeugen und Kraftfahrzeuganhängern nach Gemeinden (FZ 3)

## Deutschland: E-Ladesäulen
This dataset contains all charging stations that can be used to charge electrical vehicles that have been registered at the Bundesnetzagentur by November 14th 2022. The data is provided by the Bundesnetzagentur and was published under "Creative Commons Namensnennung 4.0 International Lizenz". The download is hosted by the "Rheinkreis Neuss" and available as .geojson, .xls, .csv, .kml, .json and .shp file. It can be found on [govdata.de](https://www.govdata.de/web/guest/suchen/-/details/deutschland-e-ladesaulenf635c).
This dataset is particularly interesting to me because electrical (individual) transportation will be an integral part of our way to move around for the foreseeable future. A well-developed charging station network is a key part to accelerate the adoption of electrical vehicles and identifying gaps in the charging infrastructure can be used to take action and improve the infrastructure where needed. 
The dataset contains the following columns:
- **Betreiber:** The legal entity that owns and runs the charging station as string
- **Anzahl Ladepunkte:** The count of available charging points at the location as integer witha range from 1 to 4
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