# End-to-End AWS Data Pipeline Project

## Description

This project demonstrates how to **build a complete end-to-end data pipeline on AWS** using S3, Glue ETL jobs, triggers, Glue Data Catalog, and Athena queries. The pipeline extracts raw CSV data, transforms it using ETL jobs, stores metadata in the Glue Data Catalog, and enables queryable access via Athena.  

The project simulates a real-world workflow for automated data processing and analytics in a serverless cloud environment.

---

## Key AWS Services Used

- **Amazon S3** – Stores raw and transformed data, as well as Athena scripts  
- **AWS Glue** – Performs ETL (Extract, Transform, Load) operations  
- **AWS Glue Data Catalog** – Stores metadata for transformed datasets  
- **Amazon Athena** – Query transformed data using SQL  
- **IAM** – Provides secure permissions for S3, Glue, and Athena  

---

## Architecture Overview

1. **Data Storage** – CSV files are uploaded to S3 in the `raw-data` folder.  
2. **ETL Job** – AWS Glue job reads raw data, transforms it, and writes to the `transformed-data` folder in Parquet format.  
3. **Automation** – Glue trigger schedules the ETL job to run hourly.  
4. **Metadata Management** – Glue Crawler extracts schema and metadata into `onlineDatabase`.  
5. **Querying** – Athena queries the cataloged data to verify results.  

---

## Step-by-Step Implementation

### 1. S3 Setup

1. Create an S3 bucket named `online-data-bucket`.  
2. Create three folders:  
   - `raw-data` → for CSV input files  
   - `transformed-data` → for ETL output files (Parquet)  
   - `athena-online-scripts` → for Athena query scripts  
3. Upload CSV datasets to the `raw-data` folder.  

---

### 2. IAM Role Creation

1. Create an IAM role named `onlinedata-role-v1`.  
2. Attach the following policies:  
   - `AmazonS3FullAccess`  
   - `AWSGlueConsoleFullAccess`  
   - `AmazonAthenaFullAccess`  
3. This role allows Glue ETL jobs, triggers, and crawlers to access S3 and Athena.  

---

### 3. AWS Glue ETL Job

1. Open **AWS Glue** → Jobs → Add job → Visual ETL.  
2. Configure the job:  
   - Name: `onlineTransformationJob`  
   - IAM role: `onlinedata-role-v1`  
   - Source: `Amazon S3` → select CSV files in `raw-data` folder  
3. Apply transformations:  
   - Drop unnecessary fields/columns  
   - Apply SQL query transformations as needed  
4. Configure target:  
   - Destination: S3 `transformed-data` folder  
   - Format: Parquet  
5. Save and run the job → verify Parquet files are in the target folder  

---

### 4. Glue Trigger

1. Open **AWS Glue → Triggers → Add trigger**  
2. Configure:  
   - Name: `onlineJobTrigger`  
   - Schedule: Every hour, starting at the first minute  
   - Target: `onlineTransformationJob`  
3. Create and activate the trigger  

---

### 5. Glue Crawler & Data Catalog

1. Create a Glue Crawler:  
   - Name: `onlineCrawler`  
   - Data source: S3 `transformed-data` folder  
   - IAM role: `onlinedata-role-v1`  
   - Database: `onlineDatabase`  
   - Frequency: On demand  
2. Run the crawler → metadata for transformed data is extracted  

---

### 6. Querying in Athena

1. Open **Amazon Athena** → select `onlineDatabase`  
2. Run SQL queries on cataloged data to preview and validate results  

---

## Environment and Tools Used

- **AWS Console Management** (latest version)  
- **Text Editor: VS Code** (for editing queries or scripts)  

---

## Full Demo & Screenshots

For a complete walkthrough, including **full-resolution screenshots and screen recordings**, visit my Google Drive folder:  
[View Project Media](https://drive.google.com/drive/folders/1NqtespQ9rwsAiQKW7FR_KWinPhTFjeMx?usp=sharing)
