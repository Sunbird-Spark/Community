# Simplifications in Spark



{% hint style="success" %}
New in Spark: The publish pipeline has been significantly simplified from Sunbird ED.&#x20;
{% endhint %}

| Aspect                  | Sunbird ED                                                                      | Sunbird Spark                              |
| ----------------------- | ------------------------------------------------------------------------------- | ------------------------------------------ |
| Number of Flink jobs    | Separate jobs for content publish, QuestionSet publish, post-publish processing | Single merged publish-pipeline job         |
| Asset enrichment        | Automatic — runs for every published content item                               | Removed entirely — not configurable        |
| Video streaming         | Enabled by default — HLS generated for every video                              | Disabled by default — configurable         |
| ECAR generation         | Enabled by default                                                              | Enabled by default — configurable          |
| Auto batch creation     | Triggered automatically on course publish                                       | Removed — batches must be created manually |
| Shallow copy publishing | Enabled                                                                         | Removed                                    |
