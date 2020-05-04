# Project 1: Query Project

- In the Query Project, you will get practice with SQL while learning about
  Google Cloud Platform (GCP) and BiqQuery. You'll answer business-driven
  questions using public datasets housed in GCP. To give you experience with
  different ways to use those datasets, you will use the web UI (BiqQuery) and
  the command-line tools, and work with them in Jupyter Notebooks.

#### Problem Statement

- You're a data scientist at Lyft Bay Wheels (https://www.lyft.com/bikes/bay-wheels), formerly known as Ford GoBike, the
  company running Bay Area Bikeshare. You are trying to increase ridership, and
  you want to offer deals through the mobile app to do so. 
  
- What deals do you offer though? Currently, your company has several options which can change over time.  Please visit the website to see the current offers and other marketing information. Frequent offers include: 
  * Single Ride 
  * Monthly Membership
  * Annual Membership
  * Bike Share for All
  * Access Pass
  * Corporate Membership
  * etc.

- Through this project, you will answer these questions: 

  * What are the 5 most popular trips that you would call "commuter trips"? 
  
  * What are your recommendations for offers (justify based on your findings)?

- Please note that there are no exact answers to the above questions, just like in the proverbial real world.  This is not a simple exercise where each question above will have a simple SQL query. It is an exercise in analytics over inexact and dirty data. 

- You won't find a column in a table labeled "commuter trip".  You will find you need to do quite a bit of data exploration using SQL queries to determine your own definition of a communter trip.  In data exploration process, you will find a lot of dirty data, that you will need to either clean or filter out. You will then write SQL queries to find the communter trips.

- Likewise to make your recommendations, you will need to do data exploration, cleaning or filtering dirty data, etc. to come up with the final queries that will give you the supporting data for your recommendations. You can make any recommendations regarding the offers, including, but not limited to: 
  * market offers differently to generate more revenue 
  * remove offers that are not working 
  * modify exising offers to generate more revenue
  * create new offers for hidden business opportunities you have found
  * etc. 

#### All Work MUST be done in the Google Cloud Platform (GCP) / The Majority of Work MUST be done using BigQuery SQL / Usage of Temporary Tables, Views, Pandas, Data Visualizations

A couple of the goals of w205 are for students to learn how to work in a cloud environment (such as GCP) and how to use SQL against a big data data platform (such as Google BigQuery).  In keeping with these goals, please do all of your work in GCP, and the majority of your analytics work using BigQuery SQL queries.

You can make intermediate temporary tables or views in your own dataset in BigQuery as you like.  Actually, this is a great way to work!  These make data exploration much easier.  It's much easier when you have made temporary tables or views with only clean data, filtered rows, filtered columns, new columns, summary data, etc.  If you use intermediate temporary tables or views, you should include the SQL used to create these, along with a brief note mentioning that you used the temporary table or view.

In the final Jupyter Notebook, the results of your BigQuery SQL will be read into Pandas, where you will use the skills you learned in the Python class to print formatted Pandas tables, simple data visualizations using Seaborn / Matplotlib, etc.  You can use Pandas for simple transformations, but please remember the bulk of work should be done using Google BigQuery SQL.

#### GitHub Procedures

In your Python class you used GitHub, with a single repo for all assignments, where you committed without doing a pull request.  In this class, we will try to mimic the real world more closely, so our procedures will be enhanced. 

Each project, including this one, will have it's own repo.

Important:  In w205, please never merge your assignment branch to the master branch. 

Using the git command line: clone down the repo, leave the master branch untouched, create an assignment branch, and move to that branch:
- Open a linux command line to your virtual machine and be sure you are logged in as jupyter.
- Create a ~/w205 directory if it does not already exist `mkdir ~/w205`
- Change directory into the ~/w205 directory `cd ~/w205`
- Clone down your repo `git clone <https url for your repo>`
- Change directory into the repo `cd <repo name>`
- Create an assignment branch `git branch assignment`
- Checkout the assignment branch `git checkout assignment`

The previous steps only need to be done once.  Once you your clone is on the assignment branch it will remain on that branch unless you checkout another branch.

The project workflow follows this pattern, which may be repeated as many times as needed.  In fact it's best to do this frequently as it saves your work into GitHub in case your virtual machine becomes corrupt:
- Make changes to existing files as needed.
- Add new files as needed
- Stage modified files `git add <filename>`
- Commit staged files `git commit -m "<meaningful comment about your changes>"`
- Push the commit on your assignment branch from your clone to GitHub `git push origin assignment`

Once you are done, go to the GitHub web interface and create a pull request comparing the assignment branch to the master branch.  Add your instructor, and only your instructor, as the reviewer.  The date and time stamp of the pull request is considered the submission time for late penalties. 

