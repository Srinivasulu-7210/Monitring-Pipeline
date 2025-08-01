# Monitring-Pipeline

Project Name: Real-Time Price Monitoring Pipeline with Snowflake, EMR, and Redshift

🌐 Use Case:
Scrape product prices from websites in real time, stream data via AWS, clean/process it on EMR, store in S3, and push it to both Redshift and Snowflake for BI dashboards and analytics.

🧱 Revised Architecture:


```
[Lambda Web Scraper + EventBridge]
              ↓
     [AWS Kinesis Data Streams]
              ↓
           [AWS Lambda]
              ↓
          [S3 Raw Zone]
              │
              └──▶ [AWS Glue Crawler]
              ↓
[EMR Spark Job for Cleaning/Transforming]
              ↓
        [S3 Processed Zone]
             │       │
             ▼       ▼
        [Redshift]  [Snowflake]
             ▼           ▼
  [Amazon QuickSight]   [Snowflake Snowsight / Tableau / Power BI]
```







## 🔁 End-to-End Data Pipeline Steps

---

### 🔹 Step 1: Web Scraper (Lambda + EventBridge)
- Use a Python AWS Lambda function with `requests` and `BeautifulSoup`.
- Schedule it to run every 5 minutes using Amazon EventBridge (CloudWatch Events).
- Send scraped product price data to **Amazon Kinesis Data Streams**.

---

### 🔹 Step 2: Real-Time Ingestion (Kinesis)
- **Kinesis** captures real-time product price events.
- Triggers another **Lambda** function to store incoming events into **Amazon S3 (Raw Zone)**.

---

### 🔹 Step 3: Raw Data Cataloging (AWS Glue Crawler)
- **AWS Glue Crawler** scans the S3 Raw Zone and catalogs the data in the AWS Glue Data Catalog.
- *(Optional)*: Query the raw data directly using **Amazon Athena**.

---

### 🔹 Step 4: Data Processing on Amazon EMR
- Use **Apache Spark** (PySpark) running on **Amazon EMR**.
- Read from the **S3 Raw Zone**.
- Clean and transform data:
  - Cast `price` to float
  - Deduplicate records
- Write the cleaned data to the **S3 Processed Zone** in formats like **Parquet**.

---

### 🔹 Step 5: Load into Amazon Redshift
- Use the **Redshift COPY command** or **AWS Glue Job** to load data.
- You can:
  - Load into an internal Redshift table
  - Or use **Redshift Spectrum** with external tables pointing to S3

---

### 🔹 Step 6: Load into Snowflake
- Use **Snowpipe** for near real-time loading from S3.
- Alternatively, use **Snowflake Tasks** to run scheduled batch jobs.
- Process involves:
  - Reading from S3 Processed Zone
  - Using `STAGE` + `FILE FORMAT` + `COPY INTO` commands

---

### 🔹 Step 7: Visualization
- Use **Amazon QuickSight** for dashboards built on Redshift.
- For Snowflake, use:
  - **Snowflake Snowsight**
  - Or BI tools like **Tableau** or **Power BI**

---






## 🧠 Tools & Skills You’ll Demonstrate

| **Area**               | **Tool/Service**                                      |
|------------------------|-------------------------------------------------------|
| Web Scraping           | Python, BeautifulSoup                                 |
| Real-Time Ingestion    | AWS Kinesis, Lambda                                   |
| Data Lake              | S3, Glue Crawler, Athena                              |
| Data Processing        | Amazon EMR (PySpark)                                  |
| Data Warehouse         | Redshift, Snowflake                                   |
| BI & Dashboards        | QuickSight, Snowflake Snowsight                       |
| Automation             | EventBridge, Snowpipe, Lambda Triggers                |


