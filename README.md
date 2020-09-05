## Sparkify Data lake Project

A music streaming startup, Sparkify, has grown their user base and song database even more and want to move their data warehouse to a data lake. Their data resides in S3, in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

This repository is an ETL pipeline that extracts their data from S3, processes them using Spark, and loads the data back into S3 as a set of dimensional tables. This will allow their analytics team to continue finding insights in what songs their users are listening to.


#### Datasets

Song data: s3://udacity-dend/song_data

Log data: s3://udacity-dend/log_data

**Song Dataset**

The first dataset is a subset of real data from the Million Song Dataset. Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. For example, here are filepaths to two files in this dataset.

song_data/A/B/C/TRABCEI128F424C983.json

song_data/A/A/B/TRAABJL12903CDCF1A.json

And below is an example of what a single song file, TRAABJL12903CDCF1A.json, looks like.

{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}

**Log Dataset**

The second dataset consists of log files in JSON format generated by this event simulator based on the songs in the dataset above. These simulate app activity logs from an imaginary music streaming app based on configuration settings.

The log files in the dataset you'll be working with are partitioned by year and month. For example, here are filepaths to two files in this dataset.

log_data/2018/11/2018-11-12-events.json

log_data/2018/11/2018-11-13-events.json

#### Schema for Songplay Analysis

Using the song and log datasets, you'll need to create a star schema optimized for queries on song play analysis. This includes the following tables.

**Fact Table**

songplays - records in log data associated with song plays i.e. records with page NextSong

    songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

**Dimension Tables**

users - users in the app

    user_id, first_name, last_name, gender, level

songs - songs in music database
    
    song_id, title, artist_id, year, duration

artists - artists in music database
    
    artist_id, name, location, lattitude, longitude

time - timestamps of records in songplays broken down into specific units

    start_time, hour, day, week, month, year, weekday
    
#### Files

The project template includes three files:

- etl.py: reads data from S3, processes that data using Spark, and writes them back to S3.

- dl.cfg: contains your AWS credentials.

- ETL-step-by-step-breakdown.ipynb: step-by-step breakdown of the entire ETL pipleine.

- Analytic-Queries: Some analytic queries performed on the data after ETL.

- README.md: provides discussion on your process and decisions.

#### How to run the etl.py?

**Locally:**

This mode is not desired since it will have to load the data chunks from the S3 and then perform ETL on single machine. However, this is method is helpful when you are handling a part of the entire data.

Set the AWS access key id and secret key:

    export AWS_ACCESS_KEY_ID="access-key"
    export AWS_SECRET_ACCESS_KEY="secret-key"

Command:

    spark-submit --jars hadoop-aws-2.7.0.jar,aws-java-sdk-1.7.4.jar etl.py 
    
You can download the jars here:

!(https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-aws/2.7.0)

**Note:** Follow the local procedure if running in any cluster other than EMR cluster.

**On the EMR cluster**

Command:

    spark-submit etl.py
    
Connect to the master node and run the command. In this method you need no specific jars as they are already available. 

#### Analytic Queries

