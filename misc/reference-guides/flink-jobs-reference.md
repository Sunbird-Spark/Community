# Flink jobs reference

Spark uses Apache Flink for stream processing — event-driven jobs that process Kafka events in real time. This page documents the Flink jobs that are active in a default Spark installation.

#### Namespaces

Flink jobs run in two Kubernetes namespaces:

* kp-flink-\<env> — jobs related to content knowledge management (publishing, indexing)
* flink-\<env> — jobs related to learning and data pipeline (certificates, telemetry)

### Active Jobs in Spark

{% tabs %}
{% tab title="kp-flink namespace (knowledge platform)" %}
| publish-pipeline      | Content publish API               | Processes content after it is submitted for publish. Generates ECAR packages (if enabled), processes media, and marks content as Live. Also handles QuestionSet publish — content and QuestionSet publish are merged into a single job in Spark. |
| --------------------- | --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| search-indexer        | learning.graph.events Kafka topic | Syncs content metadata changes from JanusGraph to Elasticsearch.                                                                                                                                                                                 |
| audit-event-processor | Content lifecycle events          | Processes content audit events for governance tracking.                                                                                                                                                                                          |
{% endtab %}

{% tab title="flink namespace (learning and data pipeline)" %}
| collection-certificate-generator | issue.certificate.request Kafka topic | Evaluates learner completion against batch certificate rules. Generates SVG certificates and stores them in Sunbird RC. This job is a merger of the old collection-cert-pre-processor and collection-certificate-generator from Sunbird ED. |
| -------------------------------- | ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| user-cache-updater-v2            | Scheduled                             | Populates the Redis user cache used by data pipeline jobs for telemetry denormalisation (adding user metadata to raw telemetry events).                                                                                                     |
| telemetry-extractor              | telemetry.raw Kafka topic             | Extracts individual telemetry events from batch payloads submitted by client apps.                                                                                                                                                          |
| pipeline-preprocessor            | Extracted telemetry events            | Deduplicates events (checks Redis for UUID), validates events against the telemetry specification, and routes valid events forward.                                                                                                         |
| de-norm                          | Validated telemetry events            | Enriches events with denormalised metadata (user name, course name, content title) by looking up the Redis user and content caches.                                                                                                         |
| druid-validator                  | Denormalised events                   | Final validation before events are written to Druid. Only active when Obsrv add-on is enabled.                                                                                                                                              |
{% endtab %}
{% endtabs %}

#### Jobs removed from Spark

The following jobs from Sunbird ED have been removed. See the [ED → Spark Migration Guide](../../use/ed-greater-than-spark-migration/flink-jobs-removed-in-spark.md) for the full list and replacements.

<br>
