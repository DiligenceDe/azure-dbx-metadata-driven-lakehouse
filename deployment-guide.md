# Deployment Guide

> Build and validate these steps in your own Azure environment. This package has not been deployed or tested against your resources.

1. **Source:** confirm AdventureWorks version, table/column names, primary keys, watermark semantics, and least-privilege read access. Use Self-hosted IR for local/on-prem SQL Server if needed.
2. **Storage:** create ADLS Gen2 with hierarchical namespace; provision raw and lakehouse paths; grant minimum required access to ADF and Databricks identities.
3. **Control DB:** create a dedicated database and run the metadata DDL. Review the seed data before running.
4. **ADF:** configure SQL/control DB/ADLS linked services, preferably using managed identity or Key Vault-backed secrets. Build parameterized datasets and Lookup → ForEach → Copy. Add success/failure logging and retries.
5. **Databricks:** configure secure ADLS access, catalog/schema or paths, and notebook parameters. Run Bronze, then Silver, then Gold.
6. **GitHub:** push sanitized source, enable pull requests, and confirm CI passes. CI validates code/config only; it does not deploy Azure.
7. **Acceptance:** verify full load, incremental load, failed-run log, rerun idempotency, row reconciliation, DQ checks, Gold outputs, and no secrets in the repository.
