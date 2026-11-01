# Metadata Framework

## `source_config`
`source_id` (PK), `source_name`, `source_type`, `connection_ref`, `database_name`, `is_active`.

## `table_config`
`table_id` (PK), `source_id` (FK), `schema_name`, `table_name`, `load_type` (`FULL`/`INCREMENTAL`), `watermark_column`, `last_successful_watermark`, `target_layer`, `is_active`.

## `pipeline_run_log`
`run_log_id`, `run_id`, `pipeline_name`, `table_id`, `start_time_utc`, `end_time_utc`, `status`, `rows_read`, `rows_written`, `watermark_value`, `error_message`.

## Contract
- Validate `load_type`; incremental mode requires a valid watermark column.
- Use a unique landing folder for each attempt.
- Log every success and failure.
- Advance watermarks only after successful, validated ingestion.
- Prevent overlapping runs per table unless concurrency is deliberately supported.
- Treat configuration as data, but validate schema/table/column identifiers before constructing dynamic SQL.
