# Reporting and analytics

## Built-in reports

Every Spark deployment ships with native reports that cover common administrative and learner analytics needs. No additional analytics infrastructure is required for these basics.

**Admin reports:**

* Content by status, shown as a donut chart for Draft, Live, and Retired content
* Content by grouping, filterable by taxonomy dimensions such as subject and grade
* Top contributors, shown as a bar chart of active content creators
* Most popular content, comparing enrolments and views per course
* Admin Course Summary, a searchable table with enrolment count, completion count, completion percentage, certificate count, and last updated date, with CSV export
* User Analytics, including total users, breakdown by role, monthly growth trend, and users by app type

**User Report:**

* Courses completed, in progress, and not started
* Progress per course with visual progress bars and last accessed date
* Certificates earned, including certificate ID and issue date
* Assessment history
* CSV export for all sections

### Full telemetry pipeline and observability

{% hint style="info" %}
**Add-on required:** Sunbird Obsrv enables end-to-end observability using Druid, Apache Spark batch summarisers, and Apache Superset custom dashboards. It is an optional Spark add-on and is disabled by default.
{% endhint %}

For deployments that need deep custom analytics, such as drop-off analysis, cohort comparison, time-series engagement data, or custom reports, the Obsrv building block can be enabled as an add-on.

**Important:** Even without Obsrv, Spark still captures and emits telemetry. Every user interaction generates a telemetry event. These events are also backed up to cloud storage through Kafka. You can enable Obsrv later without losing historical data.

The Obsrv telemetry pipeline processes:

* `START`, `END`, `INTERACT`, `IMPRESSION`, `ASSESS`, and `ERROR` events per user session
* `2 billion+` events per day at DIKSHA scale
* Deduplication, validation, enrichment, and denormalisation through Apache Flink
* OLAP storage in Apache Druid for fast ad-hoc queries
* Custom dashboards through Apache Superset