If you decide to make more changes after you have created a pull request, you can simply close the pull request (without merge!), make more changes, stage, commit, push, and create a final pull request when you are done.  Note that the last data and time stamp of the last pull request will be considered the submission time for late penalties.

---

## Parts 1, 2, 3

We have broken down this project into 3 parts, about 1 week's work each to help you stay on track.

**You will only turn in the project once  at the end of part 3!**

- In Part 1, we will query using the Google BigQuery GUI interface in the cloud.

- In Part 2, we will query using the Linux command line from our virtual machine in the cloud.

- In Part 3, we will query from a Jupyter Notebook in our virtual machine in the cloud, save the results into Pandas, and present a report enhanced by Pandas output tables and simple data visualizations using Seaborn / Matplotlib.

---

## Part 1 - Querying Data with BigQuery

### SQL Tutorial

Please go through this SQL tutorial to help you learn the basics of SQL to help you complete this project.

SQL tutorial: https://www.w3schools.com/sql/default.asp

### Google Cloud Helpful Links

Read: https://cloud.google.com/docs/overview/

BigQuery: https://cloud.google.com/bigquery/

Public Datasets: Bring up your Google BigQuery console, open the menu for the public datasets, and navigate to the the dataset san_francisco.

- The Bay Bike Share has two datasets: a static one and a dynamic one.  The static one covers an historic period of about 3 years.  The dynamic one updates every 10 minutes or so.  THE STATIC ONE IS THE ONE WE WILL USE IN CLASS AND IN THE PROJECT. The reason is that is much easier to learn SQL against a static target instead of a moving target.

- (USE THESE TABLES!) The static tables we will be using in this class are in the dataset **san_francisco** :

  * bikeshare_stations

  * bikeshare_status

  * bikeshare_trips

- The dynamic tables are found in the dataset **san_francisco_bikeshare**

### Some initial queries

Paste your SQL query and answer the question in a sentence.  Be sure you properly format your queries and results using markdown. 

- What's the size of this dataset? (i.e., how many trips)
  * Answer: 983648 trips
  * SQL query:
```sql
#standardSQL
SELECT
  COUNT(DISTINCT trip_id) AS Total_number_of_trips
FROM
  `bigquery-public-data.san_francisco.bikeshare_trips`
```

- What is the earliest start date and time and latest end date and time for a trip?
  * Answer: 
    * earliest start date and time: 2013-08-29 09:08:00 UTC, 
    * latest end date and time: 2016-08-31 23:48:00 UTC
  * SQL query: 
  
```sql
#standardSQL
SELECT
  MIN(start_date) AS earliest_start_date,
  MAX(end_date) AS latest_end_date
FROM
  `bigquery-public-data.san_francisco.bikeshare_trips`
```

- How many bikes are there?
  * Answer: 700 bikes
  * SQL query:
  
```sql
#standardSQL
SELECT
  COUNT(DISTINCT bike_number) AS total_bikes
FROM
  `bigquery-public-data.san_francisco.bikeshare_trips`
```

### Questions of your own
- Make up 3 questions and answer them using the Bay Area Bike Share Trips Data.  These questions MUST be different than any of the questions and queries you ran above.

- Question 1: How many trips started and ended at the same stations? how about the ones ended at a different station?
  * Answer: Results showed that the majority (~ 96.7%) of all trips ended at a different station rather than the same one.
  
trips | trip_type
 ---  | --- 
32047 | Same start & end stations
951601| Different start & end stations

  * SQL query:
  
```sql
#standardSQL
SELECT count(trip_id) as trips,
    CASE
      WHEN 
        start_station_id = end_station_id AND 
        start_station_name = end_station_name  
        THEN 'Same start & end stations'
      WHEN 
        start_station_id != end_station_id 
        AND start_station_name != end_station_name 
        THEN 'Different start & end stations'
  END AS trip_type
FROM  `bigquery-public-data.san_francisco.bikeshare_trips`
GROUP BY trip_type
```

  * Throughout running this query, it turned out that there exist some mismatches between station ID's/names within 
    and across different tables, which indicates that the dataset would need some cleaning.
       * First of all, the bikeshare_status table has more unique station ID's (=75) as compared to the 
        bikeshare_stations table (=74). By doing more detailed query, it turned out that station_id  = 87 only 
        appeared in the bikeshare_status table.
       * Moreover, station names and ID's (for either Start or End stations) do not match in the bikeshare_trips table.
        while there are 84 distinct station names, only 74 distinct station ID can be identified. Preliminary
        inspection showed that it could have most probably happened due to misspelling of the station names for 
        the same station ID. 
        
  * SQL queries (to detect the name/ID mismatch):
  
