# Real-time Twitter Data Pipeline with Cloud Data Fusion

This repository demonstrates building a real-time data pipeline using Google Cloud Data Fusion to process Twitter data from Pub/Sub and load it into BigQuery.

## Architecture

PubSub → Data Fusion (Wrangler) → BigQuery

## Prerequisites

- Google Cloud Project with billing enabled
- Required IAM Roles:
  - Editor role for Compute Engine default service account
  - Cloud Data Fusion API Service Agent role
  - Dataproc Administrator role
  - Service Account User permission

## Setup Steps

### 1. Create Cloud Storage Bucket
```bash
# Create bucket with same name as project ID
export BUCKET=$GOOGLE_CLOUD_PROJECT
gsutil mb gs://$BUCKET

# Copy sample tweets file
gsutil cp gs://cloud-training/OCBL013/pubnub_tweets_2019-06-09-05-50_part-r-00000 gs://$BUCKET

```

### 2. Setup Pub/Sub Infrastructure

- Create topic named cdf_lab_topic
- Create pull subscription named cdf_lab_subscription

### 3. Configure Data Fusion Pipeline

- Create new realtime pipeline
- Add PubSub source:
  - Reference Name: Twitter_Input_Stream
  - Subscription: cdf_lab_subscription
- Add Wrangler transformation:

``` bash
# Wrangler
keep :message
set-charset :message 'utf-8'
rename :message :body
parse-as-json :body 1
parse-as-json :body_payload 1
columns-replace s/^body_payload_//g
(and more transformations)

```

### 4. Deploy and Run Pipeline

- Deploy pipeline
- Start pipeline execution
- Monitor metrics in UI

  ### 5. Send Messages to Pub/Sub

- Use Dataflow template "Text Files on Cloud Storage to Pub/Sub"
- Configure job:
  - Input: gs:///pubnub_tweets file
  - Output Topic: projects//topics/cdf_lab_topic
  - Temp Location: gs:///tmp/
 
## Key Features 

- Verify IAM permissions
- Check Pub/Sub subscription exists
- Validate Wrangler transformations
- Monitor pipeline metrics
