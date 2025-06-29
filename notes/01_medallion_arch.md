### 📦 1. Medallion Architecture

#### 🧠 What is it?

A data architecture pattern to incrementally improve data quality and structure through three logical layers:

* **Bronze**: Raw ingestion layer
* **Silver**: Cleansed, enriched layer
* **Gold**: Business-level curated layer

#### 💡 Purpose

* Ensure data **quality, reproducibility, scalability**
* Optimize for **BI consumption and analytics**
* Promote **modular pipeline design**

#### 🥉 Bronze Layer

* Ingest raw data as-is from source (JSON, CSV, logs)
* Store full history
* Metadata columns (filename, ingestion timestamp)
* Used for auditing and traceability

#### 🥈 Silver Layer

* Cleanse and enrich data
* De-duplicate, typecast, standardize
* May apply Slowly Changing Dimensions (SCDs)
* Normalize entities (e.g., customer, product)

#### 🥇 Gold Layer

* Dimensional models (star/snowflake schema)
* Aggregations, KPIs, business reports
* Used by BI tools (PowerBI, Tableau), exec dashboards

---
