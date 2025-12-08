# 🚀 Azure-Based ETL Pipeline: ADF and Databricks

---

## 💡 Project Goal

This repository implements a **modern, scalable, and resilient End-to-End Data Engineering Pipeline** using core services within the **Microsoft Azure** cloud ecosystem. The goal is to ingest raw data, transform it using powerful Spark processing, and deliver analytics-ready data layers for business intelligence (BI) and machine learning (ML) consumption.

This project focuses on production-style patterns, including automated **orchestration**, implementation of the **Medallion Architecture**, and **scalable PySpark transformations**.

---

## ☁️ Architectural Overview and Components

The pipeline is built on an ELT (Extract, Load, Transform) framework, leveraging the strengths of each Azure service:

| Component | Responsibility | Concept |
| :--- | :--- | :--- |
| **Azure Data Factory (ADF)** | **Orchestration & Data Movement** | The serverless tool used to sequence and schedule the entire workflow (pipeline), controlling data ingestion and triggering transformation jobs. |
| **Azure Databricks** | **Scalable Data Transformation** | A fast, powerful, Apache Spark-based analytics platform used to run the complex transformation logic written in PySpark. |
| **Azure Data Lake Storage (ADLS)** | **Data Lakehouse Storage** | Provides tiered, scalable, and cost-effective storage for all data layers (Bronze, Silver, Gold). |
| **Delta Lake** | **Reliability & ACID Properties** | The open-source storage layer used within Databricks/ADLS to bring reliability (ACID transactions, versioning) to the data lake. |

## Overall Architecture Diagram
![ArchitectureDiag](https://raw.githubusercontent.com/dosibhatlanirmalaaiswarya-bit/Azure-Based-ETL-Pipeline-Using-ADF-Databricks/main/Azure%20Architecture%20Diagram.jpg)

### Medallion Architecture Explained

Data is segregated into three distinct layers within the **Azure Data Lake** to ensure quality, traceability, and governance:

| Layer | State | Purpose in Pipeline |
| :--- | :--- | :--- |
| **🥉 Bronze** | **Raw Data** | Stores data *as-is* immediately after ingestion from the source. Provides an immutable historical record. |
| **🥈 Silver** | **Cleaned & Conformed Data** | Data is cleansed, standardized, de-duplicated, and integrated across sources. Ready for detailed exploration. |
| **🥇 Gold** | **Curated & Aggregated Data** | Highly refined, aggregated, and modeled (e.g., Star Schema) data optimized for fast BI reporting (Power BI) and ML training. |

---

## ⚙️ Detailed Pipeline Steps

The entire workflow is orchestrated by a single **Azure Data Factory Pipeline**, executing tasks sequentially:

### Step 1: Data Ingestion (ADF to Bronze)

* **Action:** ADF's Copy Activity extracts raw data (e.g., from source JSON files like `customers.json` and `products.json`) and loads it directly into a designated **Bronze Layer** folder in **ADLS**.
* **Goal:** Establish an immutable record of the raw source data.

### Step 2: Transformation and Cleansing (Databricks - Silver Layer)

* **Action:** ADF triggers an **Azure Databricks Notebook Activity**, executing the `ETL_Notebook.ipynb`.
* **Process (PySpark):** The notebook performs the ETL logic:
    1.  Reads raw data from the Bronze Layer using Spark.
    2.  Applies data cleansing (e.g., handling nulls, type casting).
    3.  Applies standardization and deduplication.
    4.  Writes the cleaned data back to the **Silver Layer** (typically using Delta Lake format for reliability).

### Step 3: Business Logic and Aggregation (Databricks - Gold Layer)

* **Action:** The Databricks notebook continues execution, focusing on business requirements.
* **Process (PySpark):**
    1.  Reads the cleaned data from the Silver Layer.
    2.  Performs complex **joins** (e.g., combining customer and product data).
    3.  Applies business logic and **aggregations** (e.g., calculating total sales per product category or customer lifetime value).
    4.  Writes the final, modeled data to the **Gold Layer** (Delta Lake format), creating the reporting tables.

### Step 4: Data Consumption

* **Result:** The Gold Layer tables are now ready to be consumed. Downstream applications, such as **Power BI**, connect directly to the curated tables to generate real-time dashboards and reports.

---

## 📂 Repository Contents

The key files in this repository demonstrate the components necessary for the pipeline:

| File Name | Description | Role in Pipeline |
| :--- | :--- | :--- |
| `ETL_Notebook.ipynb` | The core transformation logic, written in **PySpark** (Python + Spark API). | Executes Steps 2 & 3 (Silver and Gold transformations). |
| `Azure Architecture Diagram.jpg` | Visual representation of the data flow and cloud services integration. | Documentation/Reference |
| `customers.json` | Sample raw JSON file representing source Customer data. | Input Source Data |
| `products.json` | Sample raw JSON file representing source Product data. | Input Source Data |
| `README.md` | This documentation file. | Documentation |

---

## ✅ Data Engineering Features

* **End-to-End ELT/ETL:** Full pipeline demonstrated from raw ingestion to reporting-ready data.
* **Scalability:** Leverages Spark via Databricks for massive parallel processing of data transformation.
* **Orchestration:** Uses ADF to manage dependencies, scheduling, and environment parameterization.
* **Data Quality:** Enforcement of data quality standards via the Medallion Architecture.

---

## 🤝 Contribution & Contact

Feel free to fork this repository and explore the implementation details of a modern cloud-based data pipeline.

* **Author:** dosibhatlanirmalaaiswarya-bit
* **GitHub:** [dosibhatlanirmalaaiswarya-bit](https://github.com/dosibhatlanirmalaaiswarya-bit)