```sql
#standardSQL
/* Query to find and compare the total number of unique staion ID's 
in two bikeshare_stations and bikeshare_status tables */
SELECT 
  COUNT(DISTINCT stations.station_id) AS total_station_station_id, 
  COUNT(DISTINCT status.station_id) AS total_status_station_id 
FROM  
  `bigquery-public-data.san_francisco.bikeshare_status` AS status
FULL OUTER JOIN 
  `bigquery-public-data.san_francisco.bikeshare_stations` AS stations
ON 
  status.station_id = stations.station_id 
```
--
```sql
#standardSQL
/* Query to find and compare the list of unique staion ID's 
across the bikeshare_stations and bikeshare_status tables */
SELECT 
  DISTINCT stations.station_id AS station_station_id, 
  status.station_id AS status_station_id 
FROM  
  `bigquery-public-data.san_francisco.bikeshare_status` AS status
FULL OUTER JOIN 
  `bigquery-public-data.san_francisco.bikeshare_stations` AS stations
ON 
  status.station_id = stations.station_id 
ORDER BY 
  status_station_id
```
--
```sql
#standardSQL
/*Query to find and compare the total number of unique station 
mes and ID's within the bikeshare_trips table */
SELECT 
  COUNT(DISTINCT start_station_id) AS total_start_station_id, 
  COUNT(DISTINCT end_station_id) AS total_end_station_id,
  COUNT(DISTINCT start_station_name) AS total_start_station_name,
  COUNT(DISTINCT end_station_name) AS total_end_station_name,
FROM  
  `bigquery-public-data.san_francisco.bikeshare_trips`
```
--
```sql
#standardSQL 
/* Query to find and compare the list of unique start 
station names and ID's within the bikeshare_trips table */
 SELECT DISTINCT 
  start_station_id AS start_station_id, 
  start_station_name AS start_station_name,
FROM  
  `bigquery-public-data.san_francisco.bikeshare_trips`
ORDER BY 
  start_station_name
```

- Question 2: Find the frequency of bike trips within different trip time intervals including less than 5 minutes, between 5 and 30 minutes, between 30 and 45 minutes, between 45 minutes and 1 hour, and more than 1 hour. The goal for this question is to help enhance the current offers and/or recommending new offers.
  * Answer: the majority of trips (95.19%) happened within 30 minutes.

| trip_time_interval | trips | total_trips | trips_percent (%)|
| --- | --- | --- | --- |
| 5 < t < 30 minutes | 756232 | 983648 | 76.88 |
| t < 5 minutes | 180079 | 983648 | 18.31 |
| t > 60 minutes | 28091 | 983648 | 2.86 |
| 30 < t < 45 minutes | 12906 | 983648 | 1.31 |
| 45 < t < 60 minutes | 6340 | 983648 |  0.64 |

  * SQL query:
```sql
#standardSQL
SELECT trip_time_interval, trips, total_trips, ROUND(100 * trips/total_trips, 2) AS trips_percent 
FROM
  (SELECT trip_time_interval, count(*) AS trips 
  FROM
    (SELECT 
        CASE
            WHEN duration_sec <= 5 * 60  THEN ' t < 5 minutes'
            WHEN duration_sec <= 30 * 60 THEN ' 5 < t < 30 minutes'
            WHEN duration_sec <= 45 * 60 THEN '30 < t < 45 minutes'
            WHEN duration_sec <= 60 * 60 THEN '45 < t < 60 minutes'
            ELSE 't > 60 minutes'
        END AS trip_time_interval
    FROM  `bigquery-public-data.san_francisco.bikeshare_trips`)
  Group by trip_time_interval), 
  (SELECT count(*) as total_trips
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`)
ORDER BY trips_percent DESC
```

- Question 3: Find the total hours that each bike was used. This question can help to find out if some bikes needs more maitenance (or replacement) as compared to the other ones. 
  * Answer: The most and the least used bikes (in terms of hours used) are as follows:

| max_used_time_for_single_bike (hours)| min_used_time_for_single_bike(hours)|
| --- | --- |
| 5375.0 | 0.9 |

  * SQL query:
  
```sql
#standardSQL
SELECT MAX(total_trip_hours_per_bike) AS max_used_time_for_single_bike, 
MIN(total_trip_hours_per_bike) AS min_used_time_for_single_bike,
FROM
  /* The following subquery itself can be used to find the 
  total hours that each bike was used.*/
  (SELECT 
          COUNT(trip_id) AS number_of_total_trips_per_bike, 
          ROUND(SUM(duration_sec)/3600, 1) AS total_trip_hours_per_bike
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  GROUP BY bike_number
  ORDER BY total_trip_hours_per_bike DESC)
