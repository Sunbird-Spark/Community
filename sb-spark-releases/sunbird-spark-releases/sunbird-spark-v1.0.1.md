---
description: >-
  This minor release builds on Spark v1.0.0 with the addition of Semantic/AI
  search, MCP tool support, and custom theme options, along with security fixes
  and operational improvements.
---

# Sunbird Spark v1.0.1

Here's the full file:

***

### Sunbird Spark v1.0.1 — Release Notes

**Released:** 8 June 2026 **Release type:** Minor — backward compatible with v1.0.0 **Installer tag:** `spark-v1.0.1`

v1.0.1 is a drop-in upgrade from v1.0.0. No API breakage, no mandatory configuration changes. The Kubernetes version pin is handled automatically by the installer. The search index migration (Elasticsearch 7.10 → OpenSearch) requires following the migration guide below.

***

#### What's New

| Feature                        | Summary                                                                          |
| ------------------------------ | -------------------------------------------------------------------------------- |
| Semantic Search                | OpenSearch replaces Elasticsearch; understands learner intent, not just keywords |
| MCP Tool Support               | Connect Claude, custom agents, or any MCP-compatible assistant to Spark          |
| Custom Theming                 | Change colours, fonts, and branding from the UI — no deployment required         |
| Security Hardening             | RC-Core vulnerabilities fixed; all penetration test findings resolved            |
| Kubernetes Version Pinning     | AKS cluster version is now explicitly pinned, preventing unexpected upgrades     |
| Guest Learner Regression Tests | Automated coverage for the full anonymous learner journey                        |

***

#### Feature Details

**Semantic Search (OpenSearch)**

Spark's search backend has migrated from Elasticsearch to OpenSearch, with semantic matching layered on top of keyword search. The platform now understands learner intent rather than matching exact keywords. A query like _"how do I handle errors in Python"_ surfaces relevant programming content even when none of the content uses that exact phrase. Existing content becomes more discoverable without any re-tagging.

> **Note:** Adopters running self-managed or cloud-managed Elasticsearch clusters should review the migration guide before proceeding.

High-level design document →

***

**AI Assistant Integration (MCP)**

Spark now exposes its content catalogue, learning paths, enrolments, and assessments through the **Model Context Protocol (MCP)**. Any MCP-compatible assistant — Claude, custom agents, or internal tools your organisation builds — can interact with the platform conversationally.

* **Anonymous tools** work without credentials
* **Authenticated tools** generate a login link so learners can sign in and seamlessly continue their AI-assisted learning session

[GitHub: sunbird-mcp →](https://github.com/Sunbird-Spark/sunbird-mcp)

***

**Custom Theming**

Administrators can switch themes directly from the portal UI — no code, no deployment, no developer required. Multi-tenant platforms can apply different visual experiences per programme or audience, and campaign or partnership branding can go live instantly.

Try it live on the sandbox: [sandbox.sunbirded.org](https://sandbox.sunbirded.org/)

Portal theming guide →

***

**Security Hardening**

Two security workstreams were completed in this release.

**1. RC-Core Vulnerability Assessment**

A structured assessment was run across RC-Core. All identified critical and high-severity vulnerabilities have been remediated and re-validated in the latest scan.

**2. Shannon AI Penetration Test — Knowlg and Portal**

All findings from an externally conducted penetration test on Knowlg and Portal were reviewed and resolved. There are no accepted risks or deferred items.

> Adopters subject to compliance requirements (ISO 27001, SOC 2, data protection regulations) can reference these workstreams in due-diligence documentation.

***

**Kubernetes Version Pinning**

The AKS cluster is now configured with a pinned Kubernetes version instead of always defaulting to the latest available. Control plane and worker node pools upgrade to the same explicitly specified version, ensuring staging and production remain version-consistent.

> **Note:** Kubernetes versions cannot be skipped. You must upgrade one minor version at a time. If your current AKS cluster is not already on the pinned version, plan your upgrade path incrementally before proceeding. Validate compatibility with any cluster-level customisations or extensions you have in place.

***

**Automated Tests — Guest Learner Flows**

Automated regression tests now cover the full anonymous learner journey. Coverage includes:

* Content discovery
* Search
* Course detail pages
* Player launch for unauthenticated users

Regressions in these flows are caught before they reach production.

***

#### Upgrade Guide

**Upgrading from v1.0.0**

No API changes are required. Follow the steps below in order.

***

**Step 1 — Take an Elasticsearch Backup**

Before doing anything else, take a full Elasticsearch backup.

[Elasticsearch backup guide →](https://github.com/Sunbird-Spark/sunbird-spark-installer/blob/main/helmcharts/learnbb/files/migration/README.md)

***

**Step 2 — Check and Plan Your Kubernetes Version Upgrade**

> **Important:** Kubernetes versions cannot be skipped. You must upgrade one minor version at a time. Check your current cluster version and plan the full upgrade path before proceeding.

[Kubernetes upgrade](https://github.com/Sunbird-Spark/sunbird-spark-installer/blob/main/opentofu/azure/README.md#aks-kubernetes-version-upgrade)  →

***

**Step 3 —**&#x52;un the Installer

bash

```bash
git clone https://github.com/Sunbird-Spark/sunbird-spark-installer.git
cd sunbird-spark-installer
git checkout spark-v1.0.1
```

Follow the installer guide to complete infrastructure provisioning and service deploymentfollow this link to how to do [sunbird-spark-installer-guide](../../use/getting-started/one-click-installer.md)

***

**Step 4 — Run the OpenSearch Migration**

Once all services are up, run the search index migration from Elasticsearch 7.10 to OpenSearch.

> **Reindexing required** if upgrading from a version that used Elasticsearch earlier than 7.10.

[OpenSearch Migration Guide →](https://github.com/Sunbird-Spark/sunbird-spark-installer/blob/main/helmcharts/learnbb/files/migration/README.md#step-2--elasticsearch-down-opensearch-up)

Link to Release Tag: [https://github.com/Sunbird-Spark/sunbird-spark-installer/releases/tag/spark-v1.0.1](https://github.com/Sunbird-Spark/sunbird-spark-installer/releases/tag/spark-v1.0.1)

***

**JWT Key Regeneration**

> **Required:** When upgrading from Spark v1.0.0, JWT keys must be regenerated.

1. Uncomment the relevant line in the keys module — find the path here →
2. Run the installer
3. Comment the line back in — leaving it uncommented will cause keys to be regenerated on every subsequent run

**Why this matters:** Kong acts as the API gateway — every request must carry a JWT with a `kid` (key ID) in the header. Kong uses that `kid` to look up the matching credential, verify the signature, identify the caller type (portal anonymous, portal authenticated, or mobile), and check route permissions.

**Notes**

> **Certificate keys on infrastructure reuse:** Certificate and certificate-signing keys are not regenerated when using the same infrastructure and only updating services. These keys are retained as-is in the existing `global-values.yaml` file. Reuse the existing certificate public key and certificate signing key from the old cluster. Do not regenerate them.

> **Kubernetes version upgrades:** Kubernetes versions cannot be skipped. You must upgrade one minor version at a time. Check your current cluster version and plan your full upgrade path before proceeding.
