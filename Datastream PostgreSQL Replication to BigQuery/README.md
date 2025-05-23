This README provides a step-by-step guide for setting up a PostgreSQL instance on Google Cloud SQL and using Data Stream to integrate with BigQuery.

# Steps
## 1. Create a Cloud SQL Instance
- Navigate to Cloud SQL in the Google Cloud Console.
- Click on Create Instance and select PostgreSQL.
  - Instance ID: postgres-db
  - Region: us-central1
  - Database Version: PostgreSQL 14

![image alt](https://github.com/istanbuldatascienceacademy/DE-JAN25/blob/4f73b72baacad736632808feea757a5a08998005/Proje%20Teslim/muratyardimci/Datastream-PostgreSQL-Replication-to-BigQuery/1-psql-instance-created.png)

## 2. Set Up Connection Profiles
- Navigate to Connection Profiles in the Data Stream service.
- Create Profile for both BigQuery and PostgreSQL.

![image alt](https://github.com/istanbuldatascienceacademy/DE-JAN25/blob/4f73b72baacad736632808feea757a5a08998005/Proje%20Teslim/muratyardimci/Datastream-PostgreSQL-Replication-to-BigQuery/2-connection-profiles-created.png)

## 3. Configure Data Stream
- Stream ID: test-stream
- Source profile: postgres-cp
- Destination profile: bigquery-cp

![image alt](https://github.com/istanbuldatascienceacademy/DE-JAN25/blob/4f73b72baacad736632808feea757a5a08998005/Proje%20Teslim/muratyardimci/Datastream-PostgreSQL-Replication-to-BigQuery/3-datastream-created.png)

## 4. Querying Data in BigQuery
![image alt](https://github.com/istanbuldatascienceacademy/DE-JAN25/blob/4f73b72baacad736632808feea757a5a08998005/Proje%20Teslim/muratyardimci/Datastream-PostgreSQL-Replication-to-BigQuery/4-View-the-data-in-BigQuery.png)

## 5. Querying Chaged Data in BigQuery
![image alt](https://github.com/istanbuldatascienceacademy/DE-JAN25/blob/4f73b72baacad736632808feea757a5a08998005/Proje%20Teslim/muratyardimci/Datastream-PostgreSQL-Replication-to-BigQuery/5-updated-data-and-see-the-change.png)