```
    * 
    
### Bonus activity queries (optional - not graded - just this section is optional, all other sections are required)

The bike share dynamic dataset offers multiple tables that can be joined to learn more interesting facts about the bike share business across all regions. These advanced queries are designed to challenge you to explore the other tables, using only the available metadata to create views that give you a broader understanding of the overall volumes across the regions(each region has multiple stations)

We can create a temporary table or view against the dynamic dataset to join to our static dataset.

Here is some SQL to pull the region_id and station_id from the dynamic dataset.  You can save the results of this query to a temporary table or view.  You can then join the static tables to this table or view to find the region:
```sql
#standardSQL
select distinct region_id, station_id
from `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`
```

- Top 5 popular station pairs in each region

- Top 3 most popular regions(stations belong within 1 region)

- Total trips for each short station name in each region

- What are the top 10 used bikes in each of the top 3 region. these bikes could be in need of more frequent maintenance.

---

## Part 2 - Querying data from the BigQuery CLI 

- Use BQ from the Linux command line:

  * General query structure

    ```
    bq query --use_legacy_sql=false '
        SELECT count(*)
        FROM
           `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```

### Queries

1. Rerun the first 3 queries from Part 1 using bq command line tool (Paste your bq
   queries and results here, using properly formatted markdown):

  * What's the size of this dataset? (i.e., how many trips)
    * Answer: 983648 trips
    * Linux command line:
        ```
        bq query --use_legacy_sql=false '
            SELECT count(distinct trip_id) AS Total_number_of_trips
            FROM 
                `bigquery-public-data.san_francisco.bikeshare_trips`'
        ```

  * What is the earliest start time and latest end time for a trip?
    * Answer: 
        * earliest start date and time: 2013-08-29 09:08:00 UTC, 
        * latest end date and time: 2016-08-31 23:48:00 UTC
    * Linux command line:
        ```
        bq query --use_legacy_sql=false '
            SELECT MIN(start_date) AS earliest_start_date, MAX(end_date) AS latest_end_date
            FROM 
                `bigquery-public-data.san_francisco.bikeshare_trips`'
        ```  

  * How many bikes are there?
    * Answer: 700 bikes
    * Linux command line:
        ```
        bq query --use_legacy_sql=false '
            SELECT count(DISTINCT bike_number) AS total_bikes
            FROM 
                `bigquery-public-data.san_francisco.bikeshare_trips`'
        ```

2. New Query (Run using bq and paste your SQL query and answer the question in a sentence, using properly formatted markdown):

  * How many trips are in the morning vs in the afternoon?
    * Answer: 
        * number of trips in the morning (6-9 AM): 316632  
        * number of trips in the afternoon (4-7 PM): 340697
    * Linux command line:
        ```
        bq query --use_legacy_sql=false '
            SELECT
              COUNT(trip_id) AS trips,
              CASE
                WHEN EXTRACT(hour 
                  FROM start_date) >= 6 
                  AND EXTRACT(hour 
                  FROM start_date) <= 9 
                  THEN "morning trips"
                WHEN EXTRACT(hour
                  FROM start_date) >= 16
                  AND EXTRACT(hour
                  FROM start_date) <= 19 
                  THEN "afternoon trips"
                ELSE "other times trips"
              END
              AS trip_time
            FROM
              `bigquery-public-data.san_francisco.bikeshare_trips`
            GROUP BY
              trip_time'
        ```  

### Project Questions
Identify the main questions you'll need to answer to make recommendations (list
below, add as many questions as you need).

- Question 1: In Part 1 of this project, I realized that station names and ID's (for either Start or End stations) do not match in the bikeshare_trips table. Preliminary inspection showed that it could have most probably happened due to misspelling of the station names for the same station ID. Before going further with analysis, this issue needs to be addressed.

- Question 2: searching for the "commuter trips"; find all trips that meet the following inclusion criteria: started and ended at different stations, occurred only during the week days (i.e. Mon-Fri), started within the usual commute times in the morning (6-9 AM) or in the afternoon (4-7 PM).

- Question 3: the criteria defined in the first question may incorrectly categorize some non-commuter trips as commuter ones. Another question that can help narrowing down the problem would be to ask how far are the stations from each other for each trip. Answering this question is important, because usually the trips happening between very far stations or took long time between close stations may most likely not be categorized under commuter trips. 

- Question 4: which bike stations have been used the most for commuter trips (to/from)?

- Question 5: what is the percentage of the commuter trips as found from the previous questions have been done by _Customers_ vs. _Subscribers_? the reason for asking this question is that it may help to better identify the commuter trips (_Customers_ vs. _Subscribers_).

- Question 6: how the information about the zip code of the customer/subscriber can help to identify the commuter trips as well as better offers? 

### Answers

Answer at least 4 of the questions you identified above You can use either
BigQuery or the bq command line tool.  Paste your questions, queries and
answers below.

