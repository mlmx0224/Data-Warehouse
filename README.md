#Introduction

A music streaming startup, Sparkify, has grown their user base and song database and want to move their processes and data onto the cloud. Their data resides in S3, in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

As their data engineer, I am tasked with building an ETL pipeline that extracts their data from S3, stages them in Redshift, and transforms data into a set of dimensional tables for their analytics team to continue finding insights in what songs their users are listening to. I'll be able to test my database and ETL pipeline by running queries given to me by the analytics team from Sparkify and compare my results with their expected results.

#Project Description

In this project, I'll apply what I've learned on data warehouses and AWS to build an ETL pipeline for a database hosted on Redshift. To complete the project, I will need to load data from S3 to staging tables on Redshift and execute SQL statements that create the analytics tables from these staging tables.

#Dataset

I'll be working with two datasets that reside in S3. Here are the S3 links for each:

    `Song data: s3://udacity-dend/song_data`
    `Log data: s3://udacity-dend/log_data`

Log data json path: `s3://udacity-dend/log_json_path.json`

###Song Dataset

The first dataset is a subset of real data from the Million Song Dataset. Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. For example, here are filepaths to two files in this dataset.

```
song_data/A/B/C/TRABCEI128F424C983.json
song_data/A/A/B/TRAABJL12903CDCF1A.json
```

And below is an example of what a single song file, TRAABJL12903CDCF1A.json, looks like.

```
{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}
```

###Log Dataset

The second dataset consists of log files in JSON format generated by this event simulator based on the songs in the dataset above. These simulate activity logs from a music streaming app based on specified configurations.

The log files in the dataset I'll be working with are partitioned by year and month. For example, here are filepaths to two files in this dataset.

```
log_data/2018/11/2018-11-12-events.json
log_data/2018/11/2018-11-13-events.json
```

#Schema for Song Play Analysis

Using the song and log datasets, I create a star schema optimized for queries on song play analysis. This includes the following tables.
###Fact Table

* songplays - records in log data associated with song plays i.e. records with page NextSong
        songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

###Dimension Tables

* users - users in the app
        user_id, first_name, last_name, gender, level
* songs - songs in music database
        song_id, title, artist_id, year, duration
* artists - artists in music database
        artist_id, name, location, latitude, longitude
* time - timestamps of records in songplays broken down into specific units
        start_time, hour, day, week, month, year, weekday

#How to run
1. Run `create_tables.py` in the termial by using the command 
```
python create_tables.py
```
-- This step will get the AWS IAM credential and the redshift endpoint.
-- This step will create each table and staging table, whilst drop each table if it exsits in AWS Redshift.

2. Run `etl.py` in the termail by using the command `python elt.py` to build the ETL pipeline.
-- This step will load the raw data (json format) from AWS S3 bucket into AWS Redshift staging table.
-- This step also will generate the insert query which is to select the data from staging table and then insert data into table.

5. Check the table and data in AWS Redshift


