# Tech Stack

#### **Frontend**

| Technology        | Version       | Purpose                                  |
| ----------------- | ------------- | ---------------------------------------- |
| React             | 19            | Web portal                               |
| Ionic / Capacitor | Latest stable | Mobile app (Android + iOS)               |
| Web Components    | V2            | Content players and editors (embeddable) |

#### **Backend**

| Technology     | Version | Purpose                                   |
| -------------- | ------- | ----------------------------------------- |
| Node.js        | 22 LTS  | BFF, middleware services                  |
| Play Framework | 3.0.5   | Core microservices (Scala)                |
| Apache Pekko   | 1.0.3   | Actor model for high-concurrency services |
| Scala          | 2.13.14 | Language for Play/Pekko services          |
| Java           | 11      | Language for Play/Pekko services          |

{% hint style="info" %}
**Akka → Pekko:** Spark migrates from Akka 2.5 to Apache Pekko 1.0.3. Pekko is a fully open-source fork of Akka with an identical API — no code changes are required in services that used Akka 2.5. This removes the Akka Business Source Licence dependency.
{% endhint %}

#### **Data stores**

| Technology       | Purpose                                                     | Notes                                                   |
| ---------------- | ----------------------------------------------------------- | ------------------------------------------------------- |
| YugabyteDB       | Primary transactional database                              | Replaces Cassandra + PostgreSQL. CQL + YSQL compatible. |
| Janus Graph      | Content graph — metadata and relationships                  | Replaces Neo4J                                          |
| Elasticsearch    | Search indices — content discovery and facets               | Synced from Janus Graph via Flink                       |
| Kafka            | Event streaming — telemetry, publish pipeline, certificates | Upgraded to v4.x                                        |
| Redis (optional) | Caching — API responses, sessions, telemetry dedup          | Two instances: metadata and knowledge platform          |

#### **Analytics (Obsrv add-on)**

| Technology      | Purpose                                           |
| --------------- | ------------------------------------------------- |
| Apache Druid    | OLAP database for time-series telemetry analytics |
| Apache Spark    | Batch summariser jobs                             |
| Apache Superset | Custom dashboard and report visualisation         |

#### **DevOps and infrastructure**

| Technology     | Version       | Purpose                                | Notes                         |
| -------------- | ------------- | -------------------------------------- | ----------------------------- |
| Kubernetes     | Latest stable | Container orchestration — all services | Fully native in Spark         |
| Helm           | v3            | Kubernetes package management          |                               |
| OpenTofu       | Latest stable | Infrastructure as Code                 | Replaces Terraform            |
| Grafana Alloy  | Latest stable | Log aggregation and shipping           | Replaces Promtail             |
| Grafana        | Latest stable | Metrics dashboards                     |                               |
| Prometheus     | Latest stable | Metrics collection                     |                               |
| GitHub Actions | —             | CI/CD pipelines                        | OIDC-based, no static secrets |
| Docker         | Latest stable | Container images                       |                               |

#### **Identity and security**

| Technology              | Purpose                                    | Notes                                                                                       |
| ----------------------- | ------------------------------------------ | ------------------------------------------------------------------------------------------- |
| Keycloak                | Identity provider, SSO, OAuth 2.0          | Now backed by YugabyteDB; OIDC protocol means it can be replaced by any OIDC-compatible IdP |
| Kong                    | API gateway — rate limiting, auth, routing | Upgraded                                                                                    |
| OPA (Open Policy Agent) | Declarative RBAC policy enforcement        |                                                                                             |