- Question 1: Data cleaning (solving the station name-ID mismatch in bikeshare_trips table)
  * Answer: Considering that CREATE TABLE  is not available in Google BigQuery, this process needs to be done in a few steps of query-save-update. 
  * SQL query:

```sql
/* A query to find the mismatch between the start station ID's and names */
#standardSQL
SELECT
  stations.station_id,
  stations.name,
  trips.trip_id,
  trips.start_station_id,
  trips.start_station_name,
FROM
  `bigquery-public-data.san_francisco.bikeshare_stations` AS stations
LEFT JOIN
  `bigquery-public-data.san_francisco.bikeshare_trips` AS trips
ON
  trips.start_station_id = stations.station_id
WHERE
  stations.name <> trips.start_station_name
/* Save results into the temporary table: start_stations_mismatch */
```
--
```sql 
#standardSQL
/*  A query to correct the misspelled start station names */
UPDATE
  `dev-smoke-264503.bikeshare_static.start_stations_mismatch`
SET
  start_station_name = name
WHERE
  start_station_name <> name
```
-- 
--
```sql
/* A query to find the mismatch between the end station ID's and names */
#standardSQL
SELECT
  stations.station_id,
  stations.name,
  trips.trip_id,
  trips.end_station_id,
  trips.end_station_name,
FROM
  `bigquery-public-data.san_francisco.bikeshare_stations` AS stations
LEFT JOIN
  `bigquery-public-data.san_francisco.bikeshare_trips` AS trips
ON
  trips.end_station_id = stations.station_id
WHERE
  stations.name <> trips.end_station_name
/* Save results into the temporary table: end_stations_mismatch */
```
--
```sql
/* A query to correct the misspelled end station names */
#standardSQL
UPDATE
  `dev-smoke-264503.bikeshare_static.end_stations_mismatch`
SET
  end_station_name = name
WHERE
  end_station_name <> name
```
--
```sql
#standardSQL
SELECT
  *
FROM
  `bigquery-public-data.san_francisco.bikeshare_trips` AS trips
LEFT JOIN (
  SELECT
    trip_id AS trip_id2,
    name AS corr_start_station_name
  FROM
    `dev-smoke-264503.bikeshare_static.start_stations_mismatch`) AS corrected
ON
  trips.trip_id = corrected.trip_id2
/* Save results into the temporary table: corrected_start_station_names*/
```
--
```sql
#standardSQL
SELECT
  *
FROM
  `dev-smoke-264503.bikeshare_static.corrected_start_station_names` AS trips
LEFT JOIN (
  SELECT
    trip_id AS trip_id3,
    name AS corr_end_station_name
  FROM
    `dev-smoke-264503.bikeshare_static.end_stations_mismatch`) AS corrected
ON
  trips.trip_id = corrected.trip_id3
/* Save results into the temporary table: corrected_start_end_station_names */
```
--
```sql
/* to update the corrected_start_end_station_names table */
#standardSQL
UPDATE
  `dev-smoke-264503.bikeshare_static.corrected_start_end_station_names`
SET
  corr_end_station_name = end_station_name
WHERE
  corr_end_station_name IS NULL
```
--
```sql
/* Create the final corrected table */
#standardSQL
SELECT
  trip_id,
  duration_sec,
  start_date,
  corr_start_station_name,
  start_station_id,
  end_date,
  corr_end_station_name,
  end_station_id,
  bike_number,
  zip_code,
  subscriber_type
FROM
  `bikeshare_static.corrected_start_end_station_names`
/* save the results into the temporary table: bikeshare_trips_corrected */
```

- Question 2: find all trips that meet the following inclusion criteria: started and ended at different stations, occurred only (i.e. Mon-Fri), initiated within the common commute times (6-9 AM and 4-7 PM) during the day.
  * Answer: 613771 trips met the criteria
  * SQL query: The temporary BigQuery table including the bike trips data with corrected station names as created in the previous question (i.e. _bikeshare_trips_corrected_) has been used to run this query. After running the query, the view will be saved as _bikeshare_static.commuter_trips_ and used to update the temporary table 
  
