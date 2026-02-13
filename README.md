# NYC FHV Data Platform
## Public Administration Mobility Analytics & Forecasting

Building a full-blown data platform for the nyc department of transportation to help them oversee the state of fhv and plan actions based on insights generated

---

## 1. Project Overview

New York City oversees and regulates thousands of **For-Hire Vehicles (FHVs)** operating daily across the city. These vehicles play a critical role in:

* Urban mobility and congestion
* Environmental impact
* Market competition and regulation
* Infrastructure planning

This project aims to deliver a **production-grade data platform** designed to help NYC public administration **monitor, analyze, and forecast the state of active FHVs** using daily public data.

The platform follows modern **data & analytics engineering, and MLOps best practices**, and is designed to be scalable, auditable, and decision-oriented.

---

## 2. Business Problem Statement

NYC decision-makers currently lack:

* A **single source of truth** for active FHVs
* Timely visibility into **fleet growth, churn, and concentration**
* Forward-looking insights to support **regulatory and infrastructure decisions**

### Objective

> Build a trusted, scalable data platform that transforms daily public FHV data into **actionable KPIs, executive dashboards, and short- to medium-term forecasts**.

---

## 3. Stakeholders & Use Cases

### Primary Stakeholders

* **Policy Makers** – regulation, caps, incentives, etc.
* **Regulatory Bodies (TLC)** – market oversight & compliance
* **Analytics Teams** – reporting & exploratory analysis

### Key Use Cases

| Stakeholder | Decision Enabled                   |
| ----------- | ---------------------------------- |
| Policy      | Adjust vehicle caps or regulations |
| Planning    | Anticipate infrastructure strain   |
| Regulation  | Monitor market concentration       |
| Analytics   | Detect trends and anomalies        |

---

## 4. Core Business Questions

These questions drive the entire technical architecture:

### Fleet Monitoring

* How many FHVs are **active today**?
* How has the active fleet evolved over time?

### Market Structure

* Which bases operate the largest fleets?
* How concentrated is the market?

### Vehicle Composition

* What is the mix of vehicle types?
* How is this mix changing?

### Dynamics

* How many vehicles are newly activated or churned daily?
* Are there seasonal patterns?

### Forward-Looking

* How many FHVs are expected in **3, 6, and 12 months**?

---

## 5. Success Metrics (KPIs)

These KPIs form the **semantic layer** used consistently across dashboards and models.

### Core KPIs Identified

* Total Active FHVs
* Net New Vehicles (daily / monthly)
* Churned Vehicles
* Market Share by Base
* Vehicle Type Distribution

### Forecasting KPIs

* Forecasted Active FHVs
* Forecast Accuracy (MAPE, RMSE)

---

## 6. Data Source

### Primary Dataset

**NYC Open Data – For-Hire Vehicles (FHV) Active Dataset**

### Characteristics

* Update Frequency: Daily
* Source Reliability: Official public dataset
* Access Method: Socrata API
* Historical Availability: Yes (via snapshotting)

### Constraints & Mitigations

| Constraint           | Mitigation                 |
| -------------------- | -------------------------- |
| Schema changes       | Schema validation & alerts |
| Snapshot-only status | Daily snapshot ingestion   |
| Missing updates      | Data freshness checks      |

---

## 7. Project Scope

### In Scope

* Daily ingestion of FHV Active data
* Historical tracking via snapshots
* Analytics-ready data modeling
* Executive dashboards
* Fleet size forecasting

### Out of Scope (Phase 1)

* Real-time tracking
* Trip-level ride data
* Enforcement or violation data
* External data enrichment (weather, traffic)

---

## 8. High-Level Architecture

```
NYC Open Data
     |
Airflow (Cloud Composer)
     |
Cloud Storage (Raw Data)
     |
BigQuery (Raw → Analytics)
     |
dbt (Models + Metrics)
     |
Looker Studio (Dashboards)
     |
Vertex AI (Forecasting)
```

The architecture follows a **layered, auditable design** suitable for public-sector analytics platforms.

---

## 9. Risks & Assumptions

### Key Assumptions

* Dataset is updated daily and remains publicly available
* Daily granularity is sufficient for policy-level decisions
* Active status reflects regulatory reality

### Key Risks

| Risk                  | Mitigation                   |
| --------------------- | ---------------------------- |
| Source schema changes | Automated validation         |
| Data gaps             | Snapshot completeness checks |
| KPI misalignment      | Early stakeholder validation |

---

## 10. Delivery Roadmap (12 Weeks)

* **Weeks 1–2:** Strategy, discovery, architecture, governance
* **Weeks 3–6:** Ingestion, data quality, modeling, semantic layer
* **Weeks 7–9:** BI dashboards, forecasting, Vertex AI deployment
* **Weeks 10–12:** CI/CD, monitoring, optimization, handover

---

## 11. Tech Stack to be used

* **Cloud:** Google Cloud Platform (GCP)
* **Orchestration:** Cloud Composer (Airflow)
* **Storage:** Cloud Storage
* **Warehouse:** BigQuery
* **Transformation:** dbt
* **BI:** Looker Studio
* **ML:** Vertex AI
* **Version Control:** GitHub

---

## 13. Environment Strategy (Simplified for Phase 1)

For the initial implementation of this project, a single GCP environment will be used to keep development focused and execution simple.

This allows:

* Faster iteration
* Reduced infrastructure complexity
* Lower cloud costs during early development

**Important Note on Production Best Practice**

In a real public-sector or enterprise deployment, the recommended approach is to implement separate DEV, TEST, and PROD environments with:

* Isolated GCP projects or logically separated datasets
* Environment-specific Cloud Storage buckets
* Separate BigQuery datasets per environment
* Controlled promotion via CI/CD pipelines
* Strict IAM boundaries protecting production data

This separation ensures:

* Safer experimentation
* Controlled releases
* Auditability and compliance
* Reduced operational risk

The current single-environment setup is intentionally simplified for portfolio development. The architecture is designed so that environment separation can be introduced without structural refactoring.

## 14. Next Steps

* Will reflect on possible optimization when done!
