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

## Architecture

```text
Source Systems
     |
     |-- Azure SQL DB
     |-- Flat Files
     |-- APIs
     |
     v
Landing Layer
     |
     v
Bronze Layer - Raw Parquet Files
     |
     v
Silver Layer - Cleaned Delta Tables
     |
     v
Gold Layer - Fact and Dimension Tables
     |
     v
Business Reporting / Analytics