```sql  
#standardSQL
SELECT
  *
FROM (
  SELECT
    trip_id,
    subscriber_type,
    zip_code,
    bike_number,
    start_station_id,
    end_station_id,
    start_date,
    end_date,
    EXTRACT(DAYOFWEEK
    FROM
      start_date) AS dow_int,
    CAST(ROUND(duration_sec / 60.0) AS INT64) AS duration_minutes,
    CASE EXTRACT(DAYOFWEEK
    FROM
      start_date)
      WHEN 1 THEN "Sunday"
      WHEN 2 THEN "Monday"
      WHEN 3 THEN "Tuesday"
      WHEN 4 THEN "Wednesday"
      WHEN 5 THEN "Thursday"
      WHEN 6 THEN "Friday"
      WHEN 7 THEN "Saturday"
  END
    AS dow_str,
    CASE
      WHEN EXTRACT(DAYOFWEEK FROM start_date) IN (1, 7) THEN "Weekend"
    ELSE
    "Weekday"
  END
    AS dow_weekday,
    EXTRACT(HOUR
    FROM
      start_date) AS start_hour,
    CASE
      WHEN EXTRACT(HOUR FROM start_date) <= 5 OR EXTRACT(HOUR FROM start_date) >= 23 THEN "Nightime"
      WHEN EXTRACT(HOUR
    FROM
      start_date) >= 6
    AND EXTRACT(HOUR
    FROM
      start_date) <= 8 THEN "Morning"
      WHEN EXTRACT(HOUR FROM start_date) >= 9 AND EXTRACT(HOUR FROM start_date) <= 10 THEN "Mid Morning"
      WHEN EXTRACT(HOUR
    FROM
      start_date) >= 11
    AND EXTRACT(HOUR
    FROM
      start_date) <= 13 THEN "Mid Day"
      WHEN EXTRACT(HOUR FROM start_date) >= 14 AND EXTRACT(HOUR FROM start_date) <= 16 THEN "Early Afternoon"
      WHEN EXTRACT(HOUR
    FROM
      start_date) >= 17
    AND EXTRACT(HOUR
    FROM
      start_date) <= 19 THEN "Afternoon"
      WHEN EXTRACT(HOUR FROM start_date) >= 20 AND EXTRACT(HOUR FROM start_date) <= 22 THEN "Evening"
  END
    AS start_hour_str
  FROM
    `dev-smoke-264503.bikeshare_static.bikeshare_trips_corrected` )
WHERE
  (EXTRACT(DATE
    FROM
      start_date) = EXTRACT(DATE
    FROM
      end_date))
  AND (start_station_id <> end_station_id)
  AND (dow_weekday = 'Weekday')
  AND ((start_hour >= 6
      AND start_hour <= 9)
    OR (start_hour >= 16
      AND start_hour <= 19))
ORDER BY
  duration_minutes ASC
```

- Question 3: how far were the stations from each other for each trip?
  * Answer: Trips that started and ended at the same stations were note considered. Distances between any two stations were calculated in miles and were sorted from the farthest (~ 42.4 miles) to the closest (~0.01 miles). One should note that the calculated distance is the geographical distance between two points rather than the 'actual travel distance'; however, it can still be used as a reasonable estimation of the travel distance for the purpose of filtering the non-commuter trip in this project. The first few row of the final table are shown as below.

| Row | trip_id | subscriber_type | zip_code | bike_number | start_station_id | corr_start_station_name         | end_station_id | corr_end_station_name                    | start_date              | end_date                | dow_int | duration_minutes | dow_str   | dow_weekday | start_hour | start_hour_str | distance           |
|-----|---------|-----------------|----------|-------------|------------------|---------------------------------|----------------|------------------------------------------|-------------------------|-------------------------|---------|------------------|-----------|-------------|------------|----------------|--------------------|
| 1   | 385606  | Subscriber      | 94402    | 705         | 36               | California Ave Caltrain Station | 70             | San Francisco Caltrain (Townsend at 4th) | 2014-07-29 18:33:00 UTC | 2014-07-29 19:32:00 UTC | 3       | 59               | Tuesday   | Weekday     | 18         | Afternoon      | 27.720675765087254 |
| 2   | 244143  | Customer        | 94061    | 446         | 45               | Commercial at Montgomery        | 22             | Redwood City Caltrain Station            | 2014-04-10 17:35:00 UTC | 2014-04-10 18:29:00 UTC | 5       | 54               | Thursday  | Weekday     | 17         | Afternoon      | 23.265727405396166 |
| 3   | 933963  | Subscriber      | 94061    | 569         | 74               | Steuart at Market               | 22             | Redwood City Caltrain Station            | 2015-09-16 18:17:00 UTC | 2015-09-16 19:07:00 UTC | 4       | 49               | Wednesday | Weekday     | 18         | Afternoon      | 23.07698330552151  |
  
  * SQL query: The temporary table including the bike trips data with corrected station names as created in the previous question (i.e. bikeshare_trips_corrected) has been used to run this query. After running the query, the view will be saved as _bikeshare_static.station_dist_ and will be joined to the temporary table _bikeshare_static.commuter_trips_, in order to create the a new temporary table called _bikeshare_static.commuter_trips_plus_distance_ that will be used in the future queries. 
  
