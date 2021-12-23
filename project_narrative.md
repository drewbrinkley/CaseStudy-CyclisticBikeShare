# Case Study: How Does a Bike-Share Navigate Speedy Success?

## Contents
1. [Overview](#overview)
1. [Defining the Problem](#defining-the-problem)
1. [Preparing the Data](#preparing-the-data)
1. [Cleaning and Processing the Data](#cleaning-and-processing-the-data)
1. [Analyzing the Data](#analyzing-the-data)
1. [Sharing the Analysis](#sharing-the-analysis)
1. [Taking Action](#taking-action)


## Overview

I completed this case study as my capstone project for the Google Data Analytics Professional Certificate.  This was one of the predefined, structured examples set out in the course of study.  I selected this case study because it modeled a real-world example of the types of tasks a Data Analyst may be assigned.  What follows is a description of my thought process as I worked through this project.  

### Scenario
In this case study, I assumed the role of a Junior Data Analyst working as a member of the marketing analyst team at Cyclistic, a fictional bike-share company in Chicago, Illinois.  The director of the marketing department charged my team with develping a new marketing strategy to convert casual riders into annual members.  She based this upon the belief that the company's future is contingent upon maximizing annual memberships. 

To that end, my assignmment entailed examining the previous 12 months of historical data to identify how annual members and casual riders used Cyclistic's bike share differently.  I also needed to provide my top three recommendations based on this analysis.  They key stakeholders in this scenario include:  the director of marketing, the marketing analyst team, and the executive team. 

## Defining the Problem

Since the goal of the new marketing strategy centered on converting casual riders into annual members, I first defined how Cyclistic categorizes its customers into these groups.  The company's offical definition of these groups is as follows:
* Members:  Cyclistic customers who purchase annual memberships
+ Casual Riders:  customers who purchase single-ride or full-day passes
However, I needed to drill down more into the casual ridership segment to identify a subsegment worthy of targeting for the proposed marketing strategy.  

I assumed that some casual riders reside in the Cyclistic service area whereas others are Chicago-area visitors who decided to rent bikes for transportation or excursions when touring the city.  Of these groups, there are likely riders who are one-time or infrequent riders as well as customers who rent bikes multiple times during the year.  The slice of the casual rider group that should be the focus of the marketing efforts includes those riders who live in the Chicago area and rent bikes multiple times a year.
![Casual Ridership:  Target Group](/images/member_flowchart.png) 

I also created a Venn diagram to help visualize the different segments of Cyclistic's customer base.  It can be divided into three broad segments:
1. Members whose riding habits and preferences differ from those of casual riders
1. Casual riders whose riding habits and preferences differ from those of members
1. Casual riders whose riding habits and preferences mirror those of members, but who have not elected to purchase annual memberships
![Venn Diagram of Membership Types](/images/venn_diagram.png)

### Limitations of the Data
Unfortunately, the Cyclistic data lacks detail in a couple of key areas beneficial in informing new marketing efforts.  
1. There is no information to identify customers.  I have no idea where a rider lives in relation to the service area, nor do I know if the rider is a repeat customer.
1. The data has no information to describe the reason for each rental.

### Assumptions
I made a couple of assumptions when completing my analysis.  
1. Ride durations of less than one (1) minute were excluded.  They may indicate a user re-docking a bike to verify its security during return, or they may indiacte a "false start" and not an actual ride.
1. 	The analysis should not include rides with a type "docked."  This classification represents a rental parked at one of the Cyclistic bike stations throughout the service area, though the user will be returning to the bike before completing their ride.  The additional trip(s) will be counted as new rides.
_Note:  These issues would be things that would be discussed with my manager / project leader to determine the best approach for handling them.  Since this is a hypothetical example, I chose to deal with them in the manner described._

## Preparing the Data

### Reviewing Credibility
Since Cyclistic is a fictional company, the project used actual [data](https://divvy-tripdata.s3.amazonaws.com/index.html) from the City of Chicago's Divvy bicycle sharing service as an analogue for the company's data sets.  This is public data made available by Motivate International Inc. as part of a [license agreement](https://ride.divvybikes.com/data-license-agreement).  The 12-month period from November 2020 to October 2021 comprised the data sets used for this analysis.

In this case study, the data on bike ridership was first party data since it was collected and owned by Cyclistic.  The data was internal, being maintained within Cyclistic's own system, and structured into tables that documented a complete history of rentals.  As such, there were no issues of data bias or credibility with which to contend. 

### Previewing for Organization and Completeness
Each month's data was contained in a ZIP file, which I downloaded to my local machine for initial review, transformation, and cleaning.  In accorandance with standard data practices, I maintained original copies of each file in a dedicated directory.  I saved working versions of the files in a separate folder to make sure the orignial data was uncompromised in case it needed to be restored or reviewed.  I kept a log of all changes made to the "working" versions of the files.

I previewed the monthly data to get an idea of its organization.  The structure of the tables showed data on individual rides that occurred during the calendar month; data for each ride was grouped into rows.  The columns recorded the following variables:
* Ride ID:  a unique alphanumeric identifier for each ride (a.k.a trip)
- Ride Type:  descriptor of the type of bike rented
- Start Time:  included date and time of the rental's start 
- End Time:  included date and time of the rental's end
- Starting Location:  four columns recorded the station name, alphanumeric identifier, latitude, and longitude of the rental location
- Ending Location:  four columns recorded the station name, alphanumeric identifier, latitude, and longitude of the rental's return location
- Membership Type:  recorded whether the customer was a member or casual rider
A cursory review of the data revealed that a significant number of records lacked information in the columns dedicated to the station names and identifiers for starting and ending locations.  I knew I would need to deal with these missing values when I began to clean the data.  I also observed records with a ride type of "docked" that required exclusion.

### Including Additional Data
As part of the analysis, I wanted to explore how holidays affected various facets of Cyclistic ridership.  However, I ascertained that there was no information specific to holidays contained in the monthly data.  I created a spreasheet that listed the names and dates of the federal holidays that fell within the timeframe of this study to use later in the analysis.

## Cleaning and Processing the Data

After previewing the data sets, I realized that the full scope of analysis required the use of tools other than spreadsheets due to the size of the uncompressed files.  However, I used Excel to begin the data cleaning process for a couple reasons:  the files were already in comma separated values (CSV) format which was easy to preview in a spreadsheet and the size of multiple files was exceeded import parameters of other cloud-based tools (for example, BigQuery's Sandbox limits file importation to maximum of 100 MB).

### Data Cleaning with Spreadsheets
For each of the monthly data sets, I completed the following actions to clean and verify the data:
* Performed "Remove Duplicates" action to ensure each row contained unique records.  Duplicates were deleted.
- Reviewed the minimum and maximum values for the rental start date to verify each month's data only included rentals that occurred in that month.
- Reviewed the minimum and maximum values for the rental return date to verify each month's data only included rentals that occurred in that month.  In some instances, the latest return date occurred during the following month, which was acceptable.
- Calculated the ride length (ride ending time - ride starting time) for each record.  Saved these results in a new column.
- Determined the minimum ride length for each rental.  Deleted any records with negative values since it is impossible for a ride to end before it starts.
- Reviewed the minimum and maximum values for all latitude and longitude coordinates to make sure that data did not contain outliers.
- Added new column to display the day of rental.  Calculated this from the rental start date using the WEEKDAY() function, which returned a numeric value representing the day of the week.  Used nested IF() statement to convert value into a text string.
- Deleted the columns with station names and station identifiers for starting and ending locations.
	- As noted previously, a substantial number of records had "null" values for these columns, rendering these fields of little use for analysis.
	- Each rental record already included latitude and longitude coordinates for these locations.
	- File sizes necessitated reduction to comply with limitations of other analysis tools.

### Data Cleaning with SQL
Upon completing the preliminary on the speadsheets, I saved each month's data as a .CSV file and imported them into BigQuery to utilize SQL for further investigation.  I used a UNION ALL statement to combine the monthly data into a complete data set for the period of the analysis.  
<detail>
<summary>Show SQL</summary>
```sql
/*UNION ALL statement to combine the complete records for each month into a single, comprehensive table sorted by date of rental in ascending order*/
SELECT
	*
FROM 
	cyclistic_trip_data.nov_2020_data
UNION ALL 
SELECT
	*
FROM 
	cyclistic_trip_data.dec_2020_data
UNION ALL 
SELECT
	*
FROM 
	cyclistic_trip_data.jan_2021_data
UNION ALL 
SELECT
	*
FROM 
	cyclistic_trip_data.feb_2021_data
UNION ALL 
SELECT
	*
FROM 
	cyclistic_trip_data.mar_2021_data
UNION ALL 
SELECT
	*
FROM 
	cyclistic_trip_data.apr_2021_data
UNION ALL 
SELECT
	*
FROM 
	cyclistic_trip_data.may_2021_data
UNION ALL 
SELECT
	*
FROM 
	cyclistic_trip_data.jun_2021_data
UNION ALL 
SELECT
	*
FROM 
	cyclistic_trip_data.jul_2021_data
UNION ALL 
SELECT
	*
FROM 
	cyclistic_trip_data.aug_2021_data
UNION ALL 
SELECT
	*
FROM 
	cyclistic_trip_data.sep_2021_data
UNION ALL 
SELECT
	*
FROM 
	cyclistic_trip_data.oct_2021_data
UNION ALL 
ORDER BY
	started_at;
```
</detail>
I continued my use of SQL, as that tool was the best fit for a large table of the size created by the combined data sets.

I then executed several queries to obtain summary statistics on this data set, which I used to verify subseuqent data cleaning steps returned the expected results.  
* I queried the table to determine the total number of riders, as determined by counting the number of records in the ride_id column.
<detail>
<summary>Show SQL</summary>
```sql
SELECT
	COUNT(bike.ride_id) AS num_rides
FROM
	`cyclistic_trip_data.bike_data_combined` AS bike
```
</detail>
- I queried the number of riders, grouped by the member_type column, to verify that this sum matched the total above and ensure no values other than "casual" or "member" appeared in the data.
<detail>
<summary>Show SQL</summary>
```sql
SELECT
	COUNT(bike.ride_id) AS num_rides,
	bike.member_casual AS member_type
FROM
	`cyclistic_trip_data.bike_data_combined` AS bike
GROUP BY
	member_type;
```
</detail>
- I queried the number of riders of each group having a "docked" ride type.
<detail>
<summary>Show SQL</summary>
```sql
SELECT
	COUNT(bike.ride_id) AS num_rides,
	bike.member_casual AS member_type
FROM
	`cyclistic_trip_data.bike_data_combined` AS bike
WHERE 
	bike.rideable_type = "docked"
GROUP BY
	member_type;
```
</detail>
- I queried the number of riders of each group with a ride time of less than one minute.
<detail>
<summary>Show SQL</summary>
```sql
/*The ended_at and started_at columns have date and time data formatted as YYYY-MM-DD HH:MM:SS.  TIMESTAMP_DIFF allowed calculatation of the difference between these fields and returned the result in minutes*/
SELECT
	COUNT(bike.ride_id) AS num_rides,
	bike.member_casual AS member_type
FROM
	`cyclistic_trip_data.bike_data_combined` AS bike
WHERE
	TIMESTAMP_DIFF(bike.ended_at - bike.started_at, MINUTE) < 1
GROUP BY
	member_type;
```
</detail>
- I executed a query to return all information for records that excluded a ride type of "docked" and had a duration greater or equal to one minute.
<detail>
<summary>Show SQL</summary>
```sql
SELECT
	*
FROM
	`cyclistic_trip_data.bike_data_combined` AS bike
WHERE
	bike.rideable_type NOT IN ("docked")
	AND 
	TIMESTAMP_DIFF(bike.ended_at - bike.started_at,MINUTE) >=1
ORDER BY
	bike.started_at;
```
</detail>
	- I saved the results of this query into a new table and also exported them as a .CSV file to preserve the information on which further analysis was conducted.
	- I queried this new table to verify the minimum ride duration was at least one minute.
<detail>
<summary>Show SQL</summary>
```sql
SELECT
	MIN(TIMESTAMP_DIFF(bike.ended_at-bike.started_at,MINUTE)) AS min_duration_clean
FROM
	`cyclistic_trip_data.bike_data_clean` AS bike;
```
</detail>
	- I queried the new table to verify that number of rides was as expected (total from combined monthly records less "docked" rides)
<detail>
<summary>Show SQL</summary>
```sql
SELECT
	COUNT(bike.ride_id) AS num_rides,
	bike.member_casual AS member_type
FROM
	`cyclistic_trip_data.bike_data_clean` AS bike
GROUP BY
	member_type;
```
</detail>

## Analyzing the Data
Now that I had a table containing cleaned data from all the combined months in the review period, I could begin my analysis.  I continued to use SQL to pull subsets of the data I could filter and sort in a variety of ways.  I saved the results of each of these queries as .CSV files and exported each to a "Summary Data" directory for access and use in other analysis and data visualization tools (Google Sheets and Tableau).  

Given the information contained in the monthly data, I began to explore the interplay of a variety of variables to look for trends and compare ridership for members and casual riders:
* Riders per day
- Holidays
- Average ride time
- Trip destination
- Time of day, with a particular focus on comparing commuting and non-peak hours
- Time of week (weekday vs. weekend)
- Rental and return locations
- Bike Type

### Average Riders per day
I explored the distribution of casual riders and members throughout the week to see if there were differences in their riding tendencies.  I ran a query to return a count of the riders of each membership type by day.
<detail>
<summary>Show SQL</summary>
```sql
SELECT
	COUNT(bike.ride_id) AS bike,
	bike.member_casual AS member_type,
	bike.day_text AS day
FROM
	`cyclistic_trip_data.bike_data_clean` AS bike
GROUP BY
	day,
	member_type
ORDER BY
	member_type;
```
</detail>
I used Google Sheets to create a bar graph displaying the number of riders per day for each membership type.  The number of members riding was relatively stable across each day.  However, there was a clear trend to the number of casual riders, which peaked during the weekends and fell off considerably during the weekdays.  I added a trendline displaying the moving averages for both membership types to help illustrate that observation.

### Holidays
Since the data showed a clear difference in the number of casual riders on weekends versus weekdays, I wondered if there may also be an increase in casual riders on holidays.  I wrote a query to join the table of clean bike data with a table containing a list of federal holidays and return the count of the number of riders in each membership group on these dates.
<detail>
<summary>Show SQL</summary>
```sql
/*INNER JOIN used to return the records in the bike table with rental start dates matching the federal holidays defined in the holiday table.  The started_at column is formatted as YYYY-MM-DD HH:MM:SS, but the time data was not important for this query.  FORMAT_DATETIME was used to ensure the format of the date values in the bike table matched those of the holiday table*/
SELECT
	COUNT(bike.ride_id) AS num_rides,
	holiday.holiday_date,
	holiday.holiday_name,
	bike.member_casual AS member_type
FROM
	`cyclistic_trip_data.federal_holidays` AS holiday
INNER JOIN 
ON FORMAT_DATETIME("%b-%d%-%Y",bike.started_at) AS holiday.holiday_date
GROUP BY
	holiday_date,
	member_type,
	holiday_name
ORDER BY
	holiday_date;
```
</detail>
I used Google Sheets to compile the sum of holiday riders for each membership type, and prepared a pie chart to display the result.  There was a nearly equal distribution of members and casual riders in total.  When I plotted the number of riders for each membership type on the specific holidays, the chart showed a clear increase in ridership during the warmer months, particularly for the casual riders.  

### Average Ride times
I was curious to see if there were differences in average ride times for members and causal riders.  I used SQL to calculate the average ride time for both groups in each of the 12 months in the review period.
<detail>
<summary>Show SQL</summary>
```sql
/*Used ROUND and AVG to provide the average ride time in minutes, rounded to 2 decimal places*/
SELECT
	FORMAT_DATETIME("%b %Y",bike.started_at) AS rental_month,
	ROUND(AVG(TIMESTAMP_DIFF(bike.ended_at,bike.started_at,MINUTE)),2) AS ride_time_minutes,
	bike.member_casual AS member_type
FROM
	`cyclistic_trip_data.bike_data_clean` AS bike
GROUP BY
	rental_month,
	member_type
ORDER BY
	ride_time_minutes DESC;
```
</detail> 
I created a bar chart in Google Sheets to display the comparison of each month's average ride times for members and casual riders.  I also calculated the overall average ride time for each group.  The data showed that ride times for casual riders increase seasonally as the weather warms, whereas ride times for members remain relatively constant throughout the year.  The ride times for casual riders are also longer than those of members, suggesting to me that many members may ride with a destination in mind whereas casual riders may be riding more for leisure.

### Trip Destination
I wanted to explore ride times further, so I wrote a SQL query to give me data for rides by membership type that considered the time of the rental (weekday vs. weekend) and the trip destination (one way [returned to a location that differed from the location of rental] or round trip [rented and returned at the same location]).  
<detail>
<summary>Show SQL</summary>
```sql
/*Returns average ride time in minutes, rounded to 2 decimal places, grouped by membership type, trip type, and day of ride.  CASE statement used to assign trips as "round trip" or "one way" as determined by a comparison of the staring and ending latitudes and longitudes.  CASE statement used to categorize rides as "weekday" or "Weekend" based upon value in day_text column*/
SELECT
	ROUND(AVG(TIMESTAMP_DIFF(bike.ended_at,bike.started_at,MINUTE)),2) AS ride_time_minutes,
	bike.member_casual AS member_type,
	CASE
		WHEN bike.start_lat = bike.end_lat AND
		bike.start_lng = bike.end_lng
		THEN 'round trip'
		ELSE 'one way'
	END AS trip_type,
	CASE 
		WHEN bike.day_text NOT IN ("Sat","Sun")
		THEN 'weekday'
		ELSE 'weekend'
	END AS day
FROM
	`cyclistic_trip_data.bike_data_clean` AS bike
GROUP BY
	member_type,
	trip_type,
	day;
```
</detail>
I created a chart in Google Sheets to show the impact time of week and trip destination has on the average ride times for each membership type.  I noticed obvious differences in ride times, with round trips being significantly longer than one-way rides.  Round trips for casual riders were also over twice as long as those of members.  The group with the most comparable ride times were one-way rides that took place on weekdays.

### Ridership by Time of Day
I wanted to check for trends in the number of riders who initiated their rentals within the most frequent commuting hours as compared to non-peak times.  I also incorporated the geographic information into some of the SQL to see how location related.  My thinking was that a cluster of members and casual riders using bikes at common times and locations may share purposes for renting.  I wrote several SQL queries to extract this information from the data set.

Query to return top 250 rental locations of members grouped by the time of rental (peak vs. non-peak commuting hours) and type of bike (classic or electric)
<detail>
<summary>Show SQL</summary>
```sql
/*CASE statement used to assign ride to specific windows of time, based upon the time of rental.  The started_at field is in DATETIME format, and the hour of rental was extracted to be used for sorting the data into the appropriate group.  CONCAT was used to combine latitude and longitude coordinates into a single column to faciliate correcdt grouping of rental locations*/
SELECT
	COUNT(bike.ride_id)	AS num_riders,
	CASE
		WHEN EXTRACT(HOUR FROM bike.started_at) BETWEEN 6 AND 10
		THEN 'morning commute'
		WHEN EXTRACT(HOUR FROM bike.started_at) BETWEEN 15 AND 19
		THEN 'evening commute'
		ELSE 'non-peak' 
	END AS time_of_day,
	bike.rideable_type AS ride_type,
	CONCAT(bike.start_lat,",",bike.start_lng) AS start_location
FROM
	`cyclistic_trip_data.bike_data_clean` as bike
WHERE
	bike.member_casual = "member"
GROUP BY
	time_of_day,
	ride_type,
	start_location
ORDER BY
	num_riders DESC
LIMIT 
	250;
```
</detail>
Query to return top 250 rental locations of casual riders grouped by the time of rental (peak vs. non-peak commuting hours) and type of bike (classic or electric)
<detail>
<summary>Show SQL</summary>
```sql
/*CASE statement used to assign ride to specific windows of time, based upon the time of rental.  The started_at field is in DATETIME format, and the hour of rental was extracted to be used for sorting the data into the appropriate group.  CONCAT was used to combine latitude and longitude coordinates into a single column to faciliate correcdt grouping of rental locations*/
SELECT
	COUNT(bike.ride_id)	AS num_riders,
	CASE
		WHEN EXTRACT(HOUR FROM bike.started_at) BETWEEN 6 AND 10
		THEN 'morning commute'
		WHEN EXTRACT(HOUR FROM bike.started_at) BETWEEN 15 AND 19
		THEN 'evening commute'
		ELSE 'non-peak' 
	END AS time_of_day,
	bike.rideable_type AS ride_type,
	CONCAT(bike.start_lat,",",bike.start_lng) AS start_location
FROM
	`cyclistic_trip_data.bike_data_clean` as bike
WHERE
	bike.member_casual = "casual"
GROUP BY
	time_of_day,
	ride_type,
	start_location
ORDER BY
	num_riders DESC
LIMIT 
	250;
```
</detail>
I used Google Sheets to split the start locations back into separate columns for latitude and longitude.  I also added a new column to display the ranked order of each result.  The data was then imported into Tableau, where I created maps to plot the results.  I set up a filter to show the locations by the time of the rental and observed that only 3 of the top locations for casual riders fell during the morning commuting hours.  These locations were in areas of the city near tourist attractions and hotels, suggesting to me that a number of these riders may not be residents of the Cyclistic service area.

Query to investigate the number of riders of each membership type, grouped by day (weekend vs weekday) and time (peak vs. non-peak commuting hours) of rental
<detail>
<summary>Show SQL</summary>
```sql
SELECT
	COUNT(bike.ride_id) AS num_riders,
	bike.member_casual AS member_type,
	CASE
		WHEN EXTRACT(HOUR FROM bike.started_at) BETWEEN 6 AND 10
		THEN 'morning commute'
		WHEN EXTRACT(HOUR FROM bike.started_at) BETWEEN 15 AND 17
		THEN 'evening commute'
		ELSE 'non-peak'
	END AS time_of_day,
	CASE
		WHEN bike.day_text NOT IN ("Sat","Sun")
		THEN 'weekday'
		ELSE 'weekend'
	END AS day
FROM
	`cyclistic_trip_data.bike_data_clean` AS bike
GROUP BY
	member_type,
	time_of_day,
	day;
```
</detail>
I created pie charts in Google Sheets to display the proportions of rentals by time and day of each membership type.  Members and casual riders shared the same top two categories (evening commute and non-peak hours, both on weekdays).  There was also a large slice of casual riders who rent bikes on weekends during non-peak hours.

Query to compile monthly number of riders of each membership type, based upon the time (peak vs. non-peak commuting hours) and day (weekday vs. weekend) of rental
<detail>
<summary>Show SQL</summary>
```sql
/*FORMAT_DATETIME used to return rental month and year of started_at column, which was originally formatted as YYYY-MM-DD HH:MM:SS*/
SELECT
	COUNT(bike.ride_id) AS num_riders,
	FORMAT_DATETIME("%b%y", bike.started_at) AS rental_month,
	bike.member_casual AS member_type,
	CASE
		WHEN EXTRACT(HOUR FROM bike.started_at) BETWEEN 6 AND 10
		THEN 'morning commute'
		WHEN EXTRACT(HOUR FROM bike.started_at) BETWEEN 15 and 17
		THEN 'evening commute'
		ELSE 'non-peak'
	END AS time_of_day,
	CASE
		WHEN bike.day_text NOT IN ("Sat","Sun")
		THEN 'weekday'
		ELSE 'weekend'
	END AS day
FROM
	`cyclistic_trip_data.bike_data_clean` AS bike
GROUP BY
	rental_month,
	member_type,
	time_of_day,
	day;
```
</detail>
These query results were imported into Google Sheets, where I calculated the percentage of monthly change of riderships in each category (member vs. casual, weekday vs. weekend).  I charted these results to help visualize seasonal trends in ridership.  The trendlines showed that an increase in casual ridership has a greater positive correlation to warmer months of the year than members.

### Rental Location
I leveraged the geographic coordinates in the rental data to look for hotspots throughout the Cyclistic service area.  I queried the data to generate a list of the top 100 locations for rental, grouped by membership type.
<detail>
<summary>Show SQL</summary>
```sql
SELECT
	COUNT(bike.ride_id) AS num_riders,
	bike.member_casual AS member_type,
	CONCAT(bike.start_lat,",",bike.start_lng) AS start_location
FROM
	`cyclistic_trip_data.bike_data_clean` AS bike
GROUP BY
	member_type,
	start_location
ORDER BY
	num_riders DESC
LIMIT
	100;
```
</detail>
Using Google Sheets, I split the staring location into separate columns for latitude and longitude and added a new column for rank before importing the table into Tableau.  I plotted the results onto a map of the Chicago area that included neighborhoods, major points of interest, and public transportation stations.  I captured several views of different areas of the service area and noticed several rental hotspots for members in close proximity to public transit.  Multiple locations also appeared to be significant rental locations for both membership groups.

### Bike Types
One of the prior queries returned results for the top 250 rental areas for members and casual members to include detail on the type of bike rented (classic vs. electric).  To supplement this, I also wrote a new query to provide the count of each membership type for these bike types.
<detail>
<summary>Show SQL</summary>
```sql
SELECT
	COUNT(bike.ride_id) AS num_riders,
	bike.rideable_type AS bike_type,
	bike.member_casual AS member_type
FROM
	`cyclistic_trip_data.bike_data_clean` AS bike
GROUP BY
	bike_type,
	member_type
```
</detail>
With Google Sheets, I created pie charts to show the distribution of each bike type within the different membership groups.  The data showed a preference for classic bikes for both members and casual riders.  However, when the top 250 rental locations for each membership group were viewed on Tableau-generated maps, it became apparent to me that electric bikes did not seem to be a major consideration.  The data could not reveal whether this was due to rider preference or bike availability.  However, I eliminated bike type as a factor I needed to consider when making my recommendations.

### Other Queries
In addition to the queries noted in the previous sections, I executed several others when compiling this information.  These other SQL statements are recorded in _Appedix 2 - SQL Queries_ contained in the final report of my analysis.  I will not describe any of these queries in further detail here since they were not ultimately used to present my findings.  In some intances I found that these queries did not give me information that was meaningful to illustrate new trends, while in other cases I made a choice to use only what seemed to provide the most insight into the data. 

## Sharing the analysis

In this scenario, my direct manager gave this assignment to me.  I was to report my findings back to her at the conclusion of my work.   My analysis may also need to be available for sharing with the other stakeholders:  members of the marketing team (some of which were working to answer other questions releavant to the proposed marketing campaign) and the executive team, who will ultimately be making decisions about how to proceed with the company's marketing efforts.  I knew I had to share information directly with my manager and thus expected to brief her in person at the project's conclusion.  I decided to write up a report of my analysis that could be provided to the other stakeholders for their review as needed. 

In the report, I laid out the key elements necessary to understand my analysis.  These included the following:
* A statement of the business objective
- A background on the source of the data, it's limitations, and the assumptions made when conducting the analysis
- The tools used to conduct the analysis and prepare the data visualizations
- An overview of the steps taken to clean the data
- The change log and record of SQL queries, available in appendices should anyone reviewing the report wish to drill down into this level of detail
- The descriptions of the different dimensions explored while searching for trends, complete with data visualizations as described in the previous section of this narrative
- A summary of key observations about the relationship between members and casual riders along these dimensions
- A list of final recommendations (described below)

## Taking Action

When I concluded my analysis, I arrived at three recommendations to inform how to structure the new marketing campaign.

#### Observing a rider segment (commuters) with strong correlation across a number of factors that was common to both membership types
A number of trends in the data supported my hypothesis that a segment of annual members used the bike share as a means of transportation during their daily commutes.  Rides for members were evenly distributed throughout the week, the vast majority of them were "one way" trips, and a large volume of these rentals took place during peak commute times and in close proximity to transportation stations.  Also, members' rides tended to be relatively short, suggesting to me that they were purposefully made, consistent with getting from point A to B.  

When considering casual ridership, the membership groups exhibited enough similarity in several of these dimensions to suggest that a number of casual riders use the Cyclistic bikes in the same way as members.  I recognized that my task was to identify ways in which the membership groups used bikes differently.  However, I felt that the combination of all these factors pointing towards a common use (commuting) was important to incorporate as a prong of the marketing approach.  

#### Identifying key differences in trends between casual riders and members
My analysis revealed that casual ridership increased significantly in the warmer months of spring, summer, and early fall while also being concentrated on weekend days.  Comparitively, rides for members had a much more even distribution throughout both the year and week.  Thus, I recommended that marketing efforts focus on the times when casual riders most heavily utilze the bike share (weekends and holidays during warm weather months) in order to get the most reach.

#### Obtaining more information
As I noted previously, the available ridership data lacks detail about the actual riders themselves.  We know the when and where, the type of bike, and the membership status of each bike rented.  However, we have no indication of a customer's prior rental history, we do not know if s/he lives in the Cyclistic service area, and we do not know how the bike was actually used.  I suggested a survey of riders to gather more data of this kind that could be used to refine the marketing campaign.  

Also, I thought it could be useful to incorporate the customer feedback on why the specific type of rental pass was chosen.  For instance, maybe someone rides regularly during the spring and summer but did not want to buy an annual membership since they would not be using the bike share during the rest of the year.  Alternatively, a rider may be more interested in a membership pass if billed monthly instead of annually.  There may be several other rider preferences that factor into their decisions to purchase a specific type of pass, but we currently have no way to access or gauge that information.

### Next steps
Once I submit my report to the marketing director, the next steps fall in the hands of the company's decision makers.  I would be fully prepared to craft a presentation to make to the other stakeholders if requested and/or to respond to  requests to elaborate or answer questions on elements of my analysis.