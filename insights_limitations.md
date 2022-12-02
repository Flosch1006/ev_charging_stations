# Application Insights
As outlined in the feature section of the application conceptualization, it is designed to allow the user to identify districts where the supply of publicly available charging station lags behind compared to other districts given its demand represented by the number of registered cars.

The following insights can be derived:
- Especially the very southern states of Bavaria and Baden Wurttemberg take a lead role in terms of publicly available charging stations in relation to registered vehicles.
- Interestingly, the district were former Federal Minister of Transport and Digital Infrastructure, Andreas Scheuer, has its home district and the adjacent districts form a cluster of districts having a very high density of charging stations per 1000 vehicles.
- Wolfsburg has the highest density of charging stations per 1000 cars out of the all the German districts. A potential cause for this could be the car manufacturer Volkswagen having its headquarters and production facilities in Wolfsburg. This would have to be further analyzed to confirm.
- Other notable high density values can be found in the very norther German state Schleswig-Holstein.
- In general, cities offer more charging stations per 1000 cats than more rural areas do, as can be derived from the rather small districts typically being cities.
- Also, an extreme lack of charging stations in rural areas throughout the new states being former DDR states. This is a typical pattern when it comes to infrastructural topics in Eastern states compared to Western states that can be observed here as well.
- Surprisingly, the most densely populated area in Germany, the Ruhrgebiet, has a rather bad supply of charging stations.

# Limitations
The following limitations of this application have to be kept in mind when interpreting the visualization:

- The dataset containing the registered vehicles data is only updated once a year which might result notable discrepancies towards the end of the calendar year
- There is no explicit total of electronic vehicles provided in the data, but share of EVs on total vehicles might differ strongly from district to district.
- The EV charging stations dataset only contains public charging station. Privately and company owned stations unavailable to public cannot be accounted for, but might account for a significant number of charging stations.