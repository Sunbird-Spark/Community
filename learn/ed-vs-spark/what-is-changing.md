# What is changing

### Functional / User Perspective



* **Redesigned homepage** — clean hero banner, curated content shelves (Most Popular, Trending, Resource Centre), and category browsing, all accessible without logging in. The design, look, and feel are fully customisable per tenant.
* **Personalised home dashboard** — greets you by name and displays Total Courses, In Progress, Completed, and Certifications Earned at a glance.
* **Continue where you left off** — one-click resume button for your last accessed course, surfaced directly on the home screen.
* **Recommended content** — personalised suggestions based on your learning history, shown on your home feed.
* **Collapsible left sidebar navigation** — replaces the previous top navigation bar; role-aware, so you only see items relevant to your assigned roles.
* **Language switcher always visible** — switch language from any page, including before logging in.
* **My Learning page** — Active, Completed, and Upcoming tabs with visual progress bars and a Learning Progress widget showing lessons visited and content completed.
* **Explore page with persistent filters** — filter by Collections, Categories, and Content Types from a left panel without losing your position in the results.
* **Personal User Report page** — your own analytics: Courses Completed, Certificates Issued, Assessments Done, full progress history, and Export CSV.
* **Built-in Admin Reports dashboard** — content status charts, top contributors, course summary table with colour-coded completion percentages, and user growth trends — no external analytics infrastructure required.
* **Profile page** — personal information, role badges, learning statistics, and a course progress list with status filters, all in one place.
* **Certificate ID and issue date** visible directly in your User Report — no longer buried in a separate passbook view.
* **Help & Support** in the sidebar — one click away from any page, with an FAQ section on the homepage.
* **The following features are removed from the default package** but remain available as opt-in add-ons: QR code content discovery, Energised Textbooks, Discussion Groups, Video Stream generation, and Obsrv-based reporting.

***

#### Technical Changes

**1. Database Consolidation**

{% hint style="info" %}
**Before:** Multiple databases — Cassandra (Lern), Janus Graph (Knowlg), PostgreSQL (Keycloak/Kong), Redis, and Elasticsearch running separately on VMs.\
\
**After:** YugabyteDB replaces Cassandra as the single transactional database across all building blocks.
{% endhint %}

| Migration                                       | Status |
| ----------------------------------------------- | :----: |
| Lern BB (User, Org, LMS) → YugabyteDB           | ✅ Done |
| Knowlg BB (knowlg-service, search) → YugabyteDB | ✅ Done |
| inQuiry → YugabyteDB                            | ✅ Done |
| Keycloak → YugabyteDB                           | ✅ Done |
| Sunbird RC → YugabyteDB                         | ✅ Done |

{% hint style="info" %}
**Why YugabyteDB?** It is Cassandra-compatible (CQL) and PostgreSQL-compatible (YSQL), distributed, and cloud-native — collapsing Cassandra and PostgreSQL into a single managed cluster and significantly reducing infrastructure footprint and cost.
{% endhint %}

***

**2. API Microservices Consolidation**

{% hint style="info" %}
**Before:** Separate services — `taxonomy-service`, `content-service`, `knowlg-service`, `search-service`, `userorg-service`, `lms-service` — all running independently.\
\
**After:** Consolidated into two unified services.
{% endhint %}

| Before                               | After                        | Status |
| ------------------------------------ | ---------------------------- | :----: |
| taxonomy + content + knowlg + search | `knowlg-service`             | ✅ Done |
| inQuiry APIs                         | Merged into `knowlg-service` | ✅ Done |
| userorg-service + lms-service        | `lern-service`               | ✅ Done |

***

**3. Flink Jobs Consolidation**

{% hint style="info" %}
**Before:** Dozens of Flink jobs, many enabled by default.\
\
**After:** Significant pruning — jobs merged or removed entirely.
{% endhint %}

**Merged:**

* `content-publish` + `questionset-publish` → one job
* `collection-cert-pre-processor` + `collection-certificate-generator` → one job

**Removed (dead code cleaned up):**

* `questionset-republish`, `audit-event-generator`, `audit-history-indexer`
* `auto-creator-v2`, `content-auto-creator`, `csp-migrator`
* `live-video-stream-generator`, `mvc-indexer`
* `metrics-data-transformer`, `user-cache-updater`
* `merge-user-courses`, `enrolment-reconciliation`
* Response Exhaust, User Info Exhaust, Cassandra Migration jobs
* Collection Summary, State Admin Geo/Consent Report jobs

