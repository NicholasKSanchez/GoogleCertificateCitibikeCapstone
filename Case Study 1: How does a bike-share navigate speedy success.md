# Google Data Analytics Professional Capstone 
## Case Study 1: How Does a Bike-Share Navigate Speedy Success?

## 1. Introduction
This case study involves performing the tasks of a junior datya analyst for a ficitonal bike share company known as Cyclistic. To asnwer the key business questions I have been asked to follow the data analysis process taught in the Google Data Analytics Professional Certificate Program which involves the following steps: ask, prepare, process, analyze, share, and act. 

## 2. Scenario
You are a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of
marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, your
team wants to understand how casual riders and annual members use Cyclistic bikes dierently. From these insights, your team will
design a new marketing strategy to convert casual riders into annual members. But rst, Cyclistic executives must approve your
recommendations, so they must be backed up with compelling data insights and professional data visualizations

## 3. Data Analysis Process
The process that I will be using is the following:

1. Act
2. Prepare
3. Process
4. Analyze
5. Act
6. Share

### Step 1: Ask
Three questions will guide the future marketing program:
1. How do annual members and casual riders use Cyclistic bikes dierently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to inuence casual riders to become members?

Business Task: In order to design marketing strategies aimed at converting casual riders into annual members we must determine the differences in how annual members and casual riders use the service. 
Lily Moreneo who is the director of marketing and my manager has assigned the first of the previous three quetions: How do annual members and casual riders use Cyclistic bikes di

### Step 2: Prepare
This step in the process will involve determining where the data is from, the organization of the data, the structure of the data, and how the data will be stored.

The sources of the data can be found [here](https://divvy-tripdata.s3.amazonaws.com/index.html). This webpage contains trip data from 2013 to 2023. I have decided to work with the most recent full year of data, 2023. The data is provided by Motivate International Inc.

The 2023 data files are all csv's with the following naming convention YYYYMM-divvy-tripdata. Each file contains 13 columns of data corresponding to the following information: ride id, bike type, start time, end time, starting station, starting station id, ending station, ending station id, start latitude, start longitude, end latitude, end longitutde, and finally rider type which shows if the rider was either a member or a casual rider. Below you can find the corresponding column names:

- ride_id
- rideable_type
- started_at
- ended_at
- start_station_name
- start_station_id
- end_station_name
- end_station_id
- start_lat
- start_lng
- end_lat
- end_lng
- member_casual

All csvs have been stored locally and cleaning will be done with excel afterwards the cleaned files will be uploaded to sql for analysis.

### Step 3: Process
Tool: Excel is used to clean the individual files and add two additional columns. The files are then uploaded to SQL where all 12 files are combined into a single table.

### Data cleaning
First, I checked for duplicates in the ride_id column to ensure that all rides are unique. To do this I performed the following steps:
1. Navigated to the Data tab
2. Selected entire sheet and then remove duplicates
3. Unchecked all columns except the rider_id column

No duplicate ids were found in each of the 12 csvs.

Second, I set up conditional formatting to fill all blank cells with the color red. All empty cells appeard to be in the following columns, start_station_name, end_station_name, start_lat, start_lng, end_lat, end_lng. As I do plan to use any of the longitutde or latitude values I left the rows where only those values were empty in the tables. However, the rows that were missing the station names were removed. To remove these rows I performed the following steps:

1. Navigate to Home tab
2. Selected find and select
3. Selected go to special
4. Selected blanks
5. After all blank cells were selected deleted all the rows containging a blank cell.

Repeated the above steps multiple times per sheet as all blank cells would not be deleted from one pass.

Third, I checked to see that there were not unexpected values for the rideable_type and member_casual columns by performing the following steps:

1. Selected Home tab -> Sort & Filter -> Filter
2. Chose the dropdown next to rideable_type to see if there were only the three expected values; classic_bike, docked_bike, electric_bike
3. Chose the dropdown next to member_casual to see if there were only the two expected values; member, and classic

Fourth, ensured the format for the start and end columns were the same by changing the format to date and time using the following steps:
1. Selected started_at column
2. Selected format -> number -> date_> chose following format mm/dd/yy 00:00
3. Repeated above steps for ended_at column

For each sheet I also added the following columns; ride_length which uses the start and end times to calculate the length of the ride, and day_of_week which shows the day of the week the ride took place as a number beginning with 1 for Sunday and ending with 7 for Saturday.

After creating the ride_length column used a filter to check for values that were incorrect and found that there were values such as ####### which indicated that the start time was after the end time and times that were 0 indicating that ride didnt take place. All rows that containes these values were removed from the sheet.

After performing the above cleaning steps I uploaded the 12 csvs to Google Cloud Storage and then imported them into my Citibike project on Big Query. After checking to see the schema was the same for each table I ran the following code to combine all 12 tables into one then created a table from the result:

```
SELECT
  *
FROM 
  `citibike-409417.Citibike_2023.January`
UNION ALL
  SELECT
    *
  FROM
    `citibike-409417.Citibike_2023.February`
UNION ALL
  SELECT
    *
  FROM
    `citibike-409417.Citibike_2023.March`
UNION ALL
  SELECT
    *
  FROM
    `citibike-409417.Citibike_2023.April`
UNION ALL
  SELECT
    *
  FROM
    `citibike-409417.Citibike_2023.May`
UNION ALL
  SELECT
    *
  FROM
    `citibike-409417.Citibike_2023.June`
UNION ALL
  SELECT
    *
  FROM
    `citibike-409417.Citibike_2023.July`
UNION ALL
  SELECT
    *
  FROM
    `citibike-409417.Citibike_2023.August`
UNION ALL
  SELECT
    *
  FROM
    `citibike-409417.Citibike_2023.September`
UNION ALL
  SELECT
    *
  FROM
    `citibike-409417.Citibike_2023.October`
UNION ALL
  SELECT
    *
  FROM
    `citibike-409417.Citibike_2023.November`
UNION ALL
  SELECT
    *
  FROM
    `citibike-409417.Citibike_2023.December`
```

### Step 4 Analyze & Share (WIP)

After cleaning and transforming the data was completed I began my analysis of the data. I ran a number of queries to extract information from the tables and then used Tablau to create visualizations in order to answer the question assigned by Lily; How do annual members and casual riders use Cyclistic bikes dierently? In this project I have a document that contains all queries used to extract the information being used for analysis.

There were a total of 4280349 riders for the year 2023. First lets look at the number of general riders per month.

![viz for monthly riders]()
