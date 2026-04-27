# Local Development Guide

{% hint style="success" %}
Local development setup for these projects is simple and quick — each repository's README walks you through the entire process step by step.
{% endhint %}

### Prerequisites

* Docker Desktop (latest stable)
* Docker Compose v2
* Java 11
* Maven 3.9+
* Git

***

### Getting Started

Each repository includes a **README** with detailed setup instructions. To get started with any project:

1. Clone the repository
2. Follow the step-by-step instructions in the README

The READMEs cover everything from starting infrastructure to building and running the project locally.

***

### Knowledge Platform

Knowledge Platform is the core backend that powers content management, taxonomy, assessments, and search across the Sunbird Knowlg ecosystem. Supports building both a merged service and individual microservices.

#### What's included

* **Content API** — Create, manage, and publish content and collections
* **Taxonomy API** — Manage frameworks, categories, terms, channels, and licenses
* **Assessment API** — Create and manage question sets and assessment items
* **Search API** — Composite search across the knowledge graph via Elasticsearch
* **Knowlg Service** — A single runtime that bundles Content, Taxonomy, and Assessment APIs together

**Repository:** [https://github.com/Sunbird-Knowlg/knowledge-platform](https://github.com/Sunbird-Knowlg/knowledge-platform)

***

### Knowledge Platform Jobs

Apache Flink stream processing jobs for the Sunbird Knowledge Platform. Each job consumes events from Kafka, processes content lifecycle operations, and writes results to YugabyteDB, JanusGraph, and Elasticsearch.

#### What's included

* **Knowlg Publish** — Publishes content, collections, and assets
* **Transaction Event Processor** — Generates audit events and syncs data to Elasticsearch for every JanusGraph transaction
* **QR Code Image Generator** — Generates QR code images for dial codes
* **Dialcode Context Updater** — Updates dial code context in the graph

**Repository:** [https://github.com/Sunbird-Knowlg/knowledge-platform-jobs](https://github.com/Sunbird-Knowlg/knowledge-platform-jobs)

***

### Lern Service

A unified platform component that integrates core learning functionalities, user organization management, and notification services into a single deployable unit. Supports building both a merged service and individual microservices.

#### **What's included:**

* **Lern Service (Merged)** — A single service combining all functionalities below
* **LMS Service** — Learning Management System capabilities
* **UserOrg Service** — User and Organization management
* **Notification Service** — System notifications and alerts

**Repository:** [https://github.com/Sunbird-Lern/lern-service](https://github.com/Sunbird-Lern/lern-service)

***

### Data Pipeline

Flink-based data processing jobs that handle asynchronous backend tasks such as certificate generation, notification delivery, user data management, and activity aggregation.

#### What's included

* **Certificate Generator** — Generates and issues certificates for course completions
* **Notification Job** — Processes and delivers notifications

**Repository:** [https://github.com/Sunbird-Lern/data-pipeline](https://github.com/Sunbird-Lern/data-pipeline)
