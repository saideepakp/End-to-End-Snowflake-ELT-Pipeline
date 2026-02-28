# Cricket Data Pipeline — End-to-End ELT with Snowflake
### Built with Snowflake | SQL | Semi-Structured JSON Data | Data Source: Medium

---

## About the Project

This project engineers a production-style ELT pipeline built entirely in Snowflake, designed to ingest, transform, and model raw cricket match data from semi-structured JSON files into a clean, analytics-ready dimensional schema. The dataset, sourced from Medium, contains detailed ball-by-ball cricket match records including match metadata, player lineups, innings, deliveries, wickets, and officials.

The pipeline was architected across four distinct schema layers, each serving a specific purpose in the data flow, from raw ingestion to consumption-ready fact and dimension tables. Every step was engineered using Snowflake-native capabilities including internal stages, COPY INTO, VARIANT parsing, FLATTEN with lateral joins, and referential integrity constraints — demonstrating a real-world cloud data engineering workflow from start to finish.

**Tools and Technologies Used**

| Tool | Purpose |
|---|---|
| Snowflake | Cloud data platform and pipeline execution |
| SQL | All transformation, modeling, and data loading logic |
| JSON | Semi-structured source data format |
| Snowflake Internal Stage | Local to cloud file ingestion |
| COPY INTO | Bulk data loading from staged JSON files |
| VARIANT / FLATTEN | Semi-structured JSON parsing and flattening |

---

## Problem Statement

Raw cricket match data in JSON format is inaccessible to analysts and business intelligence tools in its native form. Without a structured pipeline, ball-by-ball records, player data, match outcomes, and delivery statistics cannot be queried, aggregated, or used to generate meaningful cricket analytics. The challenge was to design and implement a scalable, layered ELT architecture in Snowflake that reliably ingests raw JSON, parses and cleans it into structured tables, enforces relational integrity, and delivers a dimensional data model ready for downstream reporting and analysis.

The pipeline was built to solve the following engineering challenges:

- How can semi-structured JSON cricket data be ingested into Snowflake reliably from a local file system?
- How should nested and array-type JSON structures be parsed and flattened into structured, typed tables?
- What multi-layer schema design best separates ingestion, transformation, and analytical consumption?
- How can relational integrity be enforced across entities like matches, players, deliveries, venues, and referees?
- How can a production-style dimensional model be built to serve downstream cricket analytics and reporting?

---

## Pipeline Architecture

The pipeline follows a four-layer schema design, with each layer serving a distinct purpose:

```
LAND → RAW → CLEAN → CONSUMPTION
```

| Layer | Schema | Purpose |
|---|---|---|
| Ingestion | LAND | Snowflake internal stage and custom JSON file format |
| Storage | RAW | Raw JSON stored in VARIANT, OBJECT, and ARRAY columns with file metadata |
| Transformation | CLEAN | Flattened, typed, and relationally constrained structured tables |
| Analytics | CONSUMPTION | Star schema with dimension and fact tables for reporting |

---

## What Was Built

**Data Ingestion (LAND and RAW)**

The LAND schema was established as the ingestion entry point, housing a Snowflake internal stage configured to load JSON files from a local system and a custom JSON file format defined for consistent and structured ingestion. From the LAND layer, raw data was loaded into a transient table in the RAW schema using COPY INTO, capturing the full JSON payload split into three typed columns: meta as OBJECT, info as VARIANT, and innings as ARRAY. File-level metadata including the source filename and last modified timestamp was also captured for traceability, ensuring every record can be traced back to its origin file.

**Data Transformation (CLEAN)**

Three structured tables were engineered in the CLEAN schema by parsing the raw JSON using lateral FLATTEN and VARIANT dot-notation extraction. The match table extracted 21 fields from the info object including season, match type, venue, city, teams, toss details, outcome, and officials. The player table used a nested lateral flatten to extract each player's name and associated country. The delivery table performed a deep multi-level lateral flatten across innings, overs, deliveries, extras, wickets, and fielders to produce ball-by-ball records with outer joins to handle nullable fields. Primary and foreign key constraints were applied across all three tables to enforce relational integrity within the CLEAN schema.

**Analytics and Dimensional Modeling (CONSUMPTION)**

A full star schema was designed and populated in the CONSUMPTION layer. Six dimension tables were created covering date, team, player, venue, referee, and match type. A comprehensive date dimension was generated for the range 2000 to 2023 with attributes including day, month, year, quarter, day of week name, and weekend flag. A match fact table was built with 28 columns capturing team-level aggregates per match including overs played, balls bowled, extra balls, extra runs, total runs scored, and wickets lost for both teams, alongside toss and result information. A delivery fact table was built at ball-by-ball granularity capturing batter, bowler, non-striker, runs, extras, and dismissal details, all resolved to surrogate keys via dimension table joins.

---

## Key SQL Concepts Applied

- Snowflake internal stage creation and custom JSON file format definition
- COPY INTO for bulk semi-structured JSON ingestion with metadata capture
- VARIANT, OBJECT, and ARRAY column types for raw semi-structured storage
- FLATTEN with lateral joins for multi-level nested JSON parsing
- Outer lateral joins for handling nullable arrays such as extras, wickets, and fielders
- Data type casting and null handling across all transformation layers
- Primary key, foreign key, and NOT NULL constraint enforcement
- Star schema dimensional modeling with surrogate key generation using ROW_NUMBER
- Aggregate fact table construction using conditional SUM and MAX with GROUP BY
- Date dimension generation and population from a staged date range table


---

## Outcome

This project delivers a fully functional, production-style ELT pipeline that transforms raw, deeply nested cricket JSON into a clean, relationally constrained, and analytically ready dimensional data model. The four-layer schema architecture ensures scalability and maintainability, while the Snowflake-native approach across ingestion, transformation, and modeling demonstrates practical cloud data engineering skills directly applicable to real-world data infrastructure and analytics platforms.

---

## Connect with Me

**Sai Deepak Poondla**
Data Analyst | Power BI | SQL | Python | Tableau

[LinkedIn](https://www.linkedin.com/in/your-linkedin-handle) | [Portfolio](https://your-portfolio-link.com)
