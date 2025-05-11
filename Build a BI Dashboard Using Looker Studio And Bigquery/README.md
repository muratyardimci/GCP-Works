# 🌳 City Tree Services Dashboard  
BigQuery & Looker Studio Operational Analytics
This repository contains an example BI workflow that ingests daily tree-service logs into **BigQuery** and visualizes key metrics with **Looker Studio**.

## Overview
Dashboards should query *aggregated* data rather than raw logs to cut costs and improve performance.  
In this project:

* Source data: `bigquery-public-data.san_francisco_trees.street_trees`
* Aggregated table: `Reports.Trees` (month × species × caretaker × address × site)
* A scheduled query appends new rows hourly.
* Looker Studio consumes the aggregated table for visualization.

# 1. Import Public Data
### Open the BigQuery console.
### + ADD DATA → Public Datasets → search “Street Trees” and add it to your project.

![image alt](https://github.com/istanbuldatascienceacademy/DE-JAN25/blob/main/Proje%20Teslim/muratyardimci/Build%20a%20BI%20Dashboard%20Using%20Looker%20Studio%20and%20BigQuery/1-data-loaded.png?raw=true)

# 2. Creating Dataset

![image alt](https://github.com/istanbuldatascienceacademy/DE-JAN25/blob/main/Proje%20Teslim/muratyardimci/Build%20a%20BI%20Dashboard%20Using%20Looker%20Studio%20and%20BigQuery/2-dataset-created.png?raw=true)

# 3. Initial Aggregation Query
```bash
-- Creates Reports.Trees
SELECT
  TIMESTAMP_TRUNC(plant_date, MONTH) AS plant_month,
  COUNT(tree_id)                      AS total_trees,
  species,
  care_taker,
  address,
  site_info
FROM `bigquery-public-data.san_francisco_trees.street_trees`
WHERE address IS NOT NULL
  AND plant_date >= TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 365 DAY)
  AND plant_date <  TIMESTAMP_TRUNC(CURRENT_TIMESTAMP(), DAY)
GROUP BY plant_month, species, care_taker, address, site_info
```
![image alt](https://github.com/istanbuldatascienceacademy/DE-JAN25/blob/main/Proje%20Teslim/muratyardimci/Build%20a%20BI%20Dashboard%20Using%20Looker%20Studio%20and%20BigQuery/3-query-the-data.png?raw=true)

![image alt](https://github.com/istanbuldatascienceacademy/DE-JAN25/blob/main/Proje%20Teslim/muratyardimci/Build%20a%20BI%20Dashboard%20Using%20Looker%20Studio%20and%20BigQuery/4-creating-table-via-querying.png?raw=true)

# 4. Daily Incremental Refresh

```bash
-- Appends records from the past day
SELECT
  TIMESTAMP_TRUNC(plant_date, MONTH) AS plant_month,
  COUNT(tree_id)                      AS total_trees,
  species,
  care_taker,
  address,
  site_info
FROM `bigquery-public-data.san_francisco_trees.street_trees`
WHERE address IS NOT NULL
  AND plant_date >= TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 1 DAY)
  AND plant_date <  TIMESTAMP_TRUNC(CURRENT_TIMESTAMP(), DAY)
GROUP BY plant_month, species, care_taker, address, site_info
```

### Schedule: Repeat every 1 hour
### Destination write: Append to table

![image alt](https://github.com/istanbuldatascienceacademy/DE-JAN25/blob/main/Proje%20Teslim/muratyardimci/Build%20a%20BI%20Dashboard%20Using%20Looker%20Studio%20and%20BigQuery/6-SCHECULED-QUERY.png?raw=true)

# 5. Build the Looker Studio Report

1. Go to Looker Studio → Create → Report
2. Add a data source → BigQuery → select Project ▸ Reports ▸ Trees.
3. Add charts, e.g.:
 - Stacked column: month × species × caretaker
 - Scorecard: total trees (past year)
 - Pie chart: species distribution
 - Table + bar: site info

![image alt](https://github.com/istanbuldatascienceacademy/DE-JAN25/blob/main/Proje%20Teslim/muratyardimci/Build%20a%20BI%20Dashboard%20Using%20Looker%20Studio%20and%20BigQuery/7-charts-created-on-looker.png?raw=true)
