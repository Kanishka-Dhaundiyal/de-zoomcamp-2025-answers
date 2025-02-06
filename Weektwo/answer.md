Within the execution for Yellow Taxi data for the year 2020 and month 12: what is the uncompressed file size (i.e. the output file yellow_tripdata_2020-12.csv of the extract task)?
128.3 MB
134.5 MB
364.7 MB
692.6 MB
### Answer: `128.3 MB`

What is the rendered value of the variable file when the inputs taxi is set to green, year is set to 2020, and month is set to 04 during execution?
{{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv
green_tripdata_2020-04.csv
green_tripdata_04_2020.csv
green_tripdata_2020.csv
### Answer: `green_tripdata_2020-04.csv`

How many rows are there for the Yellow Taxi data for all CSV files in the year 2020?
13,537.299
24,648,499
18,324,219
29,430,127
### Answer: `24,648,499`
### Approach:
```
FROM `kestra-de-sandbox.zoomcamp.yellow_tripdata`
WHERE DATE(tpep_pickup_datetime) >= '2020-01-01'
AND DATE(tpep_dropoff_datetime) <= '2020-12-31';
```
How many rows are there for the Green Taxi data for all CSV files in the year 2020?
5,327,301
936,199
1,734,051
1,342,034
### Answer: `1,734,051`
### Approach:
```
SELECT count(*)
FROM `kestra-de-sandbox.zoomcamp.green_tripdata`
WHERE DATE(lpep_pickup_datetime) >= '2020-01-01'
AND DATE(lpep_dropoff_datetime) <= '2020-12-31'; 1,734,051
SELECT count(*) FROM `kestra-de-sandbox.zoomcamp.yellow_tripdata_2021_03`
```
How many rows are there for the Yellow Taxi data for the March 2021 CSV file?
1,428,092
706,911
1,925,152
2,561,031
### Answer: `1,925,152`
### Approach:
```
SELECT count(*)
FROM `kestra-de-sandbox.zoomcamp.yellow_tripdata`
WHERE DATE(tpep_pickup_datetime) >= '2021-03-01'
AND DATE(tpep_dropoff_datetime) <= '2021-03-31';
```
How would you configure the timezone to New York in a Schedule trigger?
Add a timezone property set to EST in the Schedule trigger configuration
Add a timezone property set to America/New_York in the Schedule trigger configuration
Add a timezone property set to UTC-5 in the Schedule trigger configuration
Add a location property set to New_York in the Schedule trigger configuration
### Answer: `Add a timezone property set to America/New_York in the Schedule trigger configuration`
