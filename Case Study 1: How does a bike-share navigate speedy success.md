# Google Data Analytics Professional Capstone 
## Case Study 1: How Does a Bike-Share Navigate Speedy Success?

## 1. Introduction
This case study involves performing the tasks of a junior data analyst for a fictional bike share company known as Cyclistic. To answer the key business questions I have been asked to follow the data analysis process taught in the Google Data Analytics Professional Certificate Program which involves the following steps: ask, prepare, process, analyze, share, and act. 

## 2. Scenario
You are a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of
marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, your
team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will
design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your
recommendations, so they must be backed up with compelling data insights and professional data visualizations.

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
1. How do annual members and casual riders use Cyclistic bikes differently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?

Business Task: In order to design marketing strategies aimed at converting casual riders into annual members, we must determine the differences in how annual members and casual riders use the service.
Lily Moreno who is the director of marketing and my manager has assigned the first of the previous three questions: How do annual members and casual riders use Cyclistic bikes.

### Step 2: Prepare
This step in the process will involve determining where the data is from, the organization of the data, the structure of the data, and how the data will be stored.

The sources of the data can be found [here](https://divvy-tripdata.s3.amazonaws.com/index.html). This webpage contains trip data from 2013 to 2023. I have decided to work with the most recent full year of data, 2023. The data is provided by Motivate International Inc.

The 2023 data files are all CSV's with the following naming convention YYYYMM-divvy-tripdata. Each file contains 13 columns of data corresponding to the following information: ride id, bike type, start time, end time, starting station, starting station id, ending station, ending station id, start latitude, start longitude, end latitude, end longitude, and finally rider type which shows if the rider was either a member or a casual rider. Below, you can find the corresponding column names:

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

All CSVs have been stored locally and cleaning will be done with Excel, afterward the cleaned files will be uploaded to SQL for analysis.

### Step 3: Process
Tool: Excel is used to clean the individual files and add two additional columns. The files are then uploaded to SQL, where all 12 files are combined into a single table.

### Data cleaning
First, I checked for duplicates in the ride_id column to ensure that all rides are unique. To do this, I performed the following steps:
1. Navigated to the Data tab
2. Selected entire sheet and then remove duplicates
3. Unchecked all columns except the rider_id column

No duplicate IDs were found in each of the 12 CSVs.

Second, I set up conditional formatting to fill all blank cells with the color red. All empty cells appeared to be in the following columns, start_station_name, end_station_name, start_lat, start_lng, end_lat, end_lng. As I do plan to use any of the longitude or latitude values, I left the rows where only those values were empty in the tables. However, the rows that were missing the station names were removed. To remove these rows, I performed the following steps:

1. Navigate to Home tab
2. Selected find and select
3. Selected go to special
4. Selected blanks
5. After all blank cells were selected, deleted all the rows containing a blank cell.

Repeated the above steps multiple times per sheet, as all blank cells would not be deleted from one pass.

Third, I checked to see that there were no unexpected values for the rideable_type and member_casual columns by performing the following steps:

1. Selected Home tab -> Sort & Filter -> Filter
2. Chose the dropdown next to rideable_type to see if there were only the three expected values; classic_bike, docked_bike, electric_bike
3. Chose the dropdown next to member_casual to see if there were only the two expected values; member, and classic

Fourth, ensured the format for the start and end columns were the same by changing the format to date and time using the following steps:
1. Selected started_at column
2. Selected format -> number -> date_> chose following format mm/dd/yy 00:00
3. Repeated above steps for ended_at column

For each sheet, I also added the following columns; ride_length which uses the start and ends times to calculate the length of the ride, and day_of_week which shows the day of the week the ride took place as a number beginning with 1 for Sunday and ending with 7 for Saturday.

After creating the ride_length column used a filter to check for values that were incorrect and found that there were values such as ####### which indicated that the start time was after the end time and times that were 0 indicating that the ride didn't take place. All rows that contain these values were removed from the sheet.

After performing the above cleaning steps, I uploaded the 12 CSVs to Google Cloud Storage and then imported them into my Citibike project on Big Query. After checking to see the schema was the same for each table, I ran the following code to combine all 12 tables into one, then created a table from the result:

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

### Step 4 Analyze & Share 

After cleaning and transforming the data was completed, I began my analysis of the data. I ran a number of queries to extract information from the tables and then used Tableau to create visualizations in order to answer the question assigned by Lily; How do annual members and casual riders use Cyclistic bikes differently? In this project, I have a document that contains all queries used to extract the information being used for analysis.

There were a total of 4280349 riders for the year 2023. First, let's look at the number of general riders per month.

![viz for monthly riders](https://github.com/NicholasKSanchez/GoogleCertificateCitibikeCapstone/blob/main/Visuals/Number%20of%20Rides%20per%20Month.png)

As you can see, the summer months are the most popular times in general, with the number of riders dropping at the beginning of fall and into the winter months while steadily increasing at the beginning of Spring. 

Now to compare the amount of members vs casual riders. The below graphic shows that 64.56% of riders are member, while 35.44% are casual riders.

![viz casual vs member riders](https://github.com/NicholasKSanchez/GoogleCertificateCitibikeCapstone/blob/main/Visuals/Member%20vs%20Casual%20Number%20of%20Rides.png)

Monthly, both member and casual riders tend to use the service mostly during the summer months, while dropping off during fall and into winter.

![viz for casual vs members monthly riders](https://github.com/NicholasKSanchez/GoogleCertificateCitibikeCapstone/blob/main/Visuals/Member%20vs%20Casual%20Number%20of%20Rides%20per%20Month.png)

To keep going with the theme of time, let's look at what times during the week that riders use the service. The first visual shows in general the most popular days of the week for both member and casual. The second visual will show the most popular days of the week that member riders ride versus casual. The third will show specific start times for member and casual riders.

![viz for day of week](https://github.com/NicholasKSanchez/GoogleCertificateCitibikeCapstone/blob/main/Visuals/Number%20of%20Rides%20per%20Day%20of%20Week.png)

![viz for day of week member vs casual](https://github.com/NicholasKSanchez/GoogleCertificateCitibikeCapstone/blob/main/Visuals/Member%20vs%20Casual%20Rides%20per%20Day%20of%20Week.png)

![viz for start times member versus casual](https://github.com/NicholasKSanchez/GoogleCertificateCitibikeCapstone/blob/main/Visuals/Member%20vs%20Casual%20Start%20Times.png)

In general the number of riders during the week seem to be pretty even across all days with only Sunday and Monday seeing a drop. However, casual riders and member riders do seem to ride at different times during the week. Casual riders tend to ride more during the weekends, while member tend to ride more during the week. Additionally, looking at the start times of the riders, you can see that casual rider start times increase steadily throughout the day while member riders have two peaks. The first peak is between the times 6-9am, while the second peak is between the times of 3-7pm.

Analyzing the above information, it can be said that casual riders take trips mainly on the weekends for casual activities, while members will use the service during the week for their work commutes.

When looking at the average amount of time each ride takes in minutes, we get the below graphic.

![viz for member vs casual weekly ride lengths](https://github.com/NicholasKSanchez/GoogleCertificateCitibikeCapstone/blob/main/Visuals/Member%20vs%20Casual%20Day%20of%20Week%20Ride%20Lengths.png)

From the above that we can see that ride lengths for casual members are much longer on the weekends, decrease once the work week begins, and then increase near the weekend. While member riders have consistent ride times during the week with a slight increase when the weekend comes. Finally, for the last set of visuals, we will look at the top destinations for casual riders and members.

![viz for casual end stations](https://github.com/NicholasKSanchez/GoogleCertificateCitibikeCapstone/blob/main/Visuals/Casual%20Rider%20Popular%20End%20Stations.png)

![viz for member end stations](https://github.com/NicholasKSanchez/GoogleCertificateCitibikeCapstone/blob/main/Visuals/Member%20Popular%20End%20Stations.png)

The above visals show the top 10 end stations for casual and member riders. The casual rider's popular end stations show locations such as Millennium Park where the bean is located as well as the ribbon and outdoor ice skating rink, Streeter Dr & Grand Ave where the Navy Pier is located which another popular tourist spot etc. Meanwhile, the popular member locations such as Clinton st & Washgington Blvd as well as Kingsbury St & Kinzie St are not near any popular tourist destinations and are instead near commercial areas.

To summarize, casual riders use the bike service mostly during the weekends, with their destinations being overwhelmingly tourist destinations such as Millennium Park and Navy Pear. Additionally, Casual riders will use the service throughout the day instead of at specific peak times. Finally, casual riders will take longer trips than members, while the amount of trips is less than members.

Member riders will use the service mostly during the week, with their destinations being commercial districts and residential areas. Additionally, member riders will use the service at two different peak times, 6-9am and 3-7pm, corresponding to times when people usually commute to and from work. Finally, member riders trip lengths are shorter and remain roughly the same length throughout the week.

### Step 6: Act

With the above analysis of how casual and member riders use the service differently, I would like to share the three recommendations that I have to turn casual riders into member riders:

1. Since both types of riders use the service more during the summer, targeted marketing campaigns should be held during these months and in the areas that receive the most traffic from casual riders.
2. Casual riders most often use the service on the weekends, so introducing a weekend membership should entice those weekend warriors into buying a membership.
3. Working with local eateries around the most popular stations to provide monthly BOGO'S or discounts for members could also provide a way to convert the casual riders into members.

