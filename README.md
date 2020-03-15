# Introduction
The Sparkify analytics team wants to understand what songs users are listening to.  Unfortunately there is no easy way to do so since this information is stored as JSON log files on the file system.

## Plan
Utilize a Relational Database Management System (RDBMS).  Design tables optimized to query on song play analysis. Populate the tables from JSON log files using an ETL (Extract Transform Load) pipeline written in Python.

## Analysis
The JSON datafiles are in two separate directories: song_data and log_data.  The song_data directory contains information about a song.  The log_data directory contains activity log information separated by date.

#### song_data
The files are partitioned by the first three letters of each song's track ID. For example, here are filepaths to two files in this dataset.

    song_data/A/B/C/TRABCEI128F424C983.json
    song_data/A/A/B/TRAABJL12903CDCF1A.json

Below is an example of what a single song file, TRAABJL12903CDCF1A.json, looks like.

    {"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}

#### log_data
The log files are partitioned by year and month.

    log_data/2018/11/2018-11-12-events.json
    log_data/2018/11/2018-11-13-events.json

Below is an example of a few lines of what the data in a log file, 2018-11-12-events.json, looks like.

    {"artist":null,"auth":"Logged In","firstName":"Celeste","gender":"F","itemInSession":0,"lastName":"Williams","length":null,"level":"free","location":"Klamath Falls, OR","method":"GET","page":"Home","registration":1541077528796.0,"sessionId":438,"song":null,"status":200,"ts":1541990217796,"userAgent":"\"Mozilla\/5.0 (Windows NT 6.1; WOW64) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/37.0.2062.103 Safari\/537.36\"","userId":"53"}
    {"artist":"Pavement","auth":"Logged In","firstName":"Sylvie","gender":"F","itemInSession":0,"lastName":"Cruz","length":99.16036,"level":"free","location":"Washington-Arlington-Alexandria, DC-VA-MD-WV","method":"PUT","page":"NextSong","registration":1540266185796.0,"sessionId":345,"song":"Mercy:The Laundromat","status":200,"ts":1541990258796,"userAgent":"\"Mozilla\/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit\/537.77.4 (KHTML, like Gecko) Version\/7.0.5 Safari\/537.77.4\"","userId":"10"}
    {"artist":"Barry Tuckwell\/Academy of St Martin-in-the-Fields\/Sir Neville Marriner","auth":"Logged In","firstName":"Celeste","gender":"F","itemInSession":1,"lastName":"Williams","length":277.15873,"level":"free","location":"Klamath Falls, OR","method":"PUT","page":"NextSong","registration":1541077528796.0,"sessionId":438,"song":"Horn Concerto No. 4 in E flat K495: II. Romance (Andante cantabile)","status":200,"ts":1541990264796,"userAgent":"\"Mozilla\/5.0 (Windows NT 6.1; WOW64) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/37.0.2062.103 Safari\/537.36\"","userId":"53"}

***

#### Table definitions and source for column data

*Table:  songplays - records in log data associated with song plays, i.e. records with page `NextSong`*

|Column                          |JSON Source / Attribute|
|-------------------------------|-----------------------------
| songplay_id                   | log_data line number or could be randomly generated, e.g. GUID
|start_time | log_data / ts
|user_id | log_data / userId
| level | log_data / level
| song_id | song_data / song_id
| artist_id | song_data / artist_id
| session_id | log_data / sessionId
| location | log_data / location
| user_agent | log_data / userAgent

*Table:  users - listeners in the application*

|Column                         |JSON Source / Attribute|
|-------------------------------|-----------------------------
| user_id                       | log_data / userId
|first_name | log_data / firstName
|last_name | log_data / lastName
|gender | log_data / gender
| level | log_data / level

*Table:  songs - songs in music database*

|Column                         |JSON Source / Attribute|
|-------------------------------|-----------------------------
| song_id                       | song_data / song_id
| title | song_data / title
| artist_id | song_data / artist_id
| year | song_data / year
| duration | song_data / duration

*Table:  artists - artists in music database*

|Column                         |JSON Source / Attribute                      |
|-------------------------------|-----------------------------
| artist_id                     | song_data / artist_id
|name | song_data / artist_name
|location | song_data / artist_location
|latitude | song_data / artist_latitude
|longitude | song_data / artist_longitude

*Table:  time - timestamps of records in songplays broken down into specific units*

|Column                         | JSON Source / Attribute|
|-------------------------------|-----------------------------
| start_time                    |  log_data / ts
|hour | log_data / ts (hour decoded)
|day | log_data / ts (day decoded) 
|week | log_data / ts (week decoded)
|month | log_data / ts (month decoded)
|year | log_data / ts (year decoded)
|weekday | log_data / ts (weekday decoded)

### Project Files
* create_tables.py
* etl.ipynb
* etl.py
* sql_queries.py
* test.ipynb

### Completing the project
1.  Define tables.  Update the `sql_queries.py` file to define the CREATE TABLE and DROP TABLE statements.  Verify the tables were created by running create_tables.py.
    * `python create_tables.py`
2.  Verify the table structure by running `test.ipynb`.  Note, this file updated to include table counts.  Also, a songplays check (last SQL statement) was added based upon the initial requirements.
3. Extract data from the JSON files and populate the RDBMS tables by updating `etl.ipynb`.  Throughout the process, run `test.ipynb` to verify the results.  Rerun `create_tables.py` to reinitialize the database tables.
4. After verifying the process in the `etl.ipynb`, update `etl.py` to process the entire dataset.
5. Test etl.py.
    *  `python create_tables.py` to reinitialize the database tables
    *  `python etl.py` to populate the tables
    *  Run `test.ipynb` to verify the results.

***
###### Udacity's Data Engineering Project:  Data Modeling with Postgres