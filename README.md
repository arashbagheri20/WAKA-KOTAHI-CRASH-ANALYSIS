# WAKA-KOTAHI-CRASH-ANALYSIS
## Project objective:
Builing a Power Bi dashboard to analyse 814772 traffic crashes reported since January 2000 to Waka Kotahi by New Zealand police
## Tools:
- Data Cleaning: DAX coding
- Visualization: Power Bi
## New Zealand Waka Kotahi Crash Analysis Dashboard (2000-2022)
### 1	Questions to answer/KPIs 
 		1.1	What is the total number of crashes 
		1.2	What is the total number of casualties involved
		1.3	What is the total number of fatal crashes
		1.4	What is the total number of fatal casualties
		1.5	What is the total number of seriously injured casualties
		1.6	What is the total number of minorly-injured casualties
		1.7	What is the distribution of fatal casualties by the type of vehicles involved? (Cars, trucks, motorcycles, etc)
		1.8	What is the trend of fatal casualties from 2000-2022
		1.9	What is the distribution of fatal casualties by road surface used?
		1.10	What is the distribution of fatal casualties by the type of area (open or urban) where the crash happened?
		1.11	What is the distribution of fatal casualties by the district(city) area where the crash happened?

### 2	Data cleansing
		2.1	Downloading crash data in CSV format from Waka Kotahi Crash Analysis System on:

				•	https://opendata-nzta.opendata.arcgis.com/search?tags=CAS
		2.2	Replaced the “No” value in “crashSHDescription" column with “0”.
				•	DAX Code used: 
					= Table.ReplaceValue(#"Changed Type","No","0",Replacer.ReplaceText,{"crashSHDescription"})
		2.3	Replace the “Yes” value in “crashSHDescription" column with “1”.
				•	DAX Code used:
					= Table.ReplaceValue(#"Replaced Value","Yes","1",Replacer.ReplaceText,{"crashSHDescription"})
		2.4	Filtered the cash rows dated 2023 (as the year has not ended yet, which cannot be compared with other full-year records).
				•	DAX Code used: 
					= Table.SelectRows(#"Replaced Value3", each ([carStationWagon] <> null) and ([crashYear] <> 2023))
		2.5	Added a new column by merging the “tla Name” column with “, New Zealand” to have the country name with the crash location. This was done as there are other places 					in the world with the same city names as New Zealand.
				•	DAX code used:
		 			= Table.AddColumn(#"Filtered Rows", "Merged", each Text.Combine({[tlaName], ", New Zealand"}), type text)
		2.6	= Table.AddColumn(#"Filtered Rows", "Merged", each Text.Combine({[tlaName], ", New Zealand"}), type text)
		
### 3	KPIs/Measures
		3.1	GENERAL MEASURES
				•	Used separate cards to present these KPIs
			3.1.1	Total Crashes
				•	DAX code used: 
					Total_Crashes = COUNT('Crash_Analysis_System_(CAS)_data'[X])
			3.1.2	Total Casualties
				•	DAX code used:
					Total_Casualties = sum('Crash_Analysis_System_(CAS)_data'[fatalCount]) + sum('Crash_Analysis_System_(CAS)_data'[minorInjuryCount]) + sum('Crash_Analysis_System_(CAS)_data'[seriousInjuryCount]) 
			3.1.3	Fatal Crashes
				•	DAX code used: 
					Fatal_Crashes = CALCULATE(COUNT('Crash_Analysis_System_(CAS)_data'[X]),'Crash_Analysis_System_(CAS)_data'[fatalCount]<>0) 
			3.1.4	Fatal Casualties
				•	DAX code used:
				 Fatal_Casualties = sum('Crash_Analysis_System_(CAS)_data'[fatalCount])
		  3.1.5	Serious Injured Casualties
				•	DAX code used:
				Serious_Injuried_Casualties = sum('Crash_Analysis_System_(CAS)_data'[seriousInjuryCount])
		3.1.6	Minor Injured Casualties
				•	DAX code used:
				Minor_Injuried_Casualties = sum('Crash_Analysis_System_(CAS)_data'[minorInjuryCount])
	 
	3.2	MEASURES FOR DIFFERENT VEHICLES
			•	Used a multi-row card to present these measures
		3.2.1	Total Car Fatal Casualties
				•	DAX code used:
					Total_Car_Fatal_Casualties = CALCULATE([Fatal_Casualties] , 'Crash_Analysis_System_(CAS)_data'[carStationWagon]<>0)
		3.2.2	Total Truck Fatal Casualties
				•	DAX used:
					Total_Truck_Fatal_Casualties = CALCULATE([Fatal_Casualties] , 'Crash_Analysis_System_(CAS)_data'[truck]<>0)
		3.2.3	Total Motorcycle Fatal Casualties
				•	DAX code used:
					Total_Motorcycle_Fatal_Casualties = CALCULATE([Fatal_Casualties] , 'Crash_Analysis_System_(CAS)_data'[motorcycle]<>0)
		3.2.4	Total Bicycle Fatal Casualties
				•	DAX code used: 
					Total_Bicycle_Fatal_Casualties = CALCULATE([Fatal_Casualties] , 'Crash_Analysis_System_(CAS)_data'[bicycle]<>0)
		3.2.5	Total Bus Fatal Casualties
				•	DAX code:
					Total_Bus_Fatal_Casualties = CALCULATE([Fatal_Casualties] , 'Crash_Analysis_System_(CAS)_data'[bus]<>0)
		3.2.6	Total Moped Fatal Casualties
				•	DAX code: 
					Total_Moped_Fatal_Casualties = CALCULATE([Fatal_Casualties] , 'Crash_Analysis_System_(CAS)_data'[moped]<>0)
		3.2.7	Total School Bus Fatal Casualties
				•	DAX code: 
					Total_SchoolBus_Fatal_Casualties = CALCULATE([Fatal_Casualties] , 'Crash_Analysis_System_(CAS)_data'[schoolBus]<>0)
	3.3	Fatal casualties by crash year
				•	Used a line chart to present the number of fatal casualties over the years by choosing:
					“crashyear” column as the X-axis
					“Fatal_Casualties” measure as Y-axis
	3.4	Fatal casualties by road surface
				•	Used a clustered bar chart to present the number of fatal casualties in road types by choosing:
					“Fatal_Casualties” measure as X-axis
					“roadSurface” column as the Y-axis
	3.5	Fatal casualties by weather
				•	Used a clustered bar chart to present the number of fatal casualties in different weather types by choosing:
					“Fatal_Casualties” measure as X-axis
					“weatherA” column as the Y-axis
	3.6	Fatal casualties by area type (urban/open)
				•	Used a pie chart to present the number of fatal casualties by area type by choosing:
					“Fatal Casualties” measure as the values 
					“urban” column as the legend
	3.7	Fatal casualties by geography
				•	Used a map visual tool to present the number of fatal casualties in different parts of the country by choosing:
					“Location” column as the Location
					“Fatal_Casualties” as the Bubble size
