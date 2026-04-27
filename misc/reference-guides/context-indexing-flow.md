# Context indexing flow

This page explains how content created in Spark becomes discoverable via search.

#### The flow — step by step

```
1. Content created/updated via API
         │
         ▼
2. knowlg-service writes metadata to Janus Graph
         │
         ▼
3. Neo4j kernel extension detects the write
   Appends event to the change log file:
   /opt/bitnami/janusgraph/logs/cdc-events.log
         │
         ▼
4. Logstash service reads the log file
   Publishes events to Kafka topic:
   learning.graph.events
         │
         ▼
5. Flink search-indexer job consumes the topic
   Creates or updates the document in Elasticsearch
   (compositesearch index)
         │
         ▼
6. Content is now discoverable via
   /api/content/v1/search

```

#### Search indexer configuration

The search indexer's behaviour is controlled by its configuration file. \
\
**Key settings:**

* **nested.fields** — defines which metadata fields are stored as Elasticsearch nested objects (affects how they can be queried)
* **restrict.objectTypes** — object types listed here are excluded from the Elasticsearch index. By default, internal node types (License, Framework, Channel) are excluded — only content visible to learners is indexed.

#### Re-indexing

If the Elasticsearch index becomes out of sync with Janus Graph (e.g., after an index rebuild or a Flink job failure), a full re-index can be triggered:

```
# Trigger a full Janus Graph  → Elasticsearch sync
kubectl exec -it <knowlg-service-pod> -n default -- \
curl -X POST http://localhost:9000/v1/index/sync
```

A full re-index on a large deployment (millions of content items) can take several hours. The alias architecture (compositesearch → actual index) means search continues to work during re-indexing — the alias is switched to the new index atomically when the re-index is complete.

<br>

