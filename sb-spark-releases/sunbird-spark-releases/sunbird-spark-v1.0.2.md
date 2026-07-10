# Sunbird Spark v1.0.2

**Released:** 8 July 2026\
**Release type:** Minor — backward compatible with v1.0.1\
**Installer tag:** [`spark-v1.0.2`](https://github.com/Sunbird-Spark/sunbird-spark-installer/releases/tag/spark-v1.0.2)

{% hint style="info" %}
v1.0.2 upgrades cleanly from v1.0.1. Three items need attention during upgrade: a one-time database migration for the new QuML `pairs` column, a persistent-volume resize for the Yugabyte tserver, and one new environment variable. See the Upgrade Guide below.
{% endhint %}

***

### Overview

<table><thead><tr><th width="40">#</th><th>Major Feature</th><th>Summary</th></tr></thead><tbody><tr><td>1</td><td><strong>React-based Content Authoring Suite</strong></td><td><strong>Collection Editor, Generic/Upload Editor, QuML Editor and QuML Player</strong> rebuilt in React — faster, more reliable authoring with several longstanding bugs fixed</td></tr><tr><td>2</td><td><strong>New QuML Question Types</strong></td><td>Match the Following, Sequence, Reorder and Fill in the Blank, with partial scoring and order-independent evaluation</td></tr><tr><td>3</td><td><strong>On-demand Search Re-index Tool</strong></td><td>Operators can force a full or partial JanusGraph → OpenSearch re-index without redeploying the platform</td></tr><tr><td>4</td><td><strong>Security Hardening</strong></td><td>43 CVEs resolved in the platform core, dependency overhauls across all editors and players, Angular 19 → 21 upgrade</td></tr></tbody></table>

***

### 1. React-based Content Authoring Suite

The entire authoring experience — Collection Editor, Generic/Upload Editor, Questionset Editor and QuML Player — has been rebuilt in **React**, replacing the legacy Angular web components. The new editors are published as independent npm packages and wired into the Portal, giving creators a faster, more stable authoring workflow.

**Repositories:** [sunbird-collection-editor](https://github.com/Sunbird-Knowlg/sunbird-collection-editor) · [sunbird-generic-editor](https://github.com/Sunbird-Knowlg/sunbird-generic-editor) · [inQuiry editor](https://github.com/Sunbird-inQuiry/editor) · [inQuiry player](https://github.com/Sunbird-inQuiry/player)

#### 1.1 Collection Editor (React)

A ground-up React rewrite of the Collection Editor for creating courses and collections — covering outline management, library content, metadata forms, icon upload, dialcodes and collaborators, with several longstanding issues resolved along the way.

#### 1.2 Generic / Upload Editor V2 (React)

A new React-based editor for uploading and managing content (documents, videos, HTML and other file-based content), with a modern upload canvas, asset picker, metadata and review workflows, and built-in telemetry.

#### 1.3 QuML Editor (React) & QuML Player (React)

A new React-based questionset editor and QuML player, shipped alongside the existing Angular versions for a smooth transition — with richer question authoring, reliable editor-state restore, and improved playback and answer handling.

#### 1.4 Expanded Language & RTL Support

Arabic, French and Portuguese added across the editors and QuML player, with runtime language switching and full right-to-left (RTL) layout support.

***

### 2. New QuML Question Types

QuML 1.1 gains four new question types, giving quiz authors much richer assessment options:

* **Match the Following (MTF)** — drag-and-drop pairing.
* **Sequence / Ordered** — arrange items in the correct order.
* **Reorder** — rearrange a given list.
* **Fill in the Blank (FTB)** — free-text blanks within a passage.

Supporting capabilities:

* **Partial scoring** (`isPartialScore`) — an answer that is mostly right no longer scores the same as one that is entirely wrong.
* **Order-independent evaluation** (`evalUnordered`) — for questions where answer order shouldn't matter.
* Full **i18n support** for all new question types.
* New question-type definition scripts and QuML 1.1 schema updates in knowledge-platform.

{% hint style="info" %}
**Schema note:** version-check mode for questionset schema 1.1 is set to `OFF` in this release to ease migration to the new question types. Re-enable stricter version checks once your content authors have fully moved to schema 1.1.
{% endhint %}

***

### 3. On-demand Search Re-index Tool (JanusGraph → OpenSearch)

A new standalone sync tool lets operators force a **JanusGraph → OpenSearch re-index on demand** — useful after bulk content operations or to recover from search-index drift, without redeploying the platform.

* Packaged as an optional Helm subchart (`knowledgebb/charts/sync-tool`), run as a Kubernetes Job with an init container that waits for OpenSearch.
* **Five sync modes:** full, by object type, by identifier list, by date range (days), or from a CSV file.
* Correctly resolves relative content-storage paths to absolute cloud URLs before indexing.
* Deploy-workflow support: driven entirely via `global-values.yaml` + `specific_charts=sync-tool`.

{% hint style="info" %}
**Disabled by default** (`sync-tool.enabled: false`) — no effect on normal installs unless explicitly enabled.
{% endhint %}

***

### 4. Security Hardening

* **43 CVEs resolved** in the knowledge-platform core (Netty, cassandra-unit, embedded-kafka). (CVE-2024-35255).
* Full dependency and lockfile refresh for the **generic editor** (including CI Node.js version update) and the **content player**, with npm audit fixes across both.

***

### Bug Fixes

| Area               | Fix                                                                                               |
| ------------------ | ------------------------------------------------------------------------------------------------- |
| Knowledge Platform | HTTP 500 (`ERR_INVALID_EXISTS`) when retiring shallow-copied content                              |
| Knowledge Platform | Audio asset status not updated on upload                                                          |
| Knowledge Platform | HTML sanitizer stripping nested rich media (img/audio/video)                                      |
| QuML Player        | FTB `primaryCategory` handling; user answers lost across sections                                 |
| QuML Editor        | Editor state not restored for MTF/Sequence/Reorder; asset-creation payload (`contentType: Asset`) |

***

### Upgrade Guide

#### Upgrading from v1.0.1

Follow the steps below in order.

**Step 1 — Take a Database Backup**

This release adds a `pairs` column to the inquiry `question_data` table. Take a database backup before proceeding, as you would for any schema change.

**Step 2 — Check Storage Class Support for Volume Expansion**

The Yugabyte tserver persistent volume is resized from 10Gi to 25Gi.

> **Important:** confirm your storage class supports volume expansion before upgrading — an unsupported resize can fail mid-upgrade.

**Step 3 — Run the Installer**

```bash
git clone https://github.com/Sunbird-Spark/sunbird-spark-installer.git
cd sunbird-spark-installer
git checkout spark-v1.0.2
```

Follow the installer guide to complete infrastructure provisioning and service deployment.

**Step 4 — Run the Pending Data Migration**

Run the `pairs` column migration for the inquiry `question_data` table:

```bash
./execute_migrations.sh
```

The migration is idempotent — it checks the schema before applying, so it is safe to re-run.

**Step 5 — Set the New Environment Variable**

Add to `global-values.yaml` before deploying:

| Variable                     | Required | Purpose                                                            |
| ---------------------------- | -------- | ------------------------------------------------------------------ |
| `SUNBIRD_CLOUD_STORAGE_URLS` | Yes      | Cloud storage URL(s) used by the Portal for media/asset resolution |

**Step 6 — (Optional) Enable the Search Re-index Tool**

Only needed if you plan to use the on-demand re-index tool. In `global-values.yaml`:

```yaml
sync-tool:
  enabled: true
```

Then deploy with `specific_charts=sync-tool` (or select `knowledgebb` in the GitHub Actions deploy workflow).

Link to Release Tag: [https://github.com/Sunbird-Spark/sunbird-spark-installer/releases/tag/spark-v1.0.2](https://github.com/Sunbird-Spark/sunbird-spark-installer/releases/tag/spark-v1.0.2)

#### Notes

> **Schema version checks:** `versionCheckMode` for questionset schema 1.1 is `OFF` in this release to ease migration to the new question types. Plan to re-enable stricter checks once authors are fully on schema 1.1.

> **Kubernetes version upgrades:** unchanged from v1.0.1 — Kubernetes versions cannot be skipped. Upgrade one minor version at a time and plan your full path before proceeding.
