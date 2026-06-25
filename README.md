# Azure Data Engineering Project – Healthcare Revenue Cycle Management

## Project Overview

This project demonstrates an end-to-end Azure Data Engineering solution for the Healthcare Revenue Cycle Management domain.

Revenue Cycle Management is the process hospitals use to manage financial activities from patient registration to provider payment. The goal of this project is to build scalable data pipelines that ingest, process, clean, and transform healthcare data into analytics-ready fact and dimension tables.

The project follows the Medallion Architecture:

Landing → Bronze → Silver → Gold

## Business Use Case

Hospitals need accurate reporting for Accounts Receivable and payment tracking. This project helps reporting teams calculate important KPIs such as:

- AR greater than 90 days
- Days in AR
- Claim status tracking
- Patient payment responsibility
- Provider and department-level financial reporting

## Tech Stack

- Azure Data Factory
- Azure Databricks
- Azure Data Lake Storage Gen2
- Azure SQL Database
- Delta Lake
- Azure Key Vault
- PySpark
- SQL
- Parquet
- Unity Catalog

## Data Sources

The project uses multiple healthcare data sources:

### EMR Data – Azure SQL Database

- Patients
- Providers
- Departments
- Transactions
- Encounters

### Claims Data

- Monthly flat files from insurance providers

### API Data

- NPI data
- ICD code data

### Reference Data

- CPT codes from flat files

## Architecture Diagram (Solution Architecture)

The pipeline follows the Medallion Architecture pattern with three data layers: Bronze, Silver, and Gold.

<p align="center">
  <img src="docs/architecture.png" alt="Azure Healthcare RCM Data Pipeline Architecture" width="100%">
</p>

## 🏗️ Medallion Architecture

The project follows the **Medallion Architecture** to ensure scalable, reliable, and high-quality data processing.

### 📥 Landing Layer

- Receives raw data from multiple external source systems.
- Stores incoming files without modification.
- Acts as the initial staging area before ingestion into the data lake.

---

### 🥉 Bronze Layer (Raw Data)

The Bronze layer stores data in its original format and serves as the **single source of truth**.

**Features**

- Raw data stored in **Parquet** format
- Immutable data storage
- Supports historical tracking
- Source-level data preservation

---

### 🥈 Silver Layer (Cleaned & Standardized)

The Silver layer transforms raw data into standardized, analytics-ready datasets using Delta Lake.

#### Key Implementations

- ✅ Common Data Model (CDM)
- ✅ Data Cleaning & Standardization
- ✅ Data Quality Validation
- ✅ Bad Record Quarantine
- ✅ Slowly Changing Dimension (SCD Type 2)
- ✅ Active/Inactive Record Handling
- ✅ Delta Table Implementation

---

### 🥇 Gold Layer (Business Ready)

The Gold layer contains curated business datasets optimized for reporting and analytics.

It includes:

- Fact Tables
- Dimension Tables
- Aggregated Business Metrics
- KPI Reporting Tables

These datasets are consumed by:

- Power BI
- Business Users
- Data Analysts
- Data Scientists

---

# 🚀 Key Features

- Metadata-driven ingestion pipeline
- Full Load & Incremental Load support
- Watermark-based Incremental Processing
- Audit Logging using Delta Tables
- Automatic Archive of Existing Files
- Parallel Azure Data Factory Pipelines
- Azure Key Vault Integration
- SCD Type 2 Implementation
- Data Quality Validation Framework
- Bad Record Quarantine Process
- Gold Layer Fact & Dimension Modeling
- Unity Catalog Integration
- Secure & Scalable Azure Architecture

---

# ⚙️ Azure Data Factory Pipeline

The Azure Data Factory (ADF) pipeline is **metadata-driven**, allowing dynamic ingestion of multiple tables without creating separate pipelines.

### Pipeline Workflow

1. Read metadata configuration file.
2. Lookup active tables.
3. Iterate through each table using **ForEach**.
4. Determine whether the table requires **Full Load** or **Incremental Load**.
5. Copy data from Azure SQL Database to Azure Data Lake Storage Gen2.
6. Archive previous files before loading new data.
7. Log pipeline execution details into the Audit table.
8. Trigger downstream Databricks processing.

---

## 📄 Metadata Configuration Example

```csv
database,datasource,tablename,loadtype,watermark,is_active,targetpath

trendytech-hospital-a,hos-a,dbo.encounters,Incremental,ModifiedDate,1,hosa
trendytech-hospital-a,hos-a,dbo.patients,Incremental,ModifiedDate,1,hosa
trendytech-hospital-a,hos-a,dbo.transactions,Incremental,ModifiedDate,1,hosa
trendytech-hospital-a,hos-a,dbo.providers,Full,,1,hosa
trendytech-hospital-a,hos-a,dbo.departments,Full,,1,hosa
trendytech-hospital-b,hos-b,dbo.encounters,Incremental,ModifiedDate,1,hosb
trendytech-hospital-b,hos-b,dbo.patients,Incremental,Updated_Date,1,hosb
trendytech-hospital-b,hos-b,dbo.transactions,Incremental,ModifiedDate,1,hosb
trendytech-hospital-b,hos-b,dbo.providers,Full,,1,hosb
trendytech-hospital-b,hos-b,dbo.departments,Full,,1,hosb
```

### Metadata Column Description

| Column | Description |
|---------|-------------|
| **database** | Source Azure SQL Database |
| **datasource** | Hospital identifier |
| **tablename** | Source table name |
| **loadtype** | Full or Incremental load |
| **watermark** | Incremental tracking column |
| **is_active** | Enable or disable table ingestion |
| **targetpath** | Destination folder in ADLS Gen2 |

---

## 📊 Pipeline Benefits

- Single metadata-driven pipeline
- Easily onboard new source tables
- Supports Full and Incremental loads
- Reduces pipeline maintenance
- Highly scalable architecture
- Enterprise-grade audit logging
- Dynamic configuration without code changes