**Refactored from Flink jobs to API calls:**

* `relation-cache-updater`
* `activity-aggregator`
* `assessment-aggregator`

***

**4. Add-on Architecture**

Features previously enabled by default are now opt-in add-ons, disabled in the Spark installer unless explicitly configured.

| Feature                         | Sunbird ED | Spark                      |
| ------------------------------- | ---------- | -------------------------- |
| DIAL / QR Code                  | Default ON | ✅ Add-on (done Feb)        |
| Discussion Forum (NodeBB)       | Default ON | Add-on                     |
| Groups                          | Default ON | Add-on                     |
| Obsrv (full analytics pipeline) | Default ON | Add-on (March)             |
| Managed Users                   | Default ON | Add-on (AMJ)               |
| Video streaming generation      | Default ON | ✅ Config flag, default OFF |
| Asset Enrichment job            | Default ON | ✅ Removed entirely         |
| Auto Batch Creation             | Default ON | ✅ Removed                  |
| Shallow Copy Publishing         | Default ON | ✅ Removed                  |

{% hint style="info" %}
A lean Spark installation ships with approximately 60% fewer running services than Sunbird ED 8.x.
{% endhint %}

***

**5. Consumption Apps — Portal and Mobile Rebuilt**

{% hint style="info" %}
**Before:** Angular 14 portal + Ionic/Cordova mobile app.\
\
**After:** Completely new builds.
{% endhint %}

| Aspect            | Before          | After                                      |
| ----------------- | --------------- | ------------------------------------------ |
| Web framework     | Angular 14      | React 19 ✅                                 |
| Mobile framework  | Ionic / Cordova | Ionic 8 / Capacitor ✅                      |
| UI generation     | Manual          | AI-assisted screen generation              |
| Theming           | Limited         | Multi-theme, domain-specific templates     |
| Offline (mobile)  | Present         | Content downloadable and playable offline  |
| NL web interface  | Not present     | In scope (March 2026)                      |
| UI design process | Ad-hoc          | UX-first with design sign-off before build |

***

**6. Tech Stack Changes & Upgrades**

| Component         | Before             | After                         |
| ----------------- | ------------------ | ----------------------------- |
| Scala             | 2.11 / 2.12        | 2.13.14 ✅                     |
| Node.js           | 12 / 14            | 24 LTS ✅                      |
| Web framework     | Angular 14         | React 19 ✅                    |
| NodeBB (forums)   | 3.x                | 4.x ✅                         |
| Kafka             | 2.x / 3.x          | 4.x ✅                         |
| Akka              | 2.5.x              | Apache Pekko 1.0.3 ✅          |
| Play Framework    | 2.4–2.8            | 3.0.5 ✅                       |
| Log aggregation   | Promtail           | Grafana Alloy ✅               |
| IaC               | Terraform          | OpenTofu ✅                    |
| Cloud auth        | Static access keys | OIDC ✅                        |
| Cloud Storage SDK | Monolithic         | Lightweight, per-cloud OIDC ✅ |

***

**7. Permanent Removals**

| Item                            | Reason                                                        |
| ------------------------------- | ------------------------------------------------------------- |
| Print Service                   | PDF generation via Question Paper feature removed             |
| Obsrv v1.x Helm charts          | Replaced by the add-on model                                  |
| Report Service (legacy)         | Replaced by native built-in portal reports                    |
| Textbook deletion capability    | Removed                                                       |
| Legacy QuML assessment approach | Only the QuML spec is retained; legacy approach dropped       |
| Obsrv as default dependency     | Decoupled; basic reports now built natively into the platform |

***

**8. Publish Pipeline Simplification**

> **Before:** \
> Heavy async publish pipeline with multiple parallel jobs — ECAR generation, video streaming, and asset enrichment all running by default.

> **After:**
>
> * Multiple publish jobs merged into a single unified job
> * ECAR generation is configurable (can be disabled)
> * Video streaming disabled by default
> * Asset enrichment removed entirely

***

**9. Obsrv Decoupled — Basic Reports Built In**

> **Before:** \
> Obsrv (Druid + Flink + Spark + Superset) was a mandatory dependency for any reporting capability.

> **After:**
>
> * Obsrv becomes an optional add-on DPG
> * Platform ships with native report APIs integrated directly into the portal UI
> * Telemetry service is retained by default (events are still emitted)
> * Kafka topic backup retained so Obsrv can be connected later
> * Druid, Spark summarisers, and Superset are all disabled by default

