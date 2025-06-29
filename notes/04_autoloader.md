# ⚙️ What is Autoloader in Databricks?

**Autoloader** is a high-performance, **incremental data ingestion utility** built into Databricks that allows you to:

* Continuously ingest files from **cloud storage** (e.g., AWS S3, Azure Blob, GCS),
* Detect and process **new files only**, and
* Write them into **Delta tables** or other formats.

It’s part of **Structured Streaming**, but you can use it for batch-like or continuous pipelines.

---

## 🔍 Why Use Autoloader?

Traditional file ingestion via Spark needs full directory scans (slow, expensive). Autoloader solves this with:

* **Incremental discovery** (via file logs or cloud APIs),
* **Schema evolution**, and
* **Efficient checkpointing**.

---

# 🧠 How Autoloader Works Internally

### There are two **file discovery modes**:

---

## 1. **Directory Listing Mode (default)**

* Spark **periodically lists files** in the directory path (e.g., `s3://bucket/data/`).
* Keeps track of what files it has already processed (via checkpoint + metadata log).
* Works well for:

  * <10 million files
  * Simple cloud buckets

✅ Pros: No extra infra needed
❌ Cons: Can be slow and expensive for massive directories

---

## 2. **File Notification Mode** *(Recommended for Production)*

* Uses **cloud-native notifications**:

  * AWS SQS (for S3)
  * Azure Queue Storage
  * GCP Pub/Sub
* Every new file triggers a message → Autoloader consumes it.

✅ Pros: Near real-time, very efficient
❌ Cons: Needs cloud setup (SQS/PubSub queue, permissions)

---

## 🧾 Metadata Tracking

* Autoloader maintains a **metadata log** (like Delta log).
* It remembers every file ingested—so no need to deduplicate manually.
* This metadata is stored in a **checkpoint location** you specify.

---

## 📦 Autoloader File Formats Supported

* CSV
* JSON
* Parquet
* Avro
* Binary (e.g., PDFs, images)
* Delta

---

# 🛠️ Basic Usage

```python
from pyspark.sql.functions import *

df = (spark.readStream
    .format("cloudFiles")
    .option("cloudFiles.format", "json")  # or parquet, csv, etc.
    .option("cloudFiles.inferColumnTypes", "true")
    .load("s3://my-bucket/raw-data/")
)

df.writeStream \
  .format("delta") \
  .option("checkpointLocation", "/mnt/checkpoints/orders/") \
  .trigger(once=True) \
  .table("bronze_orders")
```

---

## 🧠 Key Options

| Option                            | Purpose                                      |
| --------------------------------- | -------------------------------------------- |
| `cloudFiles.format`               | File type (json, csv, parquet...)            |
| `cloudFiles.schemaLocation`       | Where to store inferred schema for evolution |
| `cloudFiles.inferColumnTypes`     | Infer proper data types from strings         |
| `cloudFiles.includeExistingFiles` | (default true) to read existing files        |
| `cloudFiles.useNotifications`     | Set to true for SQS/PubSub mode              |

---

# 🔁 Continuous vs Triggered Ingestion

* **Trigger Once**: Run once on all new files, like batch (but incremental)
* **Trigger AvailableNow**: Read new files *until directory is empty*, then stop
* **Continuous**: Read new files indefinitely like streaming

---

# 📈 Autoloader in Medallion Architecture

### Typically:

* Used to load raw files into the **Bronze layer**:

  * `s3://bucket/source-data/` → Autoloader → Delta Table `bronze_events`
* Can also land metadata (e.g., file\_name, \_ingest\_time) using:

  ```python
  .withColumn("_ingest_time", current_timestamp())
  ```

---

# 📂 Schema Evolution Support

* Autoloader supports **automatic schema evolution** in streaming:

  ```python
  .option("cloudFiles.schemaEvolutionMode", "addNewColumns")
  ```

* Store schema at:

  ```python
  .option("cloudFiles.schemaLocation", "/mnt/schema/bronze_orders")
  ```

---

# ✅ Benefits Summary

| Feature                       | Advantage                          |
| ----------------------------- | ---------------------------------- |
| Metadata logs                 | Incremental loading, no duplicates |
| File notifications            | Scalable real-time ingest          |
| Schema inference & evolution  | Less manual work                   |
| Streaming & batch flexibility | Adapt to use-case                  |
| Native Delta support          | Perfect for medallion pipelines    |

---

# 🚨 Limitations / Things to Know

1. ❌ Not supported for **Hive-partitioned directories**
2. 🧪 Autoloader tracks files via filename, not content hash
3. ⏳ Checkpoint location is **mandatory**
4. 🔒 Ensure IAM roles/permissions for notification mode (SQS, etc.)
5. 📁 Doesn’t delete raw files—only reads them

---

# 🧩 Use in dbt or Workflow Pipelines

* Usually placed in **notebooks or Python scripts**, run via **Databricks Jobs**.
* Not run *inside* dbt, but used before dbt models process Bronze → Silver → Gold.
* Can be coupled with `MERGE INTO` logic if needed for SCD handling.

---

## 🚀 Summary

| Feature       | Description                                       |
| ------------- | ------------------------------------------------- |
| Core Use      | Efficient, incremental file ingestion             |
| Ideal For     | Loading data into Bronze layer                    |
| Modes         | Directory Listing / Notification-based            |
| Cloud Support | S3 (SQS), Azure (Queue), GCS (Pub/Sub)            |
| Output        | Delta Table (usually Bronze)                      |
| Advantages    | Real-time, scalable, schema-aware, fault-tolerant |
