# Publish pipeline guide

This page documents the content publish pipeline in Spark, the simplifications made from Sunbird ED, and how to configure publish behaviour for your deployment.

#### What the publish pipeline does

When a content creator submits content for publish, the publish pipeline:

1. Validates the content metadata (required fields, taxonomy tags)
2. Processes media assets (transcoding, compression) if configured
3. Generates an ECAR package (content bundle for offline use) if ECAR generation is enabled
4. Generates adaptive bitrate streaming video (HLS) if video streaming is enabled
5. Updates the content status to Live in Janus Graph
6. Triggers the search-indexer to sync the published content to Elasticsearch

