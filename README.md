# Customer 360 Data Platform

## 📌 Project Overview

The **Customer 360 Data Platform** is an end-to-end data engineering
project that brings customer data from multiple sources into one
centralized platform.

The goal of this project is to create a **360-degree view of customers**
by combining customer information, orders, products, stores, feedback,
and customer activity.

The project uses **Azure Data Factory for data ingestion**, **Azure Data
Lake Storage Gen2 for storage**, **Azure Databricks and PySpark for data
processing**, **Delta Lake for reliable data storage**, and **Power BI
for analytics and visualization**.

------------------------------------------------------------------------

## 🏗️ Architecture

![Customer 360 Architecture](architecture/Customer 360 Data Flow Architecture (1).png)
``` text
                    ┌─────────────────────┐
                    │   Data Sources      │
                    ├─────────────────────┤
                    │ Azure SQL Database  │
                    │ REST API / JSON     │
                    │ GitHub CSV Files    │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Azure Data Factory  │
                    │       (ADF)         │
                    ├─────────────────────┤
                    │ Copy Activity       │
                    │ Lookup Activity     │
                    │ Script Activity     │
                    │ Incremental Load    │
                    │ Scheduled Trigger   │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │    ADLS Gen2        │
                    │   Bronze Layer      │
                    │      Raw Data       │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │     Databricks      │
                    │      PySpark        │
                    ├─────────────────────┤
                    │ Cleaning            │
                    │ Deduplication       │
                    │ Validation          │
                    │ Joins               │
                    │ Business Logic      │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │    Silver Layer     │
                    │    Delta Tables     │
                    │   Cleaned Data      │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │     Gold Layer      │
                    │    Delta Tables     │
                    │ Business Ready Data │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │      Power BI       │
                    │ Customer 360 Report │
                    │ KPIs & Dashboards   │
                    └─────────────────────┘
```

------------------------------------------------------------------------

## 🔄 Data Flow

### 1. Data Sources

Data is collected from multiple sources:

-   **Azure SQL Database**
    -   Customers
    -   Orders
-   **REST API / JSON**
    -   Customer feedback
    -   Customer activity
-   **GitHub CSV Files**
    -   Products
    -   Stores

### 2. Data Ingestion with Azure Data Factory

Azure Data Factory is used as the orchestration and ingestion layer.

ADF pipelines move data from the source systems into **ADLS Gen2**.

Activities used in the project include:

-   Copy Activity
-   Lookup Activity
-   Script Activity
-   Parameterized pipelines
-   Scheduled triggers
-   Incremental loading

### 3. Bronze Layer

Raw data is stored in the **Bronze layer** of ADLS Gen2.

Example structure:

``` text
bronze/
├── customers/
├── orders/
├── products/
├── stores/
├── feedback/
└── activity/
```

The Bronze layer keeps the source data with minimal transformation.

### 4. Silver Layer

Databricks and PySpark are used to process the Bronze data.

Main transformations include:

-   Removing duplicate records
-   Handling null values
-   Data type conversion
-   Data validation
-   Joining datasets
-   Standardizing columns
-   Applying business rules
-   Handling incremental data
-   Creating Delta tables

The cleaned data is stored in the **Silver layer** using Delta format.

### 5. Gold Layer

The Gold layer contains business-ready datasets used for analytics.

Examples:

``` text
gold/
├── customer_360/
├── customer_summary/
├── sales_analysis/
├── product_performance/
├── store_performance/
└── feedback_analysis/
```

The main **Customer 360** dataset combines information from different
customer-related sources to provide a complete view of customer
behavior.

### 6. Power BI

Power BI connects to the Gold layer and is used to create dashboards and
reports.

The dashboard can provide insights such as:

-   Total customers
-   Total orders
-   Total revenue
-   Average order value
-   Customer segments
-   Top customers
-   Sales trends
-   Product performance
-   Store performance
-   Customer feedback analysis

------------------------------------------------------------------------

## 🥉🥈🥇 Medallion Architecture

This project follows the **Medallion Architecture**.

  -----------------------------------------------------------------------
  Layer                   Purpose                 Technology
  ----------------------- ----------------------- -----------------------
  Bronze                  Store raw source data   ADLS Gen2

  Silver                  Clean and transform     Databricks + PySpark +
                          data                    Delta Lake

  Gold                    Business-ready datasets Delta Lake

  Consumption             Reporting and           Power BI
                          visualization           
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## ⚙️ Technologies Used

-   **Azure Data Factory**
-   **Azure Data Lake Storage Gen2**
-   **Azure Databricks**
-   **PySpark**
-   **Delta Lake**
-   **Azure SQL Database**
-   **REST API / JSON**
-   **GitHub**
-   **Power BI**
-   **SQL**

------------------------------------------------------------------------

## 🚀 Key Features

### Incremental Loading

Instead of loading the complete dataset every time, the pipeline
identifies new or modified records and processes only the required data.

This helps reduce:

-   Processing time
-   Data movement
-   Compute cost
-   Duplicate processing

### Parameterized Pipelines

ADF pipelines are parameterized so that the same pipeline logic can be
reused for different tables and datasets.

### Data Quality

Data quality checks are performed during processing to handle:

-   Duplicate records
-   Null values
-   Invalid data types
-   Missing values
-   Inconsistent records

### Delta Lake

Delta Lake is used for reliable data storage in the Silver and Gold
layers.

It provides features such as:

-   ACID transactions
-   Schema enforcement
-   Schema evolution
-   Reliable updates
-   Time travel

### Monitoring

ADF monitoring is used to track pipeline execution and identify
failures.

Pipeline monitoring helps check:

-   Pipeline status
-   Activity status
-   Execution time
-   Failed activities
-   Error messages
-   Trigger runs

------------------------------------------------------------------------

## 📂 Project Structure

``` text
customer-360-data-platform/
│
├── data/
│   ├── products.csv
│   └── stores.csv
│
├── adf/
│   ├── pipelines/
│   ├── datasets/
│   ├── linked-services/
│   └── triggers/
│
├── databricks/
│   ├── bronze/
│   ├── silver/
│   └── gold/
│
├── sql/
│   ├── customers.sql
│   └── orders.sql
│
├── powerbi/
│   └── customer-360-dashboard/
│
├── architecture/
│   └── customer-360-data-flow.png
│
└── README.md
```

------------------------------------------------------------------------


## 📈 Business Value

The platform helps transform raw data into useful business information.

``` text
Raw Data
   ↓
Clean Data
   ↓
Integrated Data
   ↓
Business Insights
```

The final Customer 360 dashboard can help users analyze customer
behavior, sales performance, product performance, and customer
engagement.

------------------------------------------------------------------------

## 🎯 Project Outcome

This project demonstrates an end-to-end **Azure Data Engineering
pipeline** covering:

-   Data ingestion
-   Data lake storage
-   ETL/ELT processing
-   PySpark transformations
-   Medallion architecture
-   Delta Lake
-   Incremental loading
-   Data quality
-   Pipeline monitoring
-   Business intelligence
-   Power BI reporting

------------------------------------------------------------------------

## 👨‍💻 Author

**Rohan Kumar Raj**

B.Tech -- Computer Science & Engineering\
Data Engineering / Data Science

------------------------------------------------------------------------

## ⭐ Project Highlights

**Sources → ADF → ADLS Gen2 Bronze → Databricks/PySpark → Delta Silver →
Delta Gold → Power BI**

This project demonstrates how multiple raw data sources can be
transformed into a centralized **Customer 360 analytics platform**.
