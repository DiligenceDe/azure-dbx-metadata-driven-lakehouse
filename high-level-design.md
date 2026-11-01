# High-Level Design

## Objective
Ingest selected AdventureWorks tables from SQL Server, process them through a reusable Medallion Architecture, and publish analytical Gold datasets.

## Components
1. SQL Server / AdventureWorks — source.
2. ADF — metadata lookup, table loop, copy, notebook orchestration, retries, notifications.
3. Control database — source/table configuration and run logs.
4. ADLS Gen2 — raw landing in run-specific Parquet folders.
5. Databricks — Bronze, Silver, and Gold Delta processing.
6. GitHub Actions — pull-request validation, not Azure deployment.

## Flow
1. Lookup active table configuration.
2. ForEach active table and copy full or watermark-filtered rows to a unique landing path.
3. Log success/failure and counts.
4. Load landing files to Bronze Delta with ingestion metadata.
5. Clean, standardize, validate, and deduplicate in Silver.
6. Build `dim_customer`, `dim_product`, and `fact_sales` in Gold.
7. Validate row counts, quality metrics, and rerun behavior.

## Reliability
- Start with low concurrency; increase only after capacity tests.
- Use run-specific landing folders.
- Update watermarks only after successful landing and validation.
- Avoid blind append on retries; use controlled overwrite or Delta MERGE.
- Log sanitized error details and preserve failed-run evidence.

## Assumptions
Confirm AdventureWorks version, table names, primary keys, watermark columns, source connectivity, and Unity Catalog/storage access. Incremental timestamps may miss hard deletes; use CDC/Change Tracking or reconciliation where delete capture is required.
