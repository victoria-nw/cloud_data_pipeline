# Cloud Data Pipeline

This pipeline processes raw data through three layers (Bronze for raw ingestion, Silver for cleaning and validation, and Gold for aggregations).

## Architecture
Landing → Bronze → Silver → Gold

- **Landing** – Raw parquet files stored in ADLS Gen2
- **Bronze** – Raw data ingested into Delta tables with metadata columns
- **Silver** – Cleaned, deduplicated, validated data
- **Gold** – Aggregated, business-ready tables

## Tech Stack
- Azure Databricks 
- Azure Data Lake Storage Gen2
- Delta Lake
- Unity Catalog
- PySpark / Spark Structured Streaming
- GitHub Actions (CI/CD)

## Data Source
NYC TLC Trip Record Data – Yellow Taxi, January 2023  
Source: https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page

## Pipeline Layers

### Bronze
- Batch and streaming ingestion from landing container
- Auto Loader with automatic checkpointing and restart
- Metadata columns: ingestion_timestamp, source_file

### Silver
- Null handling and default value fills
- Data quality flags: is_valid, is_valid_time
- Deduplication using VendorID + pickup_datetime + PULocationID
- Delta merge (upsert) into silver table

### Gold
- Daily trip count and revenue aggregations
- Peak hour analysis by trip volume
- Location performance by average fare and distance
- Payment type breakdown


## Repository Structure
cloud_data_pipeline/
├── setup/          # Config and pipeline runner
├── ingestion/      # Bronze ingestion notebooks
├── silver/         # Silver transformation notebooks
├── gold/           # Gold aggregation notebooks
├── tests/          # Integration tests
├── cicd/           # CI/CD pipeline configs
└── docs/           # Screenshots and architecture diagrams
