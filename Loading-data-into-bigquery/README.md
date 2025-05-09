# Google BigQuery NYC Taxi Data Demo

This repository contains a hands-on tutorial demonstrating core BigQuery features. Main objectives:

1. Creating datasets
2. Loading CSV data (via UI or CLI)
3. Writing analytical SQL queries 
4. Materializing query results into tables

## Prerequisites

- GCP Project (Skills-Boost lab project or billing enabled)
- IAM roles: BigQuery Admin or (BigQuery Data Owner + Job User)

## 1. Create Dataset

1. Go to BigQuery console
2. Click + CREATE DATASET
3. Dataset ID: `nyctaxi`
4. Location: `US` (multi-region)
5. Click Create

## 2. Load CSV Data

### Via UI:

1. In nyctaxi dataset click Create Table
2. Select Upload → choose nyc_tlc_yellow_trips_2018_subset_1.csv
3. Table name: `2018trips`
4. Check Auto-detect schema
5. Click Create table

## 3. Sample Queries

### Top 5 Highest Fares:

SELECT *
FROM nyctaxi.2018trips
ORDER BY fare_amount DESC
LIMIT 5;

### Save January Data to New Table:

CREATE TABLE nyctaxi.january_trips AS
SELECT * 
FROM nyctaxi.2018trips
WHERE EXTRACT(Month FROM pickup_datetime)=1;

### Longest Trip Distance in January:

SELECT *
FROM nyctaxi.january_trips
ORDER BY trip_distance DESC 
LIMIT 1;
