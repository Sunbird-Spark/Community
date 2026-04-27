---
description: >-
  Spark is composed of six building blocks — independently deployable units that
  each handle a specific domain of functionality. Understanding the building
  block boundaries helps architects decide what
---

# Building blocks

{% tabs %}
{% tab title="SB Knowlg" %}
The content and knowledge management building block.

**Responsibilities:**

* Content lifecycle management — create, review, publish, retire
* Collection management — courses, playlists, textbooks, and their hierarchies
* Taxonomy and framework management — configurable category trees for any domain
* Content search and discovery — Elasticsearch-powered
* Media management — cloud storage integration for content assets
* Question bank — assessment content (inQuiry APIs are merged into knowlg-service in Spark)

**Primary data stores:** Janus Graph(content graph), YugabyteDB (transactional data), Elasticsearch (search index), Redis (API response cache)

**Deployed as:** knowlg-service (single merged service in Spark)
{% endtab %}

{% tab title="SB Lern" %}
The user, organisation, and learning management building block.

**Responsibilities:**

* User registration, authentication (via Keycloak), and profile management
* Organisation and multi-tenancy (channel) management
* Course batch creation and management
* Learner enrolment and progress tracking
* Certificate issuance (via Sunbird RC integration)
* Notifications (in-app and push)
* Groups and cohort management (when Groups add-on is enabled)

**Primary data stores:** YugabyteDB

**Deployed as:** lern-service (single merged service in Spark)
{% endtab %}

{% tab title="SB inQuiry" %}
The question bank building block.

**Responsibilities:**

* Question and question set creation (QuML spec)
* Question bank API — search, read, update questions
* Assessment serving and result capture

{% hint style="info" %}
**In Spark:** inQuiry APIs are merged into knowlg-service. The QuestionSet Player and QuestionSet Editor remain as independent V2 web components.
{% endhint %}

**Note:** The legacy (non-QuML) assessment approach has been retired in Spark. Only QuML-spec questions are supported.
{% endtab %}

{% tab title="SB Obsrv(add-on)" %}
The data and analytics building block.

**Responsibilities:**

* Telemetry event ingestion, deduplication, validation, and enrichment (Flink)
* OLAP storage and querying (Druid)
* Batch summarisation — session summaries, course completion aggregates (Apache Spark)
* Custom dashboards and report generation (Superset)

{% hint style="info" %}
**In Spark:** Obsrv is an optional add-on, disabled by default. The telemetry service (event ingestion endpoint) is retained in the default install. Kafka topic backup is retained, so historical data is preserved even before Obsrv is enabled.
{% endhint %}
{% endtab %}

{% tab title="SB RC" %}
The credential and registry building block.

**Responsibilities:**

* Verifiable certificate generation and storage
* Public certificate verification endpoint
* Registry operations for organisations and individuals

{% hint style="info" %}
**In Spark:** RC v1 is used as-is. RC v2 (W3C Verifiable Credentials standard) is planned for the April–June 2026 release.
{% endhint %}
{% endtab %}
{% endtabs %}
