# Snowflake-CDC-Streams-Tasks-Pipeline

Project Overview

This project implements a complete end-to-end Change Data Capture (CDC) pipeline in Snowflake for integrating Customer and Transaction data into an SCD Type 2 (Slowly Changing Dimension) target model.
It enables continuous ingestion of INSERT, UPDATE, and DELETE events using Snowflake Streams and Tasks, ensuring that the EDW layer always contains the latest data along with full historical records.

The pipeline is architected to support:

High-volume data ingestion (50k+ customers, 200k+ transactions)

Automated CDC processing

Historical tracking with SCD Type 2

Fully incremental, cost-efficient pipeline using native Snowflake features

This approach is suitable for real-time analytics, data warehousing, customer 360, and financial reporting use cases.

🏗️ Architecture
            ┌────────────────────────────────────────┐
            │                RAW Layer                │
            │   (Source Landing Tables in Snowflake)  │
            ├────────────────────────────────────────┤
            │ CUSTOMER_RAW                            │
            │ TRANSACTION_RAW                         │
            └───────────────┬────────────────────────┘
                            │
                            │ 1. Initial full load
                            ▼
            ┌────────────────────────────────────────┐
            │        EDW Layer (SCD Type 2)          │
            │           EDW.CUST_TRANS               │
            └───────────────┬────────────────────────┘
                            │
                            │ 2. Streams capture CDC
                            ▼
            ┌────────────────────────────────────────┐
            │           Snowflake Streams             │
            │   CUSTOMER_STREAM                       │
            │   TRANSACTION_STREAM                    │
            └───────────────┬────────────────────────┘
                            │
                            │ 3. Task processes deltas
                            ▼
            ┌────────────────────────────────────────┐
            │            Snowflake Task               │
            │   CONTROL.CUST_TRANS_CDC_TASK           │
            │   (MERGE + SCD2 logic)                  │
            └─────────────────────────────────────────┘
                            │
                            ▼
            ┌────────────────────────────────────────┐
            │  Updated SCD2 Table (Full History)     │
            │      + Current Active Records          │
            └────────────────────────────────────────┘
