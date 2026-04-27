# Database usage map

Spark uses multiple specialised databases, each chosen for the characteristics of the data it stores. This page documents what each database stores and why.

{% tabs %}
{% tab title="YugabyteDB " %}
**Primary transactional database**

{% hint style="success" %}
New in Spark: YugabyteDB replaces both Cassandra and PostgreSQL from Sunbird ED. It is CQL-compatible (Cassandra Query Language) and YSQL-compatible (PostgreSQL syntax), allowing the same database cluster to serve both use cases.&#x20;
{% endhint %}

YugabyteDB stores all transactional data across all building blocks:

**Sunbird Lern (lern-service):**

* user — user profiles, preferences, and account status
* user\_org — user-to-organisation mappings and roles
* organisation — organisation records and channel metadata
* course\_batch — course batch definitions and metadata
* user\_enrolments — learner enrolment records per batch
* user\_content\_consumption — lesson-level progress per learner per batch
* user\_activity\_agg — aggregated course-level completion per learner
* cert\_registry — certificate issuance records

**Sunbird Knowlg (knowlg-service):**

* content\_data — content body and hierarchy (large content payloads)
* framework — taxonomy framework definitions
* form\_data — Form Service UI configuration JSON blobs

**Keycloak:**

* All Keycloak identity data (users, credentials, realms, clients, sessions)

**Sunbird RC:**

* Verifiable credential records and registry entries
{% endtab %}

{% tab title="JanusGraph" %}
JanusGraph (backed by YugabyteDB) stores the content metadata graph — the relationships between content objects, collections, frameworks, and taxonomy nodes.

**What JanusGraph Stores**

* **Content node properties** — title, description, status, language, content type, and all metadata fields (excluding the content body)
* **Collection hierarchy relationships** — which lessons belong to which course, and in what order
* **Framework nodes and taxonomy relationships** — Board → Medium → Grade → Subject
* **DIAL code to content mappings** — when the DIAL add-on is enabled

**Why JanusGraph**

Graph traversal is used to efficiently navigate collection hierarchies — for example, "return all lessons in this course, in order, with their metadata." This traversal is expensive in a relational database and fast in a graph database. JanusGraph provides the graph layer; YugabyteDB serves as the persistent storage backend, replacing the need for a separate Cassandra or HBase cluster.

**Write Path**

Content create / update / publish API calls write to JanusGraph first. JanusGraph then syncs to Elasticsearch via Logstash and the `search-indexer` Flink job.
{% endtab %}

{% tab title="ElasticSearch" %}
Elasticsearch stores materialised read-model views of content from Janus Graph, optimised for fast full-text search and faceted filtering.

**Index**: compositesearch (an alias to the active index — this alias approach allows zero-downtime index rebuilds by pointing the alias to a new index)

**What Elasticsearch stores**:

* A flattened, searchable copy of all content and collection metadata from Neo4j
* Updated in near-real-time via the Flink search-indexer job
* Used exclusively for read operations — never for writes

**Why Elasticsearch**: Faceted search (filter by grade AND subject AND content type simultaneously) is a native Elasticsearch capability and is complex to replicate in a graph database.
{% endtab %}

{% tab title="Redis(Optional)" %}
Redis provides high-velocity in-memory caching. Two instances are used:

**Metadata Redis (DB 12)**

* **API response cache** — frequently accessed content metadata responses
* **User session cache** — session tokens and user preference data (used by the portal backend / BFF)
* **Data pipeline user cache** — user metadata cached for Flink data pipeline jobs (denormalisation)

**Knowledge Platform Redis**

* **Content hierarchy cache** — cached collection hierarchies to avoid repeated JanusGraph queries on hot content
{% endtab %}

{% tab title="Kafka" %}
Kafka is the backbone of Spark's asynchronous event-driven architecture. It decouples producers (services that generate events) from consumers (Flink jobs and other services that process events).

**Key Kafka topics:**

| Topic                     | Producer                         | Consumer                        | Purpose                                        |
| ------------------------- | -------------------------------- | ------------------------------- | ---------------------------------------------- |
| telemetry.raw             | Portal, mobile app, desktop app  | Flink telemetry pipeline        | Raw telemetry events from learner interactions |
| learning.graph.events     | Logstash (from Neo4j change log) | Flink search-indexer            | Content change events for Elasticsearch sync   |
| coursebatch.job.request   | lern-service                     | Activity aggregator             | Course progress update events                  |
| issue.certificate.request | Activity aggregator              | Certificate generator Flink job | Certificate issuance trigger events            |
| telemetry.druid           | Flink telemetry pipeline         | Druid (Obsrv add-on)            | Processed telemetry events for OLAP ingestion  |
{% endtab %}

{% tab title="Druid" %}
#### OLAP analytics (Obsrv add-on)

Druid stores processed, denormalised telemetry data for fast analytical queries. It is only active when the Obsrv add-on is enabled.

**What Druid stores:**

* Raw telemetry events (after deduplication, validation, and enrichment by Flink)
* Summarised session data (computed by Apache Spark summariser jobs)
* Pre-aggregated metrics for dashboard rendering

**Why Druid:** Druid is optimised for time-series aggregation queries (e.g., "how many learners started Course X per day over the last 30 days"). These queries are prohibitively slow in a transactional database at scale.
{% endtab %}
{% endtabs %}
