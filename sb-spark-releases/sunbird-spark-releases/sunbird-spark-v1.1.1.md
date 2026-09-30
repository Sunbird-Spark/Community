# Sunbird Spark v1.1.1

**Released:** September 2026\
**Release type:** Patch — backward compatible with v1.1.0\
**Installer tag:** `spark-v1.1.1`

v1.1.1 upgrades cleanly from v1.1.0. This release delivers the technical design for an AI Pipeline Framework — generalising the AI capability introduced in v1.1.0 into reusable workflows with durable execution — brings the Portal, Mobile and DevOps repositories under continuous code-quality analysis with SonarQube Cloud, adds OpenSSF Scorecard supply-chain scoring to the DevOps repository, and extends the QTI proof of concept towards evaluation. Follow the Upgrade Guide below.

> **🔐 Open Source Security Posture:** With this release, Sunbird Spark publishes an **OpenSSF Scorecard** rating for its DevOps repository and enforces **SonarQube Cloud** quality gates across the Portal, Mobile and DevOps codebases — making the project's security and code-quality posture continuously measured and publicly visible.

#### Overview

<table><thead><tr><th width="54.8203125">#</th><th>Major Feature</th><th>Summary</th></tr></thead><tbody><tr><td>1</td><td>AI Pipeline Framework — Design &#x26; Enrichment Pipeline Refactor</td><td>A high-level technical design for building, running and reusing AI workflows — durable execution, a workflow registry, reusable capabilities, a generic RAG pipeline and Sunbird content enrichment — together with a modular refactor of the content embedding pipeline</td></tr><tr><td>2</td><td>Code Quality — SonarQube Cloud Integration</td><td>The Portal, Mobile and DevOps repositories are integrated with SonarQube Cloud, with continuous analysis on every pull request and Quality Gate A achieved on the release codebase</td></tr><tr><td>3</td><td>Supply-Chain Security — OpenSSF Scorecard</td><td>The DevOps repository is integrated with OpenSSF Scorecard, publishing a continuously updated supply-chain security score via a GitHub Actions workflow and a README badge</td></tr><tr><td>4</td><td>QTI Support — Player Proof of Concept for Evaluation</td><td>The QTI Player proof of concept is extended to support question consumption for assessments, ready for functional evaluation ahead of a production implementation</td></tr></tbody></table>

### 1. AI Pipeline Framework — Design & Enrichment Pipeline Refactor

AI features rarely involve a single model call — turning a video into learning material can require transcription, summarisation, translation, review and publishing. These tasks are long-running, can fail partway, and can receive the same request twice. v1.1.1 delivers the **technical design for an AI Pipeline Framework** that gives teams a common way to run such workflows, track progress and reuse work across features.

**1.1 Framework Design** The framework is designed as four layers — **Access** (workflow, query and management APIs), **Registry** (a catalogue of workflow definitions with their schemas, configuration and triggers), **Execution** (durable execution that records progress, runs steps in parallel and waits for human review), and **Shared services** (model calls, embeddings, vector search, storage and business-API adapters) — with observability across all of them for traces, evaluations, latency and cost.

Workflows are triggered by Kafka events, REST APIs or schedules, and compose a shared capability catalogue: OCR, speech-to-text, text-to-speech, summarisation, translation, metadata and tags, quiz generation and embeddings. The design also covers a **generic RAG pipeline** (separate indexing and query workflows, with tenant and version-aware retrieval) and **Sunbird content enrichment** via a reusable Sunbird API adapter with optional reviewer approval.

### 2. Code Quality — SonarQube Cloud Integration

The Portal, Mobile and DevOps repositories are integrated with **SonarQube Cloud** under the `sunbird-spark` organisation, so every pull request is analysed before merge.

**2.1 Portal & Mobile** Reliability, Security and Maintainability ratings brought to **A**, with unit-test coverage reported to SonarCloud for the first time. Fixes included crypto-backed random and identifier generation, validating URL parameters before persisting them to browser storage, Content Security Policy and CORS restrictions, hardened SVG sanitisation, denial of cleartext traffic, and keyboard accessibility on interactive elements.

**2.2 DevOps (Installer)** Onboarded with pull-request analysis, so every change is gated on **new-code** quality. Remediation of pre-existing findings in the vendored third-party assets bundled in this repository is tracked separately.

**2.3 CI/CD Hardening** Across all three repositories: GitHub Actions pinned to full commit SHAs, workflow permissions scoped to the minimum required, lockfile-pinned script-free dependency installs, and lockfiles tracked for reproducible builds.

### 3. Supply-Chain Security — OpenSSF Scorecard

The DevOps repository is integrated with the **OpenSSF Scorecard**, which continuously evaluates supply-chain practices — branch protection, pinned dependencies, workflow permissions, code review and vulnerability disclosure among them. A scheduled GitHub Actions workflow publishes results, and the score appears as a badge in the repository README.

The repository currently scores **8.6 / 10**.

### 4. QTI Support — Player Proof of Concept for Evaluation

Building on the QTI plan and design completed in v1.1.0, the **QTI Player proof of concept** is extended to demonstrate **question consumption for assessments** — rendering QTI-authored questions and capturing learner responses.

The proof of concept is delivered for **functional evaluation**, to validate the approach before committing to a production implementation.

> **Note:** QTI support remains a proof of concept in this release. It is not enabled in the product and is not intended for production use.

### Bug Fixes

<table><thead><tr><th width="171.43359375">Area</th><th>Fix</th></tr></thead><tbody><tr><td>DevOps / Security</td><td>Secrets moved out of ConfigMaps into Kubernetes Secrets across services, including Flink job configuration</td></tr><tr><td>DevOps / Security</td><td>TLS private key for the public domain moved out of a plain ConfigMap</td></tr><tr><td>DevOps</td><td>Flink LMS base URL configuration corrected</td></tr><tr><td>DevOps</td><td>Migration container CPU and memory limits right-sized</td></tr><tr><td>Portal</td><td>Privacy policy and terms-and-conditions content updated</td></tr><tr><td>Portal / Mobile</td><td>Workflow permissions set explicitly rather than inherited, across CI pipelines</td></tr></tbody></table>

### Upgrade Guide

**Upgrading from v1.1.0**

v1.1.1 is a patch release and upgrades in place from **v1.1.0**. There are no schema or infrastructure migrations in this release.

**Step 1 — Deploy**\
Deploy the services as per the installer guide using the `spark-v1.1.1` tag.

**Link to Release Tag:** [spark-v1.1.1](https://github.com/Sunbird-Spark/sunbird-spark-installer/releases/tag/spark-v1.1.1)
