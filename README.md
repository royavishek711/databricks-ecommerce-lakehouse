# 🛒 End-to-End E-Commerce Data Lakehouse 

## 📌 Project Overview
As data volumes scale, traditional Python-based ETL pipelines often face performance bottlenecks, resulting in delayed BI reporting and frozen dashboards. This project resolves those scalability challenges by replacing legacy scripts with a robust, distributed **Data Lakehouse** architecture built on **Databricks**. 

The pipeline ingests raw e-commerce data from AWS S3, processes it through a Medallion Architecture (Bronze, Silver, Gold) using PySpark, and serves highly optimized, denormalized views for downstream Business Intelligence and AI integration.

## 🏗️ Architecture & Technology Stack

```mermaid
graph TD
    %% Define Styles
    classDef aws fill:#FF9900,stroke:#232F3E,stroke-width:2px,color:black;
    classDef databricks fill:#FF3621,stroke:#242424,stroke-width:2px,color:white;
    classDef bronze fill:#CD7F32,stroke:#5A3A18,stroke-width:2px,color:white;
    classDef silver fill:#C0C0C0,stroke:#4B4B4B,stroke-width:2px,color:black;
    classDef gold fill:#FFD700,stroke:#B8860B,stroke-width:2px,color:black;
    classDef bi fill:#00A972,stroke:#004D34,stroke-width:2px,color:white;
    classDef unity fill:#1E283C,stroke:#0F141E,stroke-width:2px,color:white;

    %% Source & Storage
    A[Raw CSV Files] -->|Ingested to| B[(AWS S3 Data Lake)]
    B:::aws

    %% Databricks Pipeline
    subgraph "Databricks Data Lakehouse"
        direction TB
        
        %% Unity Catalog Wrapper
        subgraph "Unity Catalog (Data Governance & RBAC)"
            direction LR
            C[(Bronze Layer<br/>Raw Data)]:::bronze -->|Clean, Filter, Normalize<br/>PySpark| D[(Silver Layer<br/>Cleansed Data)]:::silver
            D -->|Join, Aggregate, BI Prep<br/>PySpark| E[(Gold Layer<br/>Business-Ready)]:::gold
        end
        style "Unity Catalog (Data Governance & RBAC)" fill:#f4f4f9,stroke:#1E283C,stroke-width:2px,stroke-dasharray: 5 5
    end

    B --> C

    %% Downstream Analytics
    E --> F[Databricks SQL<br/>Dashboards]:::bi
    E --> G[Databricks Genie<br/>GenAI Insights]:::bi
```

* **Compute & Processing:** Databricks, Apache Spark (PySpark)
* **Storage Layer:** Delta Lake, AWS S3
* **Data Governance:** Unity Catalog
* **Analytics & Visualization:** Databricks SQL, Databricks Dashboards, Databricks Genie (AI Assistant)

## ⚙️ The Medallion Pipeline
This project logically organizes data to progressively improve structure and quality:

### 🥉 Bronze Layer (Raw Data)
* Ingested raw e-commerce CSV files (Transactions, Customers, Products, Dates) directly from the AWS S3 data lake into Databricks.
* Retained the original schema and loaded it into Delta tables to establish a resilient, historical system of record.

### 🥈 Silver Layer (Cleansed & Conformed)
* Executed distributed PySpark transformations to sanitize the data.
* Handled null values, removed duplicate records, and normalized schemas.
* Applied Regular Expressions (RegEx) to clean text fields (e.g., stripping invalid characters from brand codes).
* Enforced strict data typing (e.g., converting string timestamps to native Date/Time formats).

### 🥇 Gold Layer (Business-Ready)
* Joined disparate dimension and fact tables into highly denormalized views.
* Generated calculated KPIs (e.g., `Gross Amount`, `Discount Amount`, `Net Sales`).
* Implemented currency normalization using spot conversion rates to standardize global transactions.
* Optimized the final schema specifically for rapid BI dashboard rendering.

## 🛡️ Data Governance with Unity Catalog
To ensure enterprise-grade security, this project leverages **Unity Catalog** for centralized metadata management. 
* Implemented fine-grained, role-based access control (RBAC).
* Separated read/write privileges between mock Data Engineering and Data Analytics teams.
* Maintained automated data lineage tracking across the entire Lakehouse.

## 📊 Analytics & Insights
The Gold layer powers an interactive Databricks SQL Dashboard, utilizing the Genie AI assistant to translate natural language queries into automated insights. Visualizations include:
* **Monthly Sales Trends:** Tracking revenue growth and order volume over time.
* **Category Performance:** Highlighting top-performing product segments (e.g., Electronics vs. Home & Kitchen).
* **Heatmap Analysis:** Uncovering peak purchasing hours and days to optimize targeted marketing campaigns.

---
*Developed by Avishek Roy.*
