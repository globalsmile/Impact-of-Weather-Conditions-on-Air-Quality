# Impact of Weather Conditions on Air Quality
![image](https://user-images.githubusercontent.com/106287208/226215112-76d10498-30a9-499f-a627-e12625a27942.png)


---


# Table of Contents

- [Introduction](https://github.com/globalsmile/Impact-of-Weather-Conditions-on-Air-Quality#Introduction)
- [Problem Statement](https://github.com/globalsmile/Impact-of-Weather-Conditions-on-Air-Quality#Problem-Statement)
- [Data Sourcing](https://github.com/globalsmile/Impact-of-Weather-Conditions-on-Air-Quality#Data-Sourcing)
- [Background Study](https://github.com/globalsmile/Impact-of-Weather-Conditions-on-Air-Quality#Background-Study)
- [Data Preparation](https://github.com/globalsmile/Impact-of-Weather-Conditions-on-Air-Quality#Data-Preparation)
- [Data Modeling](https://github.com/globalsmile/Impact-of-Weather-Conditions-on-Air-Quality#Data-Modeling)
- [Data Visualization and Analysis](https://github.com/globalsmile/Impact-of-Weather-Conditions-on-Air-Quality#Data-Visualization)
- [Recommendations](https://github.com/globalsmile/Impact-of-Weather-Conditions-on-Air-Quality#Recommendation)
- [Shareable link](https://github.com/globalsmile/Impact-of-Weather-Conditions-on-Air-Quality#Shareable-Link)


---

# Introduction

A company in the environmental consulting industry is seeking to analyze the air quality in a specific city 
during hot and cold weather, during high-wind conditions and during precipitation. They are interested in 
making recommendations to the government and businesses in the region on how to mitigate the impact of weather 
conditions on air quality.

---

# Problem Statement

As a Data Analyst, you are expected to analyze the data provided, seek insights and make recommendations to achieve the set objectives.

Additionally, kindly use this dataset to analyze the historical impact of weather conditions on air quality, and make predictions on air
quality during specific weather conditions. This information could be used to inform emergency response plans and prepare for potential air quality issues.

PSA:This project uses an hourly data collection that includes PM2.5 data from the US Embassy in Beijing from January 2010 to December 2014 to assess Beijing's 
air quality and track the effects of weather and atmospheric conditions on the city's air quality.

More details can be found [here](https://techcommunity.microsoft.com/t5/educator-developer-blog/data-analysis-challenge-impact-of-weather-conditions-on-air/ba-p/3719570?WT.mc_id=academic-00000-ooyinbooke) on the Microsoft blog.

---

# Data Sourcing

The data set can be [found here](https://archive.ics.uci.edu/ml/datasets/Beijing+PM2.5+Data)

This data set has been sourced from the Machine Learning Repository of University of California, Irvine Beijing PM2.5 Data Set (UC Irvine).
The UCI page mentions the following publication as the original source of the data set:
Liang, X., Zou, T., Guo, B., Li, S., Zhang, H., Zhang, S., Huang, H. and Chen, S. X. (2015). Assessing Beijing's PM2.5 pollution: severity, 
weather impact, APEC and winter heating. Proceedings of the Royal Society A, 471, 20150257

---

# Background Study

According to [Department of Health, New York](https://www.health.ny.gov/environmental/indoors/air/pmq_a.htm#:~:text=Fine%20particulate%20matter%20%28PM2.5,hazy%20when%20levels%20are%20elevated.), Fine particulate matter (PM2.5) is an air pollutant 
that is a concern for people’s health when levels in air are high. PM2.5 are tiny particles in the air that reduce visibility and cause the 
air to appear hazy when levels are elevated.

Air Quality in this data set is determined by the level (Concentration in Ug/m3) of Particulate matter (PM2.5) in the atmosphere. 
According to [Breeze Technologies](https://www.breeze-technologies.de/blog/what-is-an-air-quality-index-how-is-it-calculated/), PM2.5 levels over 55Ug/m3 shows a poor level of air quality 
and above 110Ug/m3 shows a severe level of air quality. For this analysis, a limit of 100Ug/m3 was placed to signify that the air quality is getting to a severe level.

The bulk of the analysis is centered around how the concentration of PM 2.5 changes due to a change in the atmospheric condition.

---


# Data Preparation

Data cleaning and transformation was done in Microsoft Excel and the datasets were loaded into Microsoft Power BI Desktop for modeling and visualization.

The dataset contains `43,824 rows and 13 columns`. The data time period is between Jan 1st, 2010 to Dec 31st, 2014. Missing data are denoted as NA.

The data dictionary containing the features of the data can be found below:


| Fields | Description |
| ----------- | ----------- |
| No | row number |
| year | year of data in this row |
| month | month of data in this row |
| day | day of data in this row |
| hour | hour of data in this row |
| pm2.5 | PM2.5 concentration (ug/m^3) |
| DEWP |  Dew Point (â„ƒ) |
| TEMP | Temperature (â„ƒ) |
| PRES | Pressure (hPa) |
| cbwd | Combined wind direction |
| lws | Cumulated wind speed (m/s) |
| ls | Cumulated hours of snow |
| lr | Cumulated hours of rain |


The dataset was checked for duplicates, invalid entries, missing values, incorrect data types and issues from data entry.

The steps involved in the data cleaning is as follows:

1. I Created a `Date` column using the `year`, `month` and `day` column. After it was created, the datatype was corrected
2. I Handled the null in the `pm2.5` column
3. In the `cbwd` column, I replaced ‘cv’ with ‘SW’ representing the South West
4. The `PRES` column which is representing the atmospheric pressure is in Hecto-paschal. I converted the unit to atm (Standard unit for pressure) and saved it in a new column `atm_pressure` 
5. I Classified the months into four seasons
- Winter — December, January and February
- Autumn — September, October and November
- Spring — March, April and May
- Summer — June, July and August
6. I extracted the month name from the month number
7. I dropped the first column
- The first column doesn't have any use case in the analysis so the column was dropped.

---

# Data Modeling

After the dataset was cleaned and transformed, it was ready to be modeled(using Power BI Desktop).

Data modeling was done for this project in order to reduce redundancy and optimize the efficiency
of our analysis(report).

To do this the dataset was split into dimension and fact tables:
`calender` : dimension
`region` : dimension
`season` : dimennsion
`weather_data': fact


- A `one-to-many (*:1) relationship` was created between the `weather_data` and the `calender` tables using the `date` column in each of the tables
- A `one-to-many (*:1) relationship` was created between the `weather_data` and the `region` tables using the `cbwd` column in each of the tables 
- A `one-to-many (*:1) relationship` was created between the `weather_data` and the `season` tables using the `month_id` column in each of the tables 
- The realtioships formed in the data model is a `Star Schema` and is shown below:

<img align="right" alt="Data Model" width="1000" height = "400" src="https://user-images.githubusercontent.com/106287208/226214827-b7740300-9080-4665-8411-3be421e8633f.png">


---

# Data Visualization and Analysis

Data visualization was done using Microsoft Power BI Desktop.

Based on the business problem, the following report was prepared:

![image](https://user-images.githubusercontent.com/106287208/226215024-79292dce-7e52-44c4-a09c-87ddfb35edc3.png)


Measures used in visualization are:

- Average PM 2.5 (Ug/m3) = `AVERAGE(weather_data[pm2.5])`
- Average Pressure (atm) = `Average(weather_data[atm_pressure])`
- Average Temperature (C) = `AVERAGE(weather_data[TEMP])`
- Precipitation Time (Minutes) = `AVERAGE(weather_data[Ir])*60`
- Snow Time (Minutes) = `AVERAGE(weather_data[Is]`
- Wind Speed (km/h) = `(SUM(weather_data[Iws]) * (0.06))*60`


Note: 
From [Breeze Technologies](https://www.breeze-technologies.de/blog/what-is-an-air-quality-index-how-is-it-calculated/), The levels of PM2.5 have been grouped to determine the air quality status

- Excellent (0 - 7): According to current research, negative impacts on ecosystems are unlikely.
- Fine (7 - 15): All values are under the legal health protection limits. Effects on ecosystems can no longer be ruled out
- Moderate (15 - 30): The health protection limits are mostly still met. Effects on ecosystems are increasingly possible.
- Poor (30 -55): The measured values are at the level of health protection limit values. Health impairments of sensitive persons may occur sporadically.
- Very Poor (55 - 110): The health protection limits have been exceeded. Health impairments of sensitive persons are possible. The population is increasingly informed about the pollutant situation.
- Severe (110 >): The measured values are at alarming levels. The health protection thresholds are clearly exceeded. Health impairments of all persons are possible.

---

# Recommendations

Based on the analysis, I have some recommendations to make:

- Firstly, given the consistently high PM2.5 levels in the city, I strongly suggest that the government refer to the United States' National Action Plan
on Pollutant & Control. This plan aims to reduce PM2.5 concentrations by 20-30% above 2017 annual levels in over 100 cities through measures such as limiting car emissions, reducing reliance on coal, promoting 
renewable energy sources, and enforcing strict emissions regulations. 
- Secondly, it has been observed that when the wind blows in the south-west and south-east directions, PM2.5 levels tend to be high. 
Therefore, identifying the sources of pollution and taking steps to mitigate them can help reduce PM2.5 concentrations. 
- Thirdly, during the winter months, PM2.5 concentrations tend to increase rapidly due to the extensive use of coal and other fossil fuels for heating purposes. To tackle this, 
I recommend that households using biomass fuels like wood, animal dung, and crop wastes, or coal for cooking and heating, should switch to cleaner fuels like biogas, 
liquid petroleum gas (LPG), electricity, or solar cookers wherever possible. 
- Finally, the data analysis revealed that high levels of rain and snow are associated with low PM2.5 levels. While rain and snow can wash particulate matter out of the air and destroy pollutants, 
it is important to note that these pollutants are not eradicated but simply relocated to other areas, such as bodies of water. 
Therefore, individuals with sensitive health should limit their outdoor activities during times of high PM2.5 levels to reduce their exposure.

---

# Shareable Link

You can interact with the report here: 

[View Report](https://app.powerbi.com/view?r=eyJrIjoiNzJjNGUxOGItYjUwNC00NWI3LTkyNDUtMzEzNDJkYWViNWNkIiwidCI6IjQ5ODY4YWYzLWNjNWYtNDIxNC04YjdmLTQwZjM3NDY0OWEwOSJ9)
