# Flink jobs removed in Spark

The following Flink jobs have been removed entirely from Spark. Do not attempt to deploy them on a Spark installation:

| Job                         | Reason for removal                                |
| --------------------------- | ------------------------------------------------- |
| questionset-republish       | Replaced by the merged publish pipeline job       |
| audit-event-generator       | Functionality retired                             |
| audit-history-indexer       | Functionality retired                             |
| auto-creator-v2             | CoKreat sync approach redesigned                  |
| content-auto-creator        | Replaced by auto-creator-v2 replacement           |
| csp-migrator                | Cloud provider migration tooling no longer needed |
| live-video-stream-generator | Video streaming now config-based, not job-based   |
| mvc-indexer                 | Functionality retired                             |
| metrics-data-transformer    | Replaced by native reports                        |
| user-cache-updater          | Replaced by API-based approach                    |
| merge-user-courses          | Functionality retired                             |
| enrolment-reconciliation    | Functionality retired                             |
| response-exhaust            | Replaced by native User Report export             |
| user-info-exhaust           | Replaced by native User Report export             |
| cassandra-migration         | Not applicable — Spark uses YugabyteDB            |
| collection-summary          | Replaced by native Admin Reports                  |
| state-admin-geo-report      | Replaced by native Admin Reports                  |
| state-admin-consent-report  | Replaced by native Admin Reports                  |
