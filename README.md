# Data Engineering Nanodegree

## Data Modeling

## Project: Data Modeling with Postgres

## Table of Contents

- [Introduction](#intro)
- [Description](#describe)
- [Data](#data)
  - [Song Dataset](#song)
  - [Log Dataset](#log)
- [Schema for Song Play Analysis](#schema)
  - [Fact Table](#fact)
  - [Dimension Tables](#dim)
- [Files](#files)

***

<a id="intro"></a>

## Introduction

A startup called Sparkify wants to analyze the data they've been collecting on
songs and user activity on their new music streaming app. The analytics team is
particularly interested in understanding what songs users are listening to.
Currently, they don't have an easy way to query their data, which resides in a
**directory of JSON logs** on user activity on the app, as well as a **directory
with JSON metadata** on the songs in their app.

They need a data engineer to create a **Postgres** database with tables designed
to **optimize queries on song play analysis.** Our role is to create a **database
schema** and **ETL pipeline** for this analysis. Then we'll test our database and ETL
pipeline by running queries given by the analytics team from Sparkify and
compare my results with their expected results.

<a id="describe"></a>

## Description

In this project, We'll apply the concepts learned in data modeling with Postgres
and build an ETL pipeline using Python. We will define **fact and dimension tables**
for a star schema for a particular analytic focus, and write an ETL pipeline
that transfers data from files in two local directories into these tables in
Postgres using Python and SQL.

<a id="data"></a>

## Data

<a id="song"></a>

### Song Dataset

The first dataset is a subset of real data from the [Million Song
Dataset](https://labrosa.ee.columbia.edu/millionsong/). Each file is in JSON
format and contains metadata about a song and the artist of that song. The files
are partitioned by the first three letters of each song's track ID. For example,
here are filepaths to two files in this dataset.

<pre>
data/song_data/A/B/C/TRABCEI128F424C983.json
data/song_data/A/A/B/TRAABJL12903CDCF1A.json
</pre>

And below is an example of what a single song file, _TRAABJL12903CDCF1A.json_,
looks like.

<pre>
{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}
</pre>

<a id="log_data"></a>

### Log Dataset

The second dataset consists of log files in JSON format generated by this [event
simulator](https://github.com/Interana/eventsim) based on the songs in the
dataset above. These simulate app activity logs from a music streaming app based
on specified configurations.

The log files in the dataset we'll be working with are partitioned by year and
month. For example, here are filepaths to two files in this dataset.

<pre>
data/log_data/2018/11/2018-11-12-events.json
data/log_data/2018/11/2018-11-13-events.json
</pre>

And below is an example of what the data in a log file, _2018-11-12-events.json_,
looks like.

![log-data](./img/log-data.png)

If you would like to look at the json data within log_data files, you will need to create a Pandas df to read the data. Remember to first import json and pandas libraries.

```python
df = pd.read_json(filepath, lines=True)
```

For e.g., `df = pd.read_json('data/log_data/2018/11/2018-11-01-events.json', lines=True)` would read the data file _2018-11-01-events.json_

Here is a helpful [video](https://www.youtube.com/watch?v=hO2CayzZBoA) from **Udacity's Data Wrangling course** talking about the JSON files format.

<a id="schema"></a>

## Schema for Song Play Analysis

Using the song and log datasets, we'll create a star schema optimized for queries
on song play analysis. This includes the following tables.

<a id="fact"></a>

### Fact Table

1. **songplays** - records in log data associated with song plays, i.e., records with
  page `NextSong`
    - *songplay_id, start_time, user_id, level, song_id, artist_id, session_id,
      location, user_agent*
    
A preview of the table

<a id="dim"></a>

### Dimension Tables

2. **users** - users in the app
    - *user_id, first_name, last_name, gender, level*

3. **songs** - songs in music database
    - *song_id, title, artist_id, year, duration*

4. **artists** - artists in music database
    - *artist_id, name, location, latitude, longitude*

5. **time** - timestamps of records in **songplays** broken down into specific units
    - *start_time, hour, day, week, month, year, weekday*

<a id="files"></a>

## Files

<pre>
.
├── create_tables.py--------Drops and creates tables. Running this file resets
|                           the tables before each time we run ETL scripts.
├── etl.ipynb---------------Reads and processes a single file from song_data and
|                           log_data and loads the data into our tables.
├── etl.py------------------Reads and processes files from song_data and
|                           log_data and loads them into our tables. We'll fill
|                           based on ETL notebook.
├── sql_queries.py----------Contains all our SQL queries, and is imported into
|                           the last three files above.
└── test.ipynb--------------Displays the first few rows of each table to let us
                            check our database.
</pre>