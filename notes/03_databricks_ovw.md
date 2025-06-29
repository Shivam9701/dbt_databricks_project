## 📘 Notes Collection: Databricks Data Engineering Concepts

---

### 🧱 0. High-Level Overview of Databricks

#### 📌 What is Databricks?

Databricks is a **cloud-based data platform** built on top of Apache Spark that provides an integrated workspace for:

* Big data processing
* Machine learning
* Data engineering
* Data science
* Business intelligence

It’s designed for collaborative work using **notebooks**, **jobs**, and **pipelines**—all running on a scalable infrastructure powered by **Delta Lake**.

#### 🌐 Supported Cloud Platforms:

* AWS (Amazon Web Services)
* Azure (Microsoft)
* GCP (Google Cloud Platform)

---

### 🔧 Core Components of Databricks

#### 1. **Workspace**

* Central UI for accessing notebooks, jobs, workflows, and files.
* Organizes your code and data into folders.
* Supports real-time collaboration (like Google Docs for notebooks).

#### 2. **Notebooks**

* Interactive development environment (IDE) supporting:

  * Python
  * SQL
  * Scala
  * R
  * Markdown
* Used for exploring data, running transformations, and building ML models.

#### 3. **Clusters**

* Compute engines running Spark jobs.
* Can be:

  * **Interactive**: For development/debugging
  * **Job**: For production tasks
* Autoscaling and termination features available.

#### 4. **Jobs**

* Automated and scheduled workflows.
* Can execute:

  * Notebooks
  * JARs
  * Python scripts
  * SQL files
* Monitorable with retries, notifications, and dependency chaining.

#### 5. **Delta Lake**

* An **open-source storage layer** providing:

  * ACID transactions
  * Time travel
  * Scalable metadata handling
  * Schema enforcement/evolution
* Works on top of Parquet files
* Core to Medallion Architecture (Bronze → Silver → Gold)

#### 6. **Unity Catalog**

* Centralized metadata governance
* Provides:

  * Table and column-level access control
  * Data lineage tracking
  * Schema enforcement
  * Multi-workspace data sharing

#### 7. **Databricks SQL (DB SQL)**

* SQL-native experience for analysts
* Create and query dashboards from Delta Tables
* Similar to BI tools like Redash or Tableau but native to Databricks

#### 8. **MLflow (Machine Learning Flow)**

* Manages ML lifecycle:

  * Experiment tracking
  * Model versioning
  * Deployment
  * Reproducibility

#### 9. **Autoloader**

* Tool for **incremental ingestion** of files from cloud storage
* Useful for building real-time data pipelines
* Tracks metadata and supports schema evolution (see section 3)
