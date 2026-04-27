---
description: System requirements, one-click installer, and enabling add-ons for a Sunbird Spark deployment.
---

# Getting Started

### Overview

From Spark v1.0, Sunbird Spark uses a fully Kubernetes-native architecture. Every service — including databases (YugabyteDB, Elasticsearch, Redis), messaging (Kafka), and stream processing (Flink) — runs on Kubernetes. There are no VM-based dependencies.

{% hint style="success" %}
&#x20;New in Spark: Previous Sunbird ED deployments used a hybrid model where databases and some services ran on VMs, with microservices on Kubernetes. Spark eliminates this split — everything runs on Kubernetes, managed by Helm charts. This makes deployments more consistent, easier to scale, and simpler to operate.
{% endhint %}

This means:

* You need a Kubernetes cluster — any provider works
* All services are deployed via the one-click installer using Helm charts
* Scaling is done by adjusting node counts and pod replica counts in your cluster
* Backups, upgrades, and rollbacks are managed via Helm and Kubernetes-native tooling

***
