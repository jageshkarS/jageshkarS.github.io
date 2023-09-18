---
title: |
  Cyclistic Data Analysis
  <img src="G:/My Drive/Data_analytics/Capstone/logo.png" alt="Alt text" width="90" height="90"> <br>
author: "Jageshkar"
date: "2023-09-12"
output:
  html_document:
    css: style.css
    theme: cerulean
  pdf_document: default
affiliation: "Cyclistic bike-share"
---

## Contents

- [Executive Summary](#summary)

- [Introduction](#intro)

- [Business Task](#business-task)
  - [Stakeholders](#stakeholders)

- [Data Description](#data-description)
  - [Source](#source)
  - [Format](#format)
  - [Data Integrity](#data-intergrity)

- [Pre-process](#pre-process)
  - [Data cleaning](#data-cleaning)
  - [Data transformation](#data-transformation)

- [Data Analysis](#data-analysis)
  - [Uploading to BigQuery](#uploading-to-bigquery)
  - [Combining the data](#combining-the-data)
  - [Creating temporary tables from the combined table](#temp-table)

- [Visuals](#visuals)

- [Findings](#findings)

- [Conclusions](#conclusions)

- [Recommendations](#recommendations)

- [Appendices](#appendix)


## <a id="summary"></a> Executive Summary

The following report provides a high-level overview of the key findings from the data analysis conducted on Cyclistic Bike Sharing data in the year 2022.

In 2022, distinct patterns in rider behaviour and preferences among annual members and casual riders were observed using data from Cyclistic bike rides. Notably, almost 60% of riders were annual members during the year, suggesting a strong commitment to the system. Ridership peaked throughout the summer, with both casual riders and annual members actively engaging. While casual riders tended to shun winter cycling, many annually members continued to ride their bikes during the colder months, demonstrating their loyalty to the program. Casual riders, on the other hand, preferred longer journeys as they spent more time cycling than annual members.

Annual members preferred classic bicycles and electric bikes, when it came to bike selection and avoided docked bicycles. Overall, docked bicycles were the favoured choice, resulting to casual riders having a longer average riding duration due to their extended use. The usage trends also demonstrated a contrast between weekdays and weekends, with annual members riding more on weekdays and casual riders riding more on weekends. Finally, peak riding hours for both groups were consistently recorded in the late afternoon, whereas early morning hours witnessed the least number of cyclists.

We offer numerous strategies for Cyclistic to leverage on these insights. To begin, Cyclistic could consider seasonally optimizing bike availability, with a focus on allocating more bikes, particularly electric bikes, during the summer months to satisfy rising demand. During the winter, Cyclistic can consider restricting bike availability while offering seasonal specials to encourage annual members to continue utilizing the service in colder weather.

Secondly, given that casual riders prefer longer rides, Cyclistic should implement a membership diversification strategy. This could include the implementation of a tiered membership structure that appeals to both categories, such as a "Casual Plus" membership with extended ride durations or packaged price for infrequent users. This strategy would attract more casual riders to join and use the service more regularly.

Finally, to encourage annual members to utilize docked bicycles, Cyclistic should launch targeted promotional efforts and instructional programs. Cyclistic can encourage the use of docked bikes among its annual members by emphasizing the benefits of docked bikes, such as guaranteed availability and lower maintenance, and by offering incentives such as discounted prices for docked bike usage.

These recommendations aim to enhance Cyclistic's ability to provide to the diverse needs and preferences of both annual members and casual riders, optimize bike availability throughout the year, and potentially increase overall ridership and revenue.

## <a id="intro"></a> Introduction

Cyclistic bike share is a fictional bike sharing company in Chicago. The main goal of the company is to convert as much casual customers into annual members. Casual customers are people who pay on single-ride pass or daily basis and annual members as the name suggests pay yearly. The goal was split into three sub-goals: how members and casual riders use the bicycles differently, why would casual riders buy annual memberships and how the company can use digital media to influence casual riders to become members. This analysis focuses on the first sub-goal: how members and casual riders use the bicycles differently.

The data was provided by the company from this [database](https://divvy-tripdata.s3.amazonaws.com/index.html), which contains rider data from 2013 till present year. To use the most recent data, January 2022 to December 2022 data was obtained.

## <a id="business-task"></a> Business Task
<p>Identify how annual members and casual riders use Cyclistic bicycles differently 
by conducting an in depth analysis of historical bike trip data. From the analysis
identify any methods which can encourage casual riders to transition into annual
members.</p>

### <a id="stakeholders"></a> Stakeholders
- Lily Moreno: Director of marketing
- Cyclistic executive team: Team who approves the recommended marketing program

## <a id="data-description"></a> Data Description
### <a id="source"></a> Source
<p>The data originates from, Motivate International Inc.(now known as Motivate LLC).
The database consists of cycle data spanning from the 2013 to July 2023. For this analysis,
the most recent full year of data was obtained, which is from January 2022 until December 2022.</p> 
#### <a id="format"></a> Format
<p>This dataset is presented in CSV (comma-separated values), with a separate CSV file for each month when the cycle was unlocked. Even if the return date falls in a different month than the unlock date, the corresponding data row is stored in the file which belongs to the month when the cycle was first unlocked.</p>
<p>The raw data for each month consists of 13 columns which are:</p>
  1) **ride_id:** A unique id representing each time a bicycle was unlocked
  2) **rideable_type:** There are 3 categories of cycles: docked, electric and classic. 
  3) **started_at:** Date and time when the bicycle was unlocked
  4) **ended_at:** Date and time when the bicycle was returned
  5) **start_station_name:** Name of the station where the bicycle was unlocked
  6) **start_station_id:** The unique identifier for each corresponding starting station
  7) **end_station_name:** Name of the station where the bicycle was returned
  8) **end_station_id:** The unique identifier for each corresponding end station
  9) **start_lat:** Latitude of start station.
  10) **start_lng:** Longitude of start station. 
  11) **end_lat:** Latitude of end station.
  12) **end_lng:** Longitude of end station.
  13) **member_casual:** If the user is an annual member or casual rider.
  
#### <a id="data-integrity"></a> Data Integrity
<p>The reliability of this dataset is emphasized by its origin from Motivate LLC, a prominent bike-sharing system in North America. This direct source connection ensures the data's authenticity and originality. Furthermore, because the data is for the year 2022, its currency is guaranteed, and it's worth noting that Motivate LLC regularly updates its dataset, extending coverage up to July 2023 at the time this project is being executed.</p>
## <a id="pre-process"></a>  Pre-process
### <a id="data-cleaning"></a> Data Cleaning
In the pre-process stage, the first step involved cleaning the data. Cleaning the data ensures that the raw data can be reliable for usage. This consisted several key tasks, including:

- ##### Identifying and eliminating duplicates by removing redundant entries
<p>The data cleaning process was carried out individually for each month in Microsoft Excel. The first step in the data cleaning technique was to identify and remove any duplicate data rows. This work was completed by using Excel's "Remove Duplicates" tool. It's worth noting that no duplicate records were identified in any of the months during this process.</p>

- ##### Detecting any instances of missing data and subsequently resolving them
<p>Following the evaluation of duplicate entries, the subsequent step involved identifying and correct missing data. This was achieved by utilizing the filter function within Microsoft Excel to pinpoint empty cells which were found in the columns for start station name, start station id, end station name, and end station id.</p>
<p>To determine which station names and IDs were missing, latitude and longitude data were cross-referenced with rows containing station names and IDs. However, during this process, it was discovered that some stations shared the same latitude and longitude coordinates, while others had multiple sets of latitude and longitude values associated with them. Faced with the unreliability of this source, a decision was made to eliminate all rows containing blank cells for both start and end station names.</p>

- ##### Conducting an in-depth review of the dataset to identify and correct any spelling errors or inconsistencies.
<p>Microsoft Excel's built-in spell-check capability was used to verify the accuracy of non-noun words, specifically checking the spellings of terms like "casual" and "member" under the member_casual column. This vast and comprehensive spell-check technique produced no spelling errors, confirming the accuracy of these terms inside the data.</p> 
<p>Following that, the focus shifted to checking street names for potential spelling problems and inconsistencies. Excel's sorting and filtering tools proved useful for this task. Using these methods, any irregularities in the data could be identified.</p>
<p>One type of inconsistency that was explored involved station names and their associated IDs. A search was carried out using the filter function to see if any stations had duplicate IDs. Fortunately, the investigation revealed that each station constantly had only one related ID, demonstrating data consistency.</p>
<p>Another aspect of data integrity checked for was the length of the ride IDs.These IDs are meant to be made up of exactly 16 characters. Excel's data validation tool was used to ensure that all ride IDs met this character length criteria. The findings revealed that all ride IDs were of the same length (16 characters), assuring data consistency and complying to the desired format.</p>

### <a id="data-transformation"></a> Data transformation
To make better sense of the data it needed to be transformed. Transforming data processes included: converting the data into the correct format, aggregation and differencing.

The started_at and ended_at columns were in a date time format "dd/mm/yyyy hh:mm". To determine the duration of each bicycle ride accurately, the time difference between "ended_at" and "started_at" was calculated. This approach, however, caused problems for trips lasting more than 23 hours and 59 minutes hence the data was converted to "English(United States) 37:30:55" format.

To provide better understanding of the ride time, all the time difference was converted to minutes using the formula:
```{}
=MINUTE("...") + HOUR("...")*60 + SECOND("...")/60 + DAY("...") * 24 * 60
```


Next, to determine the day of the week when the bicycle was rented, the date and time needed to be separated. This was achieved through the following steps:

1. The date was extracted using the formula:

```{}
=DATEVALUE(TEXT(..., "mm/dd/yyyy"))
```

2. With the date in hand, the day of the week when the bicycle was unlocked was determined using the formula:

```{}
=WEEKDAY(..., 2)
```

This formula produced integers ranging from 1 to 7, where 1 represented Monday, 6 represented Saturday, and 7 represented Sunday. These steps were applied to data across all months.

The time of day when the bicycle was unlocked was determined after finding the day. This was accomplished by doing the following steps:

1. The time was separated from the "started_at" column using the formula:

```{}
=TIMEVALUE(TEXT(..., "hh:mm"))
```
    
2. The following formula was employed to categorize the time of day into specific periods:
```{}
=IFS((J2 >= TIME(0,0,0)) * (J2 < TIME(3,0,0)), "Midnight", (J2 >= TIME(3,0,0)) * (J2 < TIME(6,0,0)), "Early Morning", (J2 >= TIME(6,0,0)) * (J2 < TIME(9,0,0)), "Morning", (J2 >= TIME(9,0,0)) * (J2 < TIME(12,0,0)), "Late Morning", (J2 >= TIME(12,0,0)) * (J2 < TIME(15,0,0)), "Afternoon", (J2 >= TIME(15,0,0)) * (J2 < TIME(18,0,0)), "Late Afternoon", (J2 >= TIME(18,0,0)) * (J2 < TIME(21,0,0)), "Evening", (J2 >= TIME(21,0,0)) + (J2 < TIME(24,0,0)), "Night", TRUE, "Invalid Time")
```

In this formula, time intervals of three hours were designated, and any wrong time values were labeled as "Invalid Time." Fortunately, there were no incorrect time entries in this dataset.

## <a id="data-analysis"></a> Data Analysis

### <a id="uploading-to-bigquery"></a> Uploading to BigQuery

In order to perform a more in-depth study of the data, SQL was used to merge all of the CSV files into a single table holding data for the year 2022. However, during the upload process, a hurdle emerged when some of the files topped 100 MB in size, making direct upload unfeasible. 

To address this issue, a strategic decision was made to optimize the data before uploading it. Columns deemed unimportant for the analysis, which include latitude and longitude values, were eliminated from the dataset. This simplified the data and decreased its total size.

The dataset was successfully uploaded into BigQuery after this data reduction stage, providing a more manageable and focused dataset for the remaining analytical activities. 

#### <a id="combining-the-data"></a> Combining the data

The data from all the months were then combined:

```{sql eval=FALSE}
WITH combined_table AS 
(SELECT * FROM `data-project-060723.Cyclistic_data_2022.April_2022`
UNION ALL SELECT * FROM `data-project-060723.Cyclistic_data_2022.August_2022`
UNION ALL SELECT * FROM `data-project-060723.Cyclistic_data_2022.December_2022` 
UNION ALL SELECT * FROM `data-project-060723.Cyclistic_data_2022.February_2022` 
UNION ALL SELECT * FROM `data-project-060723.Cyclistic_data_2022.January_2022` 
UNION ALL SELECT * FROM `data-project-060723.Cyclistic_data_2022.July_2022` 
UNION ALL SELECT * FROM `data-project-060723.Cyclistic_data_2022.June_2022` 
UNION ALL SELECT * FROM `data-project-060723.Cyclistic_data_2022.March_2022` 
UNION ALL SELECT * FROM `data-project-060723.Cyclistic_data_2022.May_2022` 
UNION ALL SELECT * FROM `data-project-060723.Cyclistic_data_2022.November_2022` 
UNION ALL SELECT * FROM `data-project-060723.Cyclistic_data_2022.October_2022` 
UNION ALL SELECT * FROM `data-project-060723.Cyclistic_data_2022.September_2022`),
new_table AS (
  SELECT * FROM `data-project-060723.Cyclistic_data_2022.April_2022_with_time`
  UNION ALL 
  SELECT * FROM `data-project-060723.Cyclistic_data_2022.August_2022_with_time`
  UNION ALL 
  SELECT * FROM `data-project-060723.Cyclistic_data_2022.December_2022_with_time`
  UNION ALL
  SELECT * FROM `data-project-060723.Cyclistic_data_2022.February_2022_with_time`
  UNION ALL 
  SELECT * FROM `data-project-060723.Cyclistic_data_2022.January_2022_with_time`
  UNION ALL
  SELECT * FROM `data-project-060723.Cyclistic_data_2022.July_2022_with_time`
  UNION ALL 
  SELECT * FROM `data-project-060723.Cyclistic_data_2022.June_2022_with_time`
  UNION ALL 
  SELECT * FROM `data-project-060723.Cyclistic_data_2022.March_2022_with_time`
  UNION ALL
  SELECT * FROM `data-project-060723.Cyclistic_data_2022.May_2022_with_time`
  UNION ALL
  SELECT * FROM `data-project-060723.Cyclistic_data_2022.November_2022_with_time`
  UNION ALL 
  SELECT * FROM `data-project-060723.Cyclistic_data_2022.October_2022_with_time`
  UNION ALL 
  SELECT * FROM `data-project-060723.Cyclistic_data_2022.September_2022_with_time`
)

```

- The eventual table was around 500 MB, hence it was uploaded into a Google drive.
- The time of day was done in a separate csv file because in BigQuery free trial version columns could not be added.

#### <a id="temp-table"></a> Creating temporary tables from the combined table

A table was created containing the mean ride time, total ride time and maximum ride time grouped by month, casual/member and day of the week.

```{sql eval=FALSE}
SELECT 
  combined_table.month,
  combined_table.member_casual,
  COUNT(combined_table.ride_id) AS number_of_riders,
  MAX(combined_table.time_difference) AS maximum_ride_time,
  combined_table.day_of_week,
  AVG(combined_table.time_difference) AS average_ride_length
FROM combined_table 
GROUP BY day_of_week, member_casual, month
```

The data was filtered to give the total number of riders, average ride time and maximum ride time for casual riders and annual members.

```{sql eval=FALSE}
SELECT 
  combined_table.member_casual,
  COUNT (*) AS number_of_riders,
  MAX(combined_table.time_difference) AS maximum_ride_time,
  AVG(combined_table.time_difference) AS average_ride_length
FROM combined_table
GROUP BY member_casual
```

The data was filtered to find out which customers use which type of bikes.

```{sql eval=FALSE}
SELECT
  combined_table.rideable_type,
  combined_table.member_casual,
  COUNT(*) AS number_of_riders
FROM combined_table
GROUP BY rideable_type, member_casual
```

A summary table was created containing the details of the number of riders and mean ride time grouped by the time of day, month, casual or member and day of the week.

```{sql eval=FALSE}
SELECT 
  combined_table.rideable_type,
  new_table.time_of_day_start,
  combined_table.member_casual,
  AVG(combined_table.time_difference) AS mean_ride_length,
  combined_table.day_of_week,
  combined_table.month,
  COUNT(combined_table.ride_id) AS number_of_riders
FROM combined_table
JOIN new_table ON combined_table.ride_id = new_table.ride_id
GROUP BY rideable_type, time_of_day_start, day_of_week, month, member_casual
```

### <a id="visuals"></a> Visuals

Using the temporary and combined tables which were created using SQL, visualizations were created using Tableau Public.

- #### Total Customers

<img src="G:/My Drive/Data_analytics/Capstone/Case Study/Visuals/count-of-customers-by-type.png"  width=50% height=50%>

In the year 2022, 59.76% of the customers were annual members while the rest 40.24% were casual riders.

- #### Riders per Month

<img src="G:/My Drive/Data_analytics/Capstone/Case Study/Visuals/riders-per-month.png">

Peak ridership for both casual and yearly members occurred during the summer months of June, July, and August in 2022. Notably, casual riders were the most active in July, while annual members were the most active in August. In contrast,the winter months of December, January, and February experienced the lowest ridership levels from both casual riders and annual members.

There is a significant difference in rider distribution between the seasons. Specifically, there were 300% more annual members than casual members throughout the winter months. During the summer, however, the gap became substantially smaller, at only 13.8%.

- #### Ride length

<img src="G:/My Drive/Data_analytics/Capstone/Case Study/Visuals/average-ride-length.png" width=40% height=40%>

The data analysis found a substantial variance in average ride durations between casual riders and members. Casual riders, in particular, spent nearly twice as much time on bicycles as members. On average, casual riders logged approximately 22 minutes per ride, while members averaged around 12 minutes per ride.

- #### Type of bicycle

<img src="G:/My Drive/Data_analytics/Capstone/Case Study/Visuals/type-of-bike.png" width=80%>

The bicycles are divided into three categories: classic, docked, and electric. The preference among annual members is for classic bikes over electric bikes. Notably, annual members have not utilized docked bikes at all.

Annual members use classic bikes twice as much as electric bikes, casual riders prefer classic bicycles to electric bicycles, albeit the difference is less evident. Remarkably unlike annual members, casual riders have used docked bikes, despite the fact that it is the least used category of all bike kinds.

One noticeable trend emerges in terms of bike ride duration. Riders spend much longer time on docked bikes, averaging nearly 60 minutes per ride. Because casual riders are the predominant users of docked bikes, the longer ride duration on docked bikes may be a contributing factor to the observed data suggesting that casual riders spend more time riding on average.

When examining the average ride duration of docked bikes, however, classic bikes are used for fairly longer time than electric bikes.

- #### Riders per day

<img src="G:/My Drive/Data_analytics/Capstone/Case Study/Visuals/riders-per-day.png">

On the chart above the "number" on day of the week represents Monday to Sunday as 1 to 7. On weekdays, it is clear that there are more annual members riding bicycles than casual riders. The peak number of annual members using bicycles occurs on Tuesdays, Wednesdays, and Thursdays, with a decline observed after Thursday. Tuesdays, on the other hand, had the lowest number of casual riders unlocking bicycles.

Analysis shows that on weekends, more casual riders choose "Cyclistic" bicycles over annual members. The peak number of casual riders can be observed on Saturdays, and on Sundays, the number of casual riders is similar to that of annual members.

The difference between annual members using bicycles on weekdays is more significant when compared to casual riders, whereas on weekends, the gap between casual riders and annual members is less pronounced.

- #### Time of the day preference 

<img src="G:/My Drive/Data_analytics/Capstone/Case Study/Visuals/time-preference-for-casual-riders-vs-annual-members.png" width=70% height=70%>

The 24 hour day period is divided into 8 segments:

- **Midnight** : 00:00 to 02:59
- **Early Morning** : 03:00 to 05:59
- **Morning** : 06:00 to 08:59
- **Late Morning** : 09:00 to 11:59
- **Afternoon** : 12:00 to 14:59
- **Late Afternoon** : 15:00 to 17:59
- **Evening** : 18:00 to 20:59
- **Night** : 21:00 to 23:59

According to the data, the majority of cyclists, both casual and annual members, start their rides in the late afternoon. The early morning period, on the other hand, has the lowest ridership for both casual riders and annual members. However, as the day advances into the night, both groups of riders have identical numbers. Surprisingly, the number of casual riders was more than the number of annual members only at midnight. This pattern implies that riders have a preference, with a noteworthy spike in late afternoon riders.

- #### Day of week and time of day

<img src="G:/My Drive/Data_analytics/Capstone/Case Study/Visuals/time-of-the-day-riders-prefer-on-each-day-of-the-week.png" width=70% height=70%>

It is well-established that the majority of casual riders choose bicycles on weekends, whilst annual members prefer them on weekdays. Among casual riders, those who ride in the morning have higher daily ridership but a lower weekend ridership. In other cases, Saturday has the largest peak in ridership, followed by a dip on Sunday.

Interestingly, the behavior of annual members deviates from the regular pattern during late morning rides. Rather than peaking during the week and dropping on weekends, their numbers peak on Saturday, with increased riding on weekends.A similar trend is observed for annual members riding at midnight.

Casual riders who start their rides in the afternoon experience a significant surge in numbers during the weekends, similar to those who prefer late afternoon. This pattern is shared by members, with the main exception being a significant fall in the number of riders choosing for late afternoon rides.

### <a id="findings"></a> Findings

- In the year 2022, nearly 60% of the riders are annual members.
- Peak ridership occurred during the summer months of June, July and August for both casual riders and annual members.
- Rarely casual riders are seen using bicycles in the winter but more than a few annual members use bicycles during the winter.
- Casual riders spend more time riding bicycles compared to annual members.
- Annual members do not use docked bicycles, both casual and annual riders mostly use classic bicycles followed by electric bikes.
- Riders spend the most time on docked bicycles which is why casual riders have the higher average time. 
- During the weekdays significantly more members use the bicycles and during the weekends slightly more casual riders ride more than the members.
- Both casual riders and members peak during the late afternoon and the least number of riders are seen during early morning.

### <a id="conclusions"></a> Conclusions

- A greater proportion of people who use "Cyclistic" bicycles are annual members.
- Both casual riders and annual members use the bicycles most during the summer indicating that they use the bicycles for leisure mostly during the warmer months.
- During the winter more annual members are seen riding but very few casual riders. This shows members depend on bicycles for all year commuting.
- Casual riders on average, spend more time riding bicycles compared to annual members. This might suggest that casual riders utilize the bikes for longer journeys or leisurely rides, whereas annual members use them for shorter, more practical journeys.
- Annual members do not utilize docked bicycles, and both casual and annual riders choose classic bicycles over electric bikes. This suggests a common preference for classic and electric bikes, with annual members avoiding docked bikes entirely.
- Significantly more members utilize the bicycles during the weekdays, indicating that annual members may rely on the service for daily commuting or work-related trips. On weekends, the bikes are used by significantly more casual users, reflecting that they prefer to use the service for recreational activities in their spare time.

In summary, the seasons, winter riding, ride duration, bike type preferences, and daily riding patterns differ between annual members and casual riders. Annual members appear to rely on the service for everyday transportation, whilst casual users use it for leisure and amusement.

### <a id="recommendations"></a> Recommendations

1) **Seasonal bike availability : ** Given the peak ridership during the summer months and the decline in casual ridership during the winter, Cyclistic should consider maximizing their bike availability. In order to meet the increased demand throughout the summer, allocate more bikes, particularly electric bikes. In contrast, reduce bike availability or even give seasonal specials throughout the winter to encourage annual members to continue utilizing the service in colder weather.

2) **Membership representation : ** Recognizing that casual riders prefer longer journeys, Cyclistic could introduce a tiered membership scheme that appeals to both groups. For example, they may provide a "Casual Plus" membership with longer ride durations or bundled price for infrequent riders. This would encourage more casual riders to join the service and utilize it more regularly.

3) **Promotion and Education on Docked Bikes : ** Annual members show a reluctance to use docked bicycles. Cyclistic can execute targeted promotional efforts and educational initiatives that encourage annual members to try docked bikes. Highlight the benefits of docked bikes, such as guaranteed availability and lower maintenance, to attract more users to this bike type. To encourage usage, incentives such as lower fees for docked bike usage should be offered.

## <a id="appendix"></a> Appendices

#### Dashboard

The dashboard of the data visuals can be found in the below links: 

+ [Dashboard_1](https://public.tableau.com/views/Capstone_project_16941720817680/Countof?:language=en-US&:display_count=n&:origin=viz_share_link)

+ [Dashboard_2](https://public.tableau.com/views/Capstone2_16944462221580/Dashboard1?:language=en-US&publish=yes&:display_count=n&:origin=viz_share_link)

#### Gant Chart

```{r echo=FALSE}
library(ggplot2)
tasks <- data.frame(
  Task = c("Choose project", "Data collection", "Data Cleaning", "Data Analysis", "Create visualization", "Come up with recommendations", "Write data analysis report"),
  Start_Date = as.Date(c("2023-09-12", "2023-09-13", "2023-09-13", "2023-09-14", "2023-09-15", "2023-09-16", "2023-09-13")),
  End_Date = as.Date(c("2023-09-13", "2023-09-14", "2023-09-14", "2023-09-15", "2023-09-16", "2023-09-18", "2023-09-18"))
)
ggplot(tasks, aes(x = Start_Date, xend = End_Date, y = Task, yend = Task)) +
  geom_segment(linewidth = 10, color = "#4095A5") +
  labs(title = "Gantt Chart", x = "Timeline") +
  scale_x_date(date_breaks = "1 day", date_labels = "%b %d") +
  theme(panel.grid.major = element_line(color = "gray", linetype = "solid"))
```




