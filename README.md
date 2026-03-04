# Enterprise Risk Monitoring Pipeline

-> Risk Data Analytics Pipeline (Jira → BigQuery → Dashboard)

This project demonstrates a small data analytics pipeline designed to transform operational issue data into a structured risk monitoring system.

The pipeline ingests issue data from Jira, transforms it into consistent data models in BigQuery, and calculates risk scores that power an analytics 
dashboard used for monitoring operational risk and SLA breaches.

The goal of the project was to simulate how teams can build a lightweight analytics workflow to support risk and compliance monitoring across multiple 
product areas.

-> What the pipeline does

1. Extracts issue data from Jira using the REST API
2. Loads the data into BigQuery raw tables
3. Transforms operational data into structured analytics models
4. Calculates weighted risk scores using SQL
5. Enables dashboards for monitoring risk signals and SLA breaches

This mirrors how organizations transform multiple operational datasets into structured analytics models for monitoring risk across systems.

-> Architecture

Jira API → Python ingestion script → BigQuery raw tables → SQL transformation → Risk scoring model → Dashboard

-- Data pipeline components

-> Jira Data Ingestion

`jira_to_bigquery.py`

Python script that extracts issue data from the Jira REST API and loads it into BigQuery.

Example fields ingested:

* issue_id
* status
* created_date
* last_updated
* due_date
* summary

This simulates a lightweight ETL pipeline moving operational issue data into an analytics environment.

-> BigQuery Data Tables

`jira_issues_raw`

Operational issue data extracted directly from Jira.

`issues_raw`

Governance metadata used for risk evaluation, including:

* product_area
* issue_type
* severity
* data_sensitivity
* regulation
* owner_team
* detected_via

`issues_joined`

A unified dataset created by joining Jira operational data with governance metadata.
This dataset enables issue monitoring across product areas and regulations.

`issues_scored_final`

Final analytics dataset produced by the SQL scoring model.

This table contains the calculated metrics used by the monitoring dashboard, including:

* severity_weight
* sensitivity_weight
* aging_weight
* open_days
* risk_score
* risk_level
* SLA breach indicators

This serves as the primary dataset for risk analytics and prioritization.

-> Risk Scoring Model

`risk_scoring.sql`

SQL model used to calculate a weighted risk score for each issue.

Risk score formula:

(severity_weight × 2)

* (sensitivity_weight × 2)
* (aging_weight × 1)

Aging weight rules:

* < 60 days → 1
* 60–120 days → 2
* > 120 days → 3

This scoring model allows issues to be prioritized based on severity, data sensitivity, and how long the issue has remained open.

-> Risk Monitoring Dashboard

The final dataset powers a monitoring dashboard that tracks:

* total open issues
* SLA breaches
* risk score distribution
* risk by product area
* severity distribution
* issue aging trends

This demonstrates how operational issue data can be transformed into a monitoring system for risk analytics.

The dashboard export is included in the repository.

-> Technologies used

Python
Jira REST API
Google BigQuery
SQL
Looker Studio
GitHub

-> Why I built this

I built this project to demonstrate how operational issue data can be transformed into structured analytics models that enable continuous monitoring of 
risk signals and compliance metrics.

