# Project: Datalake application with Sparkify data
### 1. Introduction

Sparkify uses their own user base and song database to develop. Now, after using Data warehouse in operation, they change to use datalake. Their data reside in S3, in directory of JSON logs on user activities as well as a directory with JSON metadata on their app

In this project, I am tasked with building an ETL pipeline that **Extract** their data from S3, processed by Spark and **Load** back to S3 a set as dimensional tables. **This helps analytics team to continue find Insight in what songs their users are listening to**

I'll be able to test my database and ETL pipeline by running querries with requirement from analytics team from Sparkify and compare the result with their expectations.

### 2. Project description

In this project, I apply:
- Spark and data lakes to **Build ETL pipeline** for a data lake hosted on S3
  - in detail:
    - Load data from S3
    - Process the data into analytics tables using Spark 
    - Load them back into S3
    -> deploy this Spark process on a cluster using AWS
    
### 3. Project dataset

We will be working on 2 dataset that resides on S3. Links of S3 for each:

- Song data: `s3://udacity-dend/song_data`
- Log data: `s3://udacity-dend/log_data`  
