# Technical Architecture of the NYC Data Platform

---

## 1. Architecture Objectives
This architecture is designed to:
* Ingest reliably data on a daily basis from public available data
* Prevent raw data immutability (for the sake of auditability)
* Enable analytics-ready modeling
* Support self-service BI
* Integrate forecasting with ML capabilities
* Remain scalable and cost-efficient

The design is aimed at reflecting production-ready modern data infrastructures for enterprises and public sector environments.

## 2. High level overview of the infra

```
NYC Open Data (API)
        |
        |
Cloud Composer(Airflow)

        |
        |
Cloud Storage(Raw landing zone)

        |
        |
Big Query
|-- Raw Layer
|-- Staging Layer
|-- Semantic(Business) Layer
|-- ML layer

        |
        |
dbt(ELT transformations & Metrics building)
        |
        |
Looker Studio(BI & Dashboards)
        |
        |
Vertex AI (Forecasting)

```

## 3. Archi Components breakdown
### 3.1 Airflow Cloud Composer

**Role**

* Helps orchestrating the daily ingestion of FHV Active Data
* Ensure reliability, retries and monitoring

**Key design specification**

* Snapshot based historical tracking
* Idempotents DAGs
* Incremental loads using update timestamps

---
### 3.2 Cloud Storage - Raw data Storage

**Role**

* Acts as a landing zone for raw ingested data

**Key design specification**

* Append only
* Partitioned by ingestion date
* Data stored in structured format (JSON/Parquet)

---
### 3.3 Big Query - Data Warehouse

**Role**

* Acts as the centralized analytical engine

**Key design specification**

* raw_fhv: source-aligned data with no tranfo
* staging_fhv: cleansed and standardized data
* analytics_fhv: facts and dimensions ready for business use
* ml_fhv: features and predictions

**Partition strategy**

* Partition by snapshot_date
* cluster by primary identifiers if relevant
---

### 3.4 Transformations – dbt

**Role**

* Data cleaning, modeling, testing, and documentation

**Key Features Used**

* Staging models
* Fact and dimension models
* Tests and snapshots
* Documentation generation

---
### 3.5 Business Intelligence – Looker

**Role**

* Executive dashboards and self-service analytics

**Design Principles**

* KPI-driven views
* Single source of truth through the semantic layer
* Decision-oriented dashboards
---
### 3.6 ML - Vertex AI

**Role**

* Forecast future FHV fleet size

**Capabilities**

* Manage model training and deployment
* Schedule retraining
* Model registry
* Predictions stored in BQ
---
## 4. Non Functional-Requirements of the data platform

|Requirements              | Approcah                    |
|--------------------------|-----------------------------|
|Scalability               |GCloud Storage and BQ        |
|Auditability              |Immutable raw data (GCS)     |
|Reliability               |Airflow retries & monitoring |
|Cost controls             |Partitioning & Scheduled jobs|
|Security                  |IAM roles & Service Accounts |

```
As a reminder, non functional requirements are a must in any project or software design endeavors as it helps define how the system we're building should work as opposed to what the system should do(funcional requirements). Here's a link to have more explanations: [https://www.geeksforgeeks.org/software-engineering/functional-vs-non-functional-requirements/]

```
---
## 5. Tradeoffs ad future developments

We chose to :
* Batch ingestion vs Realtime for simplicity and cost
* Daily snapshots over event tracking
* Manged services for self-hosted tools 

**Future extensions**

* Integration with TLC trip data if available; Right now only taxi trip past data are available.
* Geospatial analysis: The consolidated BASE could be used for that[https://data.cityofnewyork.us/Transportation/CURRENT-BASES/eccv-9dzr/about_data] 
* CO2 emission analysis
* Multi-State/City federation as now we're only tareting NYC.

