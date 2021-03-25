# Data Modeling with PostgreSQL

## Table of contents
* [Introduction](#introduction)
* [Setup](#setup)

## Introduction 
A startup company called Sparkify that provides music streaming service. The data about songs and users' activities are stored in JSON format. The analytic team would like to ask the data engineer to create a Postgres database with tables designed to optimize queries on song play analysis. 

This task is to model the data using STAR schema and then build an ETL pipeline to populate the database optimized for analysis. 

The STAR schema is compiosed of one or more fact tables that could be referenced to other dimension tables according to the needs. 

## Setup
### How to run 
* Set up local PostgreSQL instance, more detailed instructions in [PostgreSQL](https://www.postgresql.org/docs/9.1/runtime.html).
* Set up Python environment in version 3.7 or newer, run create_tables.py to set up databases, including reseting and creating tables. Do not forget to run this file before each time you run the ETL scripts.
* Run etl.py to conduct the ETL process. 
* Launch test.ipynb in Jupter Notebook to validate the ETL process.

### Database design
To learn how/why this schema design was made in this way, you should read our docs below: 

### Song Plays table

- Name: `songplays`
- Type:`Fact table`

| Column | Type |
| ------ | ---- |
| `songplay_id` | `SERIAL PRIMARY KEY` |
| `start_time` | `TIMESTAMP` |
| `user_id` | `INTEGER` |
| `level` | `TEXT` |
| `song_id` | `TEXT` |
| `artist_id` | `TEXT` |
| `session_id` | `INTEGER` |
| `location` | `TEXT` |
| `user_agent` | `TEXT` |

### Users table

- Name: `users`
- Type:`Dimension table`

| Column | Type |
| ------ | ---- |
| `user_id` | `INTEGER PRIMARY KEY` |
| `first_name` | `TEXT` |
| `last_name` | `TEXT` |
| `gender` | `TEXT` |
| `level` | `TEXT` |


### Songs table

- Name: `songs`
- Type:`Dimension table`

| Column | Type |
| ------ | ---- |
| `song_id` | `TEXT PRIMARY KEY` |
| `title` | `TEXT` |
| `artist_id` | `TEXT` |
| `year` | `INTEGER` |
| `duration` | `DECIMAL` |


### Artists table

- Name: `artists`
- Type:`Dimension table`

| Column | Type |
| ------ | ---- |
| `artist_id` | `TEXT PRIMARY KEY` |
| `name` | `TEXT` |
| `location` | `TEXT` |
| `latitude` | `NUMERIC` |
| `longitude` | `NUMERIC` |

### Time table

- Name: `time`
- Type:`Dimension table`

| Column | Type |
| ------ | ---- |
| `start_time` | `TIMESTAMP PRIMARY KEY` |
| `hour` | `INTEGER` |
| `day` | `INTEGER` |
| `week` | `INTEGER` |
| `month` | `INTEGER` |
| `year` | `INTEGER` |
| `weekday`| `INTEGER` |

### Data cleaning
Data is composed of two JSON format files: Song data from the [Million Song Dataset](https://labrosa.ee.columbia.edu/millionsong/) and songplay data from user logs. Panda is used to read the JSON files in dataframes, then process and uploaded them iinto the database using psycopg2. 

Steps for cleaning data:
* Rows from users, artists tables are excluded when user_id or artist_id are missing.
* Extract Data for Time Table is filterd by the `NextSong` Page.
* Convert the ts timestamp column to datetime.

### Files in the Project
| Files | Description |
| ------ | ---- |
| `create_tables.py` | `Drop and create tables, run each time before ETL process.` |
| `sql_queries.py` | `contains sql queries and be imported into other files.`|
| `etl.py` | `Extract and process all data and loads them into tables.` |
| `etl.ipyb` | `Extract and process one file data and loads them into tables.` |
| `test.ipynb` | `Used to test the databases whether ETL works.` |