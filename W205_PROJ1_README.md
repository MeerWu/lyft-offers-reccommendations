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

Make sure you receive the emails related to your repository! Your project feedback will be given as comment on the pull request. When you receive the feedback, you can address problems or simply comment that you have read the feedback. 
AFTER receiving and answering the feedback, merge you PR to master. Your project only counts as complete once this is done.

---

## Parts 1, 2, 3

We have broken down this project into 3 parts, about 1 week's work each to help you stay on track.

**You will only turn in the project once at the end of part 3!**

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
  * Answer: 983648
  * SQL query: ```SELECT COUNT(*) FROM `bigquery-public-data.san_francisco.bikeshare_trips`
               ```

- What is the earliest start date and time and latest end date and time for a trip?
  * Answer: The earliest start datetime is 2013/08/29 9:08am PST, and the latest end datetime is 2016/08/31 11:48pm PST.
  * SQL query: ```SELECT MIN(start_date) AS earliest_start, MAX(end_date) AS latest_end FROM `bigquery-public-data.san_francisco.bikeshare_trips`
               ```

- How many bikes are there?
  * Answer: 700
  * SQL query: ```SELECT COUNT(distinct bike_number) FROM `bigquery-public-data.san_francisco.bikeshare_trips`
               ```

### Questions of your own
- Make up 3 questions and answer them using the Bay Area Bike Share Trips Data.  These questions MUST be different than any of the questions and queries you ran above.

- Question 1: How long is the shortest and longest trip?
  * Answer: The shortest trip lasted 60 seconds. The longest trip lasted 199 days, 21 hours, 20 minutes, and 0 seconds.
  * SQL query: 
  ```
  SELECT FORMAT("%i seconds", MIN(duration_sec)) AS shortest_trip,
  FORMAT(
  '%i days %i hours %i minutes %i seconds',
  unix_date(date(timestamp_seconds(MAX(duration_sec)))),
  EXTRACT(hour from timestamp_seconds(MAX(duration_sec))),
  EXTRACT(minute from timestamp_seconds(MAX(duration_sec))),
  EXTRACT(second from timestamp_seconds(MAX(duration_sec)))) AS longest_trip
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  ```

- Question 2: What are the start and end stations of the most popular trip? How many trips have been taken for this particular route?
  * Answer: Harry Bridges Plaza (Ferry Building) to Embarcadero at Sansome with 9150 trips taken. (interesting thing I noticed: The 3rd and 4th most popular trips have start and end stations that mirror each other between 2nd at Townsend station and Harry Bridges Plaza (Ferry Building) station. This is a good indicator that this might be a popular commuter trip, but more queries are needed.)
  * SQL query:
  ```
  SELECT start_station_name, end_station_name, COUNT(*) AS number_of_trips
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  GROUP BY start_station_name, end_station_name ORDER BY number_of_trips DESC
  ```

- Question 3: What are the top 6 busiest hours (ie. hours with the most number of trips)? For subscribers only?
  * Answer: The top 6 busiest hours are the same for all users and for only subscribers.
    1. 8am (132,464 trips, 127,171 trips for subscribers only)
    2. 5pm (126,302 trips, 114,915 trips for subscribers only)
    3. 9am (96,118 trips, 89,546 trips for subscribers only)
    4. 4pm (88,755 trips, 76,051 trips for subscribers only)
    5. 6pm (84,569 trips, 75,798 trips for subscribers only)
    6. 7am (67,531 trips, 64,946 trips for subscribers only)
  * SQL query:
  ```
  SELECT EXTRACT(hour from start_date) AS start_hour, COUNT(*) AS number_of_trips
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  GROUP BY start_hour ORDER BY number_of_trips DESC
  
  SELECT EXTRACT(hour from start_date) AS start_hour, COUNT(*) AS number_of_trips
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  WHERE subscriber_type = 'Subscriber'
  GROUP BY start_hour ORDER BY number_of_trips DESC
  ```

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
    * Answer: 983648
    * SQL query: ```bq query --nouse_legacy_sql 'SELECT COUNT(*) FROM `bigquery-public-data.san_francisco.bikeshare_trips`'```

  * What is the earliest start time and latest end time for a trip?
    * Answer: Earliest: 2013-08-29 09:08:00 PST. Latest: 2016-08-31 23:48:00 PST
    * SQL query: 
    ```
    bq query --nouse_legacy_sql 'SELECT MIN(start_date) AS earliest_start, MAX(end_date) AS latest_end FROM `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```

  * How many bikes are there?
    * Answer: 700 bikes
    * SQL query: ```bq query --nouse_legacy_sql 'SELECT COUNT(distinct bike_number) FROM `bigquery-public-data.san_francisco.bikeshare_trips`'```

2. New Query (Run using bq and paste your SQL query and answer the question in a sentence, using properly formatted markdown):

  * How many trips are in the morning vs in the afternoon?
    * Answer: 412339 trips in the morning (midnight-11:59:59am) and 475768 trips in the afternoon (12:00:00pm-6:59:59pm). Trips that started in the morning are considered morning trips, while trips that started in the afternoon are considered afternoon trips.
    * SQL query: 
    ```
    bq query --nouse_legacy_sql 'SELECT COUNT(*) FROM `bigquery-public-data.san_francisco.bikeshare_trips` WHERE EXTRACT(hour from start_date) BETWEEN 0 AND 11'
    
    bq query --nouse_legacy_sql 'SELECT COUNT(*) FROM `bigquery-public-data.san_francisco.bikeshare_trips` WHERE EXTRACT(hour from start_date) BETWEEN 12 AND 18'
    ```


### Project Questions
Identify the main questions you'll need to answer to make recommendations (list
below, add as many questions as you need).

- Question 1: What are the peak hours weekdays and for weekends? Are they different?

- Question 2: What are the most popular trips in the morning vs. in the afternoon?

- Question 3: What are the most popular trips during peak hours on weekdays? Are they different from the most popular trips during peak hours on weekends?

- Question 4: What is the average trip duration for each of the 5 most popular commuter trips?

- Question 5: What is the average trip duration of trips that started during weekday peak hours? (helps answer: What are reasonable lengths of commute times?)

- Question 6: What is the shortest distance between each of the 5 most popular commuter trips?

- Question 7: Are most commuters subscribers or customers?

- Question 8: How many trips of the 5 most popular weekday peak hour station pairs are taken in the morning vs. afternoon? How popular were they?

### Answers

Answer at least 4 of the questions you identified above You can use either
BigQuery or the bq command line tool.  Paste your questions, queries and
answers below.

- Question 1: What are the peak hours for weekdays and for weekends? Are they different?
  * Answer: Yes, they're different. Weekday peak hours are from 7am to 10am (up to and including 9:59am) and 4pm to 7pm (up to and including 6:59pm), while weekend peak hours range from 11am to 5pm (up to and including 4:59pm).
  * SQL query:
    ```
    bq query --nouse_legacy_sql '
      SELECT EXTRACT(hour from start_date) AS start_hour, COUNT(*) AS number_of_trips FROM `bigquery-public-data.san_francisco.bikeshare_trips`
      WHERE EXTRACT(DAYOFWEEK from start_date) BETWEEN 2 AND 6 GROUP BY start_hour ORDER BY number_of_trips DESC'
      
    bq query --nouse_legacy_sql '
      SELECT EXTRACT(hour from start_date) AS start_hour, COUNT(*) AS number_of_trips FROM `bigquery-public-data.san_francisco.bikeshare_trips`
      WHERE EXTRACT(DAYOFWEEK from start_date) NOT BETWEEN 2 AND 6 GROUP BY start_hour ORDER BY number_of_trips DESC'
    ```

- Question 2: What are the 5 most popular trips during peak hours on weekdays? How does it compare to the 5 most popular trips during peak hours on weekends?
  * Answer:
    * Weekday:
      1. 2nd at Townsend &#8594; Harry Bridges Plaza (Ferry Building): 5165 trips
      2. Harry Bridges Plaza (Ferry Building) &#8594; 2nd at Townsend: 5127 trips
      3. San Francisco Caltrain 2 (330 Townsend) &#8594; Townsend at 7th: 5040 trips
      4. Embarcadero at Sansome &#8594; Steuart at Market: 4904 trips
      5. Embarcadero at Folsom &#8594; San Francisco Caltrain (Townsend at 4th): 4756 trips

    * Weekend:
      1. Harry Bridges Plaza (Ferry Building) &#8594; Embarcadero at Sansome: 1406 trips
      2. Embarcadero at Sansome &#8594; Embarcadero at Sansome: 797 trips
      3. Harry Bridges Plaza (Ferry Building) &#8594; Harry Bridges Plaza (Ferry Building): 743 trips
      4. Embarcadero at Sansome &#8594; Harry Bridges Plaza (Ferry Building): 687 trips
      5. Embarcadero at Vallejo &#8594; Embarcadero at Sansome: 412 trips
  * SQL query:
    ```
    bq query --nouse_legacy_sql '
    SELECT start_station_name, end_station_name, COUNT(*) AS number_of_trips,
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    WHERE EXTRACT(dayofweek from start_date) BETWEEN 2 AND 6 AND (EXTRACT(hour from start_date) BETWEEN 7 AND 9 OR EXTRACT(hour from start_date) BETWEEN 16 AND 18)
    GROUP BY start_station_name, end_station_name ORDER BY number_of_trips DESC'
    
    bq query --nouse_legacy_sql '
    SELECT start_station_name, end_station_name, COUNT(*) AS number_of_trips FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    WHERE EXTRACT(hour from start_date) BETWEEN 11 AND 16 AND EXTRACT(dayofweek from start_date) NOT BETWEEN 2 AND 6
    GROUP BY start_station_name, end_station_name ORDER BY number_of_trips DESC'
    ```

- Question 3: How many trips of the 5 most popular weekday peak hour station pairs are taken in the morning vs. afternoon? How popular were they?
  * Answer: All trips were within the top 10 most popular morning/afternoon peak hour trips. Number in parenthesis at the end indicate the trip's ranking by trip frequency (1=most popular, 2=2nd most popular, etc.).
    * During morning peak hours:
      * Harry Bridges Plaza (Ferry Building) &#8594; 2nd at Townsend: 4509 trips (1)
      * San Francisco Caltrain 2 (330 Townsend) &#8594; Townsend at 7th: 3512 trips (3)
      * Steuart at Market &#8594; Embarcadero at Sansome: 2777 trips (10)
      * San Francisco Caltrain (Townsend at 4th) &#8594; Embarcadero at Folsom: 3364 trips (4)
    * During afternoon peak hours:
      * 2nd at Townsend &#8594; Harry Bridges Plaza (Ferry Building): 4191 trips (1)
      * Townsend at 7th &#8594; San Francisco Caltrain 2 (330 Townsend): 2798 trips (8)
      * Embarcadero at Sansome &#8594; Steuart at Market: 3863 trips (3)
      * Embarcadero at Folsom &#8594; San Francisco Caltrain (Townsend at 4th): 4022 trips (2)

 
  * SQL query:
  ```
  bq query --nouse_legacy_sql '
  SELECT start_station_name, end_station_name, COUNT(*) AS number_of_trips, 
  CASE 
    WHEN EXTRACT(hour from start_date) BETWEEN 7 AND 9 THEN "morning peak" 
    WHEN EXTRACT(hour from start_date) BETWEEN 16 AND 18 THEN "afternoon peak" 
    ELSE "not peak" 
  END AS trip_time FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
  WHERE EXTRACT(dayofweek from start_date) BETWEEN 2 AND 6 AND (EXTRACT(hour from start_date) BETWEEN 7 AND 9 OR EXTRACT(hour from start_date) BETWEEN 16 AND 18) 
  GROUP BY start_station_name, end_station_name, trip_time ORDER BY number_of_trips DESC'
  ```

- Question 4: What is the shortest distance of each of the 5 most popular commuter trips?
  * Answer: 886.27 meters, or roughly half a mile.
  * SQL query:
  ```
  WITH trips_and_stations AS
    (WITH start_loc_trips AS
      (SELECT `bigquery-public-data.san_francisco.bikeshare_trips`.start_station_name, `bigquery-public-data.san_francisco.bikeshare_trips`.end_station_name, 
       COUNT(*) AS number_of_trips, AVG(`bigquery-public-data.san_francisco.bikeshare_trips`.duration_sec) AS mean_duration_sec,
       `bigquery-public-data.san_francisco.bikeshare_stations`.longitude AS start_lon, `bigquery-public-data.san_francisco.bikeshare_stations`.latitude AS start_lat
       FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
       INNER JOIN `bigquery-public-data.san_francisco.bikeshare_stations` ON `bigquery-public-data.san_francisco.bikeshare_trips`.start_station_name=`bigquery-public-data.san_francisco.bikeshare_stations`.name
       WHERE EXTRACT(dayofweek from `bigquery-public-data.san_francisco.bikeshare_trips`.start_date) BETWEEN 2 AND 6 AND
       (EXTRACT(hour from `bigquery-public-data.san_francisco.bikeshare_trips`.start_date) BETWEEN 7 AND 9 OR EXTRACT(hour from `bigquery-public-     data.san_francisco.bikeshare_trips`.start_date) BETWEEN 16 AND 18)
       GROUP BY start_station_name, end_station_name, start_lon, start_lat)
    SELECT start_loc_trips.start_station_name, start_loc_trips.end_station_name, start_loc_trips.number_of_trips, start_loc_trips.mean_duration_sec, start_loc_trips.start_lon, start_loc_trips.start_lat,
    `bigquery-public-data.san_francisco.bikeshare_stations`.longitude AS end_lon, `bigquery-public-data.san_francisco.bikeshare_stations`.latitude AS end_lat
    FROM start_loc_trips
    INNER JOIN `bigquery-public-data.san_francisco.bikeshare_stations` ON start_loc_trips.end_station_name=`bigquery-public-data.san_francisco.bikeshare_stations`.name
    WHERE start_loc_trips.start_station_name != start_loc_trips.end_station_name)
  SELECT start_station_name, end_station_name, number_of_trips, mean_duration_sec, ST_DISTANCE(ST_GEOGPOINT(start_lon, start_lat), ST_GEOGPOINT(end_lon, end_lat)) AS trip_dist_meter FROM trips_and_stations
  ORDER BY number_of_trips DESC
  ```
  
- Question 5: What is the average trip duration and the standard deviation of trip duration of trips that started during weekday peak hours?
  * Answer: The average trip duration is 11 minutes 6 seconds. The standard deviation is 30 minutes 16 seconds. Trips that lasted longer than a day were excluded since we're only interested in commuter trips, which should occur on a daily basis.
  * SQL query:
  ```
  SELECT 
  FORMAT(
  '%i days %i hours %i minutes %i seconds',
  unix_date(date(timestamp_seconds(CAST(ROUND(AVG(duration_sec)) AS int)))),
  EXTRACT(hour from timestamp_seconds(CAST(ROUND(AVG(duration_sec)) AS int))),
  EXTRACT(minute from timestamp_seconds(CAST(ROUND(AVG(duration_sec)) AS int))),
  EXTRACT(second from timestamp_seconds(CAST(ROUND(AVG(duration_sec)) AS int)))) AS duration_mean,
  FORMAT(
  '%i days %i hours %i minutes %i seconds',
  unix_date(date(timestamp_seconds(CAST(ROUND(STDDEV_SAMP(duration_sec)) AS int)))),
  EXTRACT(hour from timestamp_seconds(CAST(ROUND(STDDEV_SAMP(duration_sec)) AS int))),
  EXTRACT(minute from timestamp_seconds(CAST(ROUND(STDDEV_SAMP(duration_sec)) AS int))),
  EXTRACT(second from timestamp_seconds(CAST(ROUND(STDDEV_SAMP(duration_sec)) AS int)))) AS duration_stddev
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  WHERE EXTRACT(dayofweek from start_date) BETWEEN 2 AND 6 AND (EXTRACT(hour from start_date) BETWEEN 7 AND 9 OR EXTRACT(hour from start_date) BETWEEN 16 AND 18) AND
        duration_sec < 86400
  ```
  
- Question 6: What is the average trip duration for each of the 5 most popular commuter trips?
  * Answer: Maximum trip duration is set as the mean trip duraion + 2 standard deviations, which is 1 hour 11 minutes and 38 seconds (4298 seconds).
      1. 2nd at Townsend &#8594; Harry Bridges Plaza (Ferry Building): 8 minutes 18 seconds
      2. Harry Bridges Plaza (Ferry Building) &#8594; 2nd at Townsend: 9 minutes 42 seconds
      3. San Francisco Caltrain 2 (330 Townsend) &#8594; Townsend at 7th: 4 minutes 15 seconds
      4. Embarcadero at Sansome &#8594; Steuart at Market: 6 minutes 41 seconds
      5. Embarcadero at Folsom &#8594; San Francisco Caltrain (Townsend at 4th): 10 minutes 10 seconds
  * SQL query:
  ```
  SELECT start_station_name, end_station_name, COUNT(*) AS number_of_trips, 
  FORMAT(
  '%i days %i hours %i minutes %i seconds',
  unix_date(date(timestamp_seconds(CAST(ROUND(AVG(duration_sec)) AS int)))),
  EXTRACT(hour from timestamp_seconds(CAST(ROUND(AVG(duration_sec)) AS int))),
  EXTRACT(minute from timestamp_seconds(CAST(ROUND(AVG(duration_sec)) AS int))),
  EXTRACT(second from timestamp_seconds(CAST(ROUND(AVG(duration_sec)) AS int)))) AS mean_duration_sec
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  WHERE EXTRACT(dayofweek from start_date) BETWEEN 2 AND 6 AND (EXTRACT(hour from start_date) BETWEEN 7 AND 9 OR EXTRACT(hour from start_date) BETWEEN 16 AND 18) AND
        duration_sec < 4298
  GROUP BY start_station_name, end_station_name ORDER BY number_of_trips DESC
  ```

- Question 7: Are most commuters subscribers or customers? (first check overall ratio between subscirbers and customers, then check the percentage of subscribers for commuters vs. non-commuters)
  * Answer: Over 95% of commuters- defined as users who started trips during weekday peak hours, the range of which is specified in Question 1 of this part- of the 10 most popular trips are subscribers. To ensure that this is not simply a phenomenon of having more subscribers (~86%) than customers (~14%) in the dataset, I investigated subscriber types of trips made during weekend peak hours since commuters typically only make trips for commuting purposes during weekdays. The proportion of subscribers during weekend peak hours mirrors those during weekday peak hours. For each of the 5 most popular trips during weekend peak hours, over 60% of their trips are completed by customers. This shows that most commuters are subscribers.
  * SQL query:
  ```
  Overall ratio of subscriber type:
  
  SELECT subscriber_type, COUNT(*) AS type_count, ROUND(COUNT(*)*100/983648, 1) AS percentage FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  GROUP BY subscriber_type
  
  
  For commuters:
  
  SELECT start_station_name, end_station_name, COUNT(*) AS number_of_trips,
  ROUND(SUM(CASE WHEN subscriber_type="Subscriber" THEN 1 ELSE 0 END)/COUNT(*)*100,1) AS prop_subscribers FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  WHERE EXTRACT(dayofweek from start_date) BETWEEN 2 AND 6 AND (EXTRACT(hour from start_date) BETWEEN 7 AND 9 OR EXTRACT(hour from start_date) BETWEEN 16 AND 18)
  GROUP BY start_station_name, end_station_name ORDER BY number_of_trips DESC
  
  
  For non-commuters:
  
  SELECT start_station_name, end_station_name, COUNT(*) AS number_of_trips, 
  ROUND(SUM(CASE WHEN subscriber_type="Subscriber" THEN 1 ELSE 0 END)/COUNT(*)*100,1) AS prop_subscribers
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  WHERE EXTRACT(hour from start_date) BETWEEN 11 AND 16 AND EXTRACT(dayofweek from start_date) NOT BETWEEN 2 AND 6
  GROUP BY start_station_name, end_station_name ORDER BY number_of_trips DESC
  ```

---

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