```sql
#standardSQL
SELECT
  trip_id1 AS trip_id,
  start_station_id,
  corr_start_station_name,
  start_lat,
  start_lot,
  end_station_id,
  corr_end_station_name,
  end_lat,
  end_lot,
  2 * 3961 * ASIN( SQRT( POW(SIN((ACOS(-1) * (end_lat - start_lat)/180 / 2)), 2) 
  + COS(ACOS(-1) *(start_lat)/180) * COS(ACOS(-1) *(end_lat)/180) 
  * POW(SIN((ACOS(-1) *(end_lot - start_lot) /180/ 2)),2) ) ) AS distance
FROM (
  SELECT
    trips.trip_id AS trip_id1,
    trips.start_station_id,
    trips.corr_start_station_name,
    stations.station_id AS station_id1,
    stations.latitude AS start_lat,
    stations.longitude AS start_lot
  FROM
    `bikeshare_static.commuter_trips` AS trips,
    `bigquery-public-data.san_francisco.bikeshare_stations` AS stations
  WHERE
    stations.name = trips.corr_start_station_name) AS starts
FULL OUTER JOIN (
  SELECT
    trips.trip_id AS trip_id2,
    trips.end_station_id,
    trips.corr_end_station_name,
    stations.station_id AS station_id2,
    stations.latitude AS end_lat,
    stations.longitude AS end_lot
  FROM
    `bikeshare_static.commuter_trips` AS trips,
    `bigquery-public-data.san_francisco.bikeshare_stations` AS stations
  WHERE
    stations.name = trips.corr_end_station_name) AS ends
ON
  starts.trip_id1= ends.trip_id2
WHERE
  corr_start_station_name <> corr_end_station_name
ORDER BY
  distance DESC
-- Save the results after running as bikeshare_static.station_dist
```
--
```sql
/* This query is used to update the commuter_trips table with dsitances between stations */
#standardSQL
SELECT
  trips.*,
  dist.distance
FROM
  `bikeshare_static.commuter_trips` AS trips
LEFT JOIN
  `bikeshare_static.station_dist` AS dist
ON
  trips.trip_id = dist.trip_id
ORDER BY
  distance DESC
-- Save the results after running as bikeshare_static.commuter_trips_plus_distance
```

- Question 4: which bike stations have been used the most and the least for commuter trips?
  * Answer: 
      * The most used start and end station was _San Francisco Caltrain (Townsend at 4th)_ with total number of commuter trips of 58546 and 70023, respectively. 
      *  The least used start and end station was _5th S. at E. San Salvador St_ with total number of commuter trips of 7 and 12 respectively.
  * SQL query:
  
```sql
/* This query is to find the most used start stations for commuter trips */
#standardSQL
SELECT 
  COUNT(trip_id) AS trips_from_start_stations, 
   corr_start_station_name
FROM
  `bikeshare_static.commuter_trips_plus_distance`
GROUP BY
  corr_start_station_name
ORDER BY
 trips_from_start_stations desc
```
--
```sql
/* This query is to find the most used end stations for commuter trips */
#standardSQL
SELECT 
  COUNT(trip_id) AS trips_from_start_stations, 
   corr_start_station_name
FROM
  `bikeshare_static.commuter_trips_plus_distance`
GROUP BY
  corr_start_station_name
ORDER BY
 trips_from_start_stations desc
```
  
- Question 5: what is the percentage of the commuter trips as found from the previous questions have been done by 'Customers' vs. 'Subscribers'? how this information can be used to find the most popular commuter tips and/or help with recommending better offers?
  * Answer: The results are summarized in the following table. The information about the subscriber type may not be very useful/relevant to help answering the major questions in this project.

| subscriber_type|trips|total_trips|trips_percent|
| --- | --- | --- | --- | 
|Subscriber | 569598 | 598248 | 95.21 |
|Customer |28650 | 598248 | 4.79 |

  * SQL query:
  
```sql
#standradSQL
SELECT
  subscriber_type,
  trips,
  total_trips,
  ROUND(100 * trips/total_trips, 2) AS trips_percent
FROM (
  SELECT
    subscriber_type,
    COUNT(*) AS trips
  FROM (
    SELECT
      CASE
        WHEN subscriber_type = 'Customer' THEN 'Customer'
        WHEN subscriber_type = 'Subscriber' THEN 'Subscriber'
      ELSE
      'None'
    END
      AS subscriber_type
    FROM
       `bikeshare_static.commuter_trips_plus_distance`)
  GROUP BY
    subscriber_type),
  (
  SELECT
    COUNT(*) AS total_trips
  FROM
     `bikeshare_static.commuter_trips_plus_distance`)
ORDER BY
  trips_percent DESC
```
  
