# Project: Datalake application with Sparkify data
## 1. Introduction

Sparkify uses their own user base and song database to develop. Now, after using Data warehouse in operation, they change to use datalake. Their data reside in S3, in directory of JSON logs on user activities as well as a directory with JSON metadata on their app

In this project, I am tasked with building an ETL pipeline that **Extract** their data from S3, processed by Spark and **Load** back to S3 a set as dimensional tables. **This helps analytics team to continue find Insight in what songs their users are listening to**

I'll be able to test my database and ETL pipeline by running querries with requirement from analytics team from Sparkify and compare the result with their expectations.

## 2. Project description

In this project, I apply:
- Spark and data lakes to **Build ETL pipeline** for a data lake hosted on S3
  - in detail:
    - Load data from S3
    - Process the data into analytics tables using Spark 
    - Load them back into S3
    -> deploy this Spark process on a cluster using AWS
    
## 3. Project dataset

We will be working on 2 dataset that resides on S3. Links of S3 for each:

- Song data: `s3a://udacity-dend/song_data`
- Log data: `s3a://udacity-dend/log_data`  
### Song dataset 
The first dataset is a subset of real data from the Million Song Dataset. Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. For example, here are filepaths to two files in this dataset.

`song_data/A/B/C/TRABCEI128F424C983.json`

`song_data/A/A/B/TRAABJL12903CDCF1A.json`

This is an example of what a single song files, look likes:

`{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}`
### Log dataset
These activity logs from an imaginary music stream app. The log files in the dataset are partitioned by year and month
Here is the example:
`log_data/2018/11/2018-11-12-events.json`

And the dataframe of log files look like:
![log_file](https://video.udacity-data.com/topher/2019/February/5c6c3f0a_log-data/log-data.png)

### Output:
Using our own S3 to store the data:
`"s3a://aws-emr-resources-449629588483-us-west-2/sparkify/"`
## 4. Project instructions:
### a. Read data from S3, process that data using Spark and write back to S3
- Read data from S3 of Udacity hosted by assign inputs with log and song, process with Spark by SparkSession
- an environment with AWS, using `dl.cfg`

**NOTICE:**

Remember to create ACCESS KEY on crendential **NOT ON IAM ROLE** because it got conflict with the s3 url from Udacity.
We impletement with this following steps:

![Step 1](https://udacity-user-uploads.s3.us-west-2.amazonaws.com/uploads/user-uploads/6a156899-077c-48b2-b356-cf233e97d8d3-mobile.png)

We go to *Access Managemement* then *Access key*

![Step 2](https://udacity-user-uploads.s3.us-west-2.amazonaws.com/uploads/user-uploads/29c25a25-a212-42ca-b324-51d2d2e4ad64-mobile.png)

### b. Manipulate tables
- Simplify and fasten the process on READ & WRITE S3 and to S3, test only with `/A/A/A/*` for data songs url
- Read the files with `spark.read.json` drop the duplicates and cache() for lazy transformation. [Read more about cache](https://towardsdatascience.com/best-practices-for-caching-in-spark-sql-b22fb0f02d34). With logs input_data, we filter with `df.page=="NextSong"` for purpose of usage.
- The above approach is applied for users_table, songs_table and artists_table
Here are the Schemas:
<h4>songs_table</h4>

~~~~
root
 |-- artist_id: string (nullable = true)
 |-- artist_latitude: double (nullable = true)
 |-- artist_location: string (nullable = true)
 |-- artist_longitude: double (nullable = true)
 |-- artist_name: string (nullable = true)
 |-- duration: double (nullable = true)
 |-- num_songs: long (nullable = true)
 |-- song_id: string (nullable = true)
 |-- title: string (nullable = true)
 |-- year: long (nullable = true)
~~~~

<h4>artists_table</h4>

~~~~
root
 |-- artist_id: string (nullable = true)
 |-- artist_name: string (nullable = true)
 |-- artist_location: string (nullable = true)
 |-- artist_latitude: double (nullable = true)
 |-- artist_longitude: double (nullable = true)

~~~~

<h4>users_table</h4>

~~~~
root
 |-- firstName: string (nullable = true)
 |-- lastName: string (nullable = true)
 |-- gender: string (nullable = true)
 |-- level: string (nullable = true)
 |-- userId: string (nullable = true)

~~~~

**Time table**
- create timestamp and datetime with @udf with `get_timestamp` from column ts of the logs df.
- after that, extract into a separate time_table with units of time

~~~~
root
 |-- start_time: timestamp (nullable = true)
 |-- hour: integer (nullable = true)
 |-- day: integer (nullable = true)
 |-- week: integer (nullable = true)
 |-- month: integer (nullable = true)
 |-- year: integer (nullable = true)
 |-- weekday: integer (nullable = true)
~~~~

**Song_plays**
- in this table, partition the table by year and month so we need to join the df_song and df_log with artist is primary key.

~~~~
root
 |-- start_time: timestamp (nullable = true)
 |-- userId: string (nullable = true)
 |-- level: string (nullable = true)
 |-- sessionId: long (nullable = true)
 |-- location: string (nullable = true)
 |-- userAgent: string (nullable = true)
 |-- song_id: string (nullable = true)
 |-- artist_id: string (nullable = true)
 |-- songplay_id: long (nullable = false)
~~~~

### c. Write ouput to S3:
write tables to parquet files with:
`name_table.write.parquet("{}name/name_table.parquet".format(output_data))`
with output_data is the link of S3 from AWS.
**Write on song and song_play tables**

We partion these 2 tables by Year and artist_id/Month to work with log files so add `.partitionBy(element1,element2)`
#### d. On terminal
Run the etl.py on terminal with `python etl.py` to auto generate the file and check the tables on S3

## 4. Result
![img](https://github.com/alexdinh1997/DataLakeWithSpark/blob/main/Project4_Data_Lakes/img/Screenshot%202021-05-06%20at%2014.48.24.png?raw=true)
