# Olist Data Warehouse & BI Analytics

Production-oriented data warehouse project for the Brazilian Olist e-commerce dataset. The repository contains the source data, SQL scripts for profiling and transformation, dimensional warehouse loading logic, validation checks, and a Power BI dashboard.

## Contents

```text
Olist_Renamed_Queries_v3/
├── Olist_Data/                         Source datasets and exchange-rate data
├── Olist_Queries/
│   ├── 01_Setup/                       Schemas, staging tables, roles, profiling setup
│   ├── 02_Data_Profiling/              Source and staging data profiling
│   ├── 03_Quality_Checks/               Data quality and integrity checks
│   ├── 04_Data_Cleaning/                Standardisation and cleansing transformations
│   ├── 05_Data_Loading/                 Dimension and fact table loading
│   ├── 06_Post_Load_Processing/         Post-load calculations and currency columns
│   └── 07_Validation/                   Source-to-warehouse reconciliation
├── Olist_Dashboard/                     Power BI dashboard
└── Olist_Data_Warhouse_Backups/         Local SQL Server backup files, not tracked by Git
```

## Architecture

The solution follows a layered warehouse pattern:

1. Source files are loaded into staging tables.
2. Profiling and quality checks identify missing, duplicate, invalid, and inconsistent records.
3. Cleaning scripts standardise source values before warehouse loading.
4. Conformed dimensions and the sales fact table are populated.
5. Post-load processing adds analytical attributes, including currency conversions.
6. Validation scripts reconcile warehouse results against the source and staging layers.
7. Power BI consumes the warehouse model for reporting and analysis.

The warehouse model includes customer, date, exchange-rate, location, order, product, product-category, and seller dimensions, together with a sales fact table.

## Prerequisites

- A Snowflake account with permission to create databases, schemas, tables, roles, and views as required by the setup scripts.
- SQL client capable of executing the `.sql` files.
- Power BI Desktop for opening the dashboard file.
- Git for obtaining the repository.

## Getting started

### 1. Clone the repository

```bash
git clone https://github.com/JulanaPremasekara/OLIST_Data_Warehouse.git
cd OLIST_Data_Warehouse
```

### 2. Load the source data

Upload the CSV and JSON files from `Olist_Renamed_Queries_v3/Olist_Data/` to the approved staging location for your warehouse environment. The geolocation dataset is intentionally excluded from Git because of its size; obtain it separately and place it in the same directory before loading.

Do not commit database backup files or credentials. Local `.bak` files and the large geolocation CSV are excluded by `.gitignore`.

### 3. Execute the SQL workflow

Run the scripts in the numbered directory order:

```text
01_Setup
02_Data_Profiling
03_Quality_Checks
04_Data_Cleaning
05_Data_Loading
06_Post_Load_Processing
07_Validation
```

Run the setup scripts first and replace environment-specific values such as database, schema, warehouse, role, stage, and file locations according to your Snowflake account. Review profiling and quality-check results before loading production tables.

### 4. Open the dashboard

Open `Olist_Renamed_Queries_v3/Olist_Dashboard/DWBI_Dashboard_final.pbix` in Power BI Desktop. Configure the data source connection and credentials for the target warehouse, then refresh the model.

## Data model

The analytical model is designed around sales and order activity. Typical analytical use cases include:

- Revenue and order trends over time
- Product and category performance
- Seller performance
- Customer and geographic analysis
- Payment and review analysis
- Currency-normalised reporting

The SQL scripts are the authoritative definition of the staging, cleaning, dimensional, fact, and validation logic.

## Data quality and validation

Before publishing refreshed analytics, execute the checks in:

- `03_Quality_Checks/` for table-level data quality rules
- `07_Validation/` for source-to-staging and staging-to-warehouse reconciliation

Investigate failed checks before promoting a load. Validation should cover row counts, key uniqueness, nullability, referential integrity, date consistency, and measure reconciliation.

## Repository and large-file policy

GitHub rejects files larger than 100 MB and recommends keeping individual files below 50 MB. The following artifacts are therefore kept outside version control:

- `Olist_Data/olist_geolocation_dataset.csv`
- `Olist_Data_Warhouse_Backups/*.bak`

For reproducible deployments, store these artifacts in approved object storage or an internal artifact repository and document their location and checksum in the deployment environment. Never store passwords, access tokens, connection strings, or production extracts in this repository.

## Security and operational guidance

- Use least-privilege roles for ingestion, transformation, validation, and reporting.
- Keep credentials in a secrets manager or the warehouse's secure authentication mechanism.
- Separate development, test, and production schemas.
- Make loading scripts idempotent where possible and use controlled reruns after failures.
- Capture row counts and validation outcomes as load metadata for auditability.
- Review dashboard refresh credentials and permissions before sharing the PBIX file.

## Project status

This repository contains the completed SQL analysis, warehouse loading workflow, validation scripts, and BI dashboard for the Olist dataset. Environment-specific deployment configuration and external storage for excluded large artifacts must be supplied by the operator.

## License and dataset attribution

This project is intended for educational and analytical use. Confirm the applicable terms and attribution requirements for the Olist dataset before redistributing the source data or derived artifacts.