- Question 6: how the information about the zip code of the customer/subscriber can help to identify the commuter trips as well as better offers?
  * Answer: The most and least appeared subscribers _valid_ zip codes among commuter trips are, respectively, summarized in the top table. Please note that only _valid_ zip code entries have been considered. In the next step, I found the zip code for the top 5 most used start and end stations (from Q4) using the Google Map and compared them with the top most appeared subscriber zip code in this question. The results are summarized in the bottom table. Results from this table shows a failry good match between the most used zip codes of the subscribers and those of the most used stations. This means that most likely the most used stations (i.e. with the highest number of trips from/to) can be reasonably used to define the most popular commuter trips. 

|trips | subscriber_type|zip code|
| --- | --- | --- |
|58928|Subscriber | 94107 | 
|1144|Subscriber | 94523 |
   
|most used start st. | zip code (Google map)|most used end st. | zip code (Google map)| most frequent subscriber zip code|
|--- | --- | --- |--- |--- |
|San Francisco Caltrain (Townsend at 4th)| 94107 | San Francisco Caltrain (Townsend at 4th) |94107 |94107 |
|  San Francisco Caltrain 2 (330 Townsend) | 94107 | San Francisco Caltrain 2 (330 Townsend) |94107 |94105 |
| Temporary Transbay Terminal (Howard at Beale) | 94105 | 2nd at Townsend |94107 |94133 |
|Harry Bridges Plaza (Ferry Building)| 94111 | Harry Bridges Plaza (Ferry Building) |94111 |94103 |
| Steuart at Market | 94105 | Steuart at Market |94105 |94111 |

  * SQL query:
  
```sql
-- Query used to extract the numbers in the top table
#standardSQL
SELECT
  COUNT(trip_id) AS trips,
  subscriber_type,
  zip_code
FROM
  `bikeshare_static.commuter_trips_plus_distance`
WHERE
  -- to filter zip code entries with more or less than 5 characters
  zip_code LIKE '_____'
  -- to filter zip code entries with letters
  AND NOT REGEXP_CONTAINS(zip_code, r"[a-zA-Z]")
GROUP BY
  zip_code,
  subscriber_type
ORDER BY
  trips DESC
```
--
```sql
-- Query used to extract the numbers in the bottom table
#standardSQL
SELECT *
FROM ((
    SELECT
      COUNT(trip_id) AS number_trips,
      corr_start_station_name as station
    FROM
      `bikeshare_static.commuter_trips_plus_distance`
    GROUP BY
      station
    ORDER BY
      number_trips DESC
    LIMIT
      5 )
  UNION ALL (
    SELECT
      COUNT(trip_id) AS number_trips,
      corr_end_station_name as station
    FROM
      `bikeshare_static.commuter_trips_plus_distance`
    GROUP BY
      station
    ORDER BY
      number_trips DESC
    LIMIT
      5 )
  UNION ALL (
    SELECT
      COUNT(trip_id) AS number_trips,
      zip_code
    FROM
      `bikeshare_static.commuter_trips_plus_distance`
    WHERE
      -- to filter zip code entries with more or less than 5 characters
      zip_code LIKE '_____'
      -- to filter zip code entries with letters
      AND NOT REGEXP_CONTAINS(zip_code, r"[a-zA-Z]")
    GROUP BY
      zip_code,
      subscriber_type
    ORDER BY
      number_trips DESC
    LIMIT
      5))
```
  
## Part 3 - Employ notebooks to synthesize query project results

### Get Going

Create a Jupyter Notebook against a Python 3 kernel named Project_1.ipynb in the assignment branch of your repo.

#### Run queries in the notebook 

At the end of this document is an example Jupyter Notebook you can take a look at and run.  

You can run queries using the "bang" command to shell out, such as this:

```
! bq query --use_legacy_sql=FALSE '<your-query-here>'
```

- NOTE: 
- Queries that return over 16K rows will not run this way, 
- Run groupbys etc in the bq web interface and save that as a table in BQ. 
- Max rows is defaulted to 100, use the command line parameter `--max_rows=1000000` to make it larger
- Query those tables the same way as in `example.ipynb`

Or you can use the magic commands, such as this:

```sql
%%bigquery my_panda_data_frame

select start_station_name, end_station_name
from `bigquery-public-data.san_francisco.bikeshare_trips`
where start_station_name <> end_station_name
limit 10
```

```python
my_panda_data_frame
```

#### Report in the form of the Jupter Notebook named Project_1.ipynb

- Using markdown cells, MUST definitively state and answer the two project questions:

  * What are the 5 most popular trips that you would call "commuter trips"? 
  
  * What are your recommendations for offers (justify based on your findings)?

- For any temporary tables (or views) that you created, include the SQL in markdown cells

- Use code cells for SQL you ran to load into Pandas, either using the !bq or the magic commands

- Use code cells to create Pandas formatted output tables (at least 3) to present or support your findings

- Use code cells to create simple data visualizations using Seaborn / Matplotlib (at least 2) to present or support your findings

### Resource: see example .ipynb file 

[Example Notebook](example.ipynb)

