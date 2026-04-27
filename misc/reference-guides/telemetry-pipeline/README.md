# Telemetry Pipeline

This page explains how telemetry data flows from a learner's device to storage and analytics.

#### Overview

Every interaction a learner has with content — opening a lesson, answering a question, closing a video — generates a telemetry event. These events flow through a pipeline that deduplicates, validates, enriches, and stores them.

#### Event types

| Event type | When it fires                                         |
| ---------- | ----------------------------------------------------- |
| START      | Learner opens a content item or course                |
| END        | Learner closes a content item or course               |
| INTERACT   | Learner interacts with the UI (clicks, taps, scrolls) |
| IMPRESSION | A new page or screen is viewed                        |
| ASSESS     | Learner submits an answer in an assessment            |
| ERROR      | A technical error occurs in the client app            |

#### The pipeline — step by step

```
1. Client app (portal, mobile, desktop)
   Batches events locally (max 32 events or 30-second interval)
   Sends batch to:
   POST /api/data/v1/telemetry
         │
         ▼
2. Telemetry Service (always running — part of default install)
   Validates the batch format
   Writes events to Kafka topic: telemetry.raw
         │
         ▼
3. Flink: telemetry-extractor
   Reads from telemetry.raw
   Extracts individual events from batches
         │
         ▼
4. Flink: pipeline-preprocessor
   Deduplication: checks event UUID against Redis
   (duplicate events are dropped — this handles network retries)
   Validation: checks event JSON against Sunbird Telemetry Spec
   (invalid events are written to a dead-letter topic)
         │
         ▼
5. Flink: de-norm
   Enriches events with denormalised metadata
   from Redis user cache and content cache
   (adds user name, course name, content title to the event)
         │
         ▼
6a. [Without Obsrv add-on]
    Events are backed up to cloud storage via Secor
    (Kafka → S3/Azure Blob/GCP Storage)
    Native reports are served from YugabyteDB aggregates
         │
6b. [With Obsrv add-on]
    Flink: druid-validator
    Final validation before Druid ingestion
         │
         ▼
    Apache Druid
    Time-series OLAP storage
    Fast ad-hoc aggregation queries
         │
         ▼
    Apache Spark (batch summarisers)
    Compute session summaries, course completion aggregates
    Write summaries back to Kafka → Druid
         │
         ▼
    Apache Superset
    Custom dashboards and reports
    
```

#### Data backup

Regardless of whether Obsrv is enabled, all raw telemetry events are backed up to cloud storage via Secor (a Kafka-to-cloud-storage backup service developed by Pinterest). This ensures no telemetry data is lost even if Obsrv is not enabled at the time of data collection.

If you enable Obsrv after an initial period of data collection, historical telemetry data can be loaded from cloud storage into Druid for retrospective analysis.

<br>
