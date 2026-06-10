# Sunbird Spark v1.0.1

**Released:** 8 June 2026 \
**Release type:** Minor — backward compatible with v1.0.0 \
**Installer tag:** [`spark-v1.0.1`](https://github.com/Sunbird-Spark/sunbird-spark-installer/releases/tag/spark-v1.0.1)

{% hint style="info" %}
v1.0.1 is a drop-in upgrade from v1.0.0. No API breakage, no mandatory configuration changes. The Kubernetes version pin is handled automatically by the installer. The search index migration (Elasticsearch 7.10 → OpenSearch) requires following the migration guide below.
{% endhint %}

***

### What's in this release

| Capability                                               | What it does for you                                                                                                                |
| -------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| **Smarter search** _(OpenSearch replaces Elasticsearch)_ | Learners find the right content even when they don't know the exact title. Search index is migrated automatically by the installer. |
| **AI assistant integration (MCP)**                       | Connect Spark to Claude, custom agents or any MCP-compatible assistant — learners can browse, enrol and track progress through chat |
| **Custom theming**                                       | Change colours, fonts and branding without a deployment. Try it on the sandbox, customise your own using the published guide        |
| **Security hardening**                                   | Vulnerabilities in the platform core fixed, plus all findings from an independent penetration test on Knowlg and Portal resolved    |
| **Predictable Kubernetes upgrades**                      | No more surprise version bumps — cluster versions are now pinned and explicitly controlled                                          |
| **Automated tests for guest learner flows**              | Search, content discovery and player launch for unauthenticated users covered by regression tests                                   |

***

### Feature Details

#### Smarter search

{% hint style="warning" %}
**Infrastructure change — Elasticsearch replaced by OpenSearch.** This release migrates the search backend from Elasticsearch to OpenSearch. The installer handles index migration automatically (existing ES index is read, embeddings are generated, and the new vector index is populated). Adopters running self-managed or cloud-managed Elasticsearch clusters should review the upgrade notes before proceeding.
{% endhint %}

Search is now powered by **OpenSearch** with semantic matching layered on top of keyword search. The platform understands what a learner means, not just the words they type — a query like _"how do I handle errors in Python"_ surfaces relevant programming content even if none of it uses that exact phrase. Existing content becomes more discoverable without any re-tagging.

[High-level design document →](https://docs.google.com/document/d/1LtDnhFfW9VLbhX9pY4W9XbA8FNBh2uFhVEfNXojzE3g/edit)

***

#### AI assistant integration (MCP)

Spark now exposes its content catalogue, learning paths, enrolments and assessments through the **Model Context Protocol (MCP)**. Any MCP-compatible AI assistant — Claude, custom agents, or internal tools your organisation builds — can interact with the platform conversationally. Anonymous tools work without credentials; authenticated tools generate a login link for learners to sign in and seamlessly continue their AI-assisted learning experience.

[GitHub repository — sunbird-mcp →](https://github.com/Sunbird-Spark/sunbird-mcp)

***

#### Custom theming, live from the UI

Administrators can switch themes directly from the UI — no code, no deployment, no developer needed. Multi-tenant platforms can swap visual experiences per programme or audience, and branding for campaigns or partnerships can go live instantly.

**Try it:** the new theme is enabled on our sandbox so you can experience it end to end.

{% hint style="info" %}
**Sandbox:** [https://sandbox.sunbirded.org/](https://sandbox.sunbirded.org/)\
**Build your own theme** (colours, fonts, branding): [Portal theming guide →](https://github.com/Sunbird-Spark/sunbird-spark-portal#theming-system)
{% endhint %}

#### Security hardening

Two security workstreams completed this release.

**RC-Core vulnerability assessment.** A structured assessment was run across RC-Core. All identified critical and high-severity vulnerabilities have been remediated and re-validated in the latest scan.

**Shannon AI penetration test (Knowlg and Portal).** All findings from an externally conducted penetration test on Knowlg and Portal were reviewed and resolved in this sprint, with no accepted risk or deferred items.

Adopters subject to compliance requirements (ISO 27001, SOC 2, data protection regulations) can reference these workstreams in due diligence.

#### Predictable Kubernetes upgrades

{% hint style="info" %}
**First-time upgrade note:** If your current AKS cluster is not already on the pinned version, this upgrade will trigger a Kubernetes version change on both the control plane and worker node pools. Check the installer changelog for the exact pinned version before upgrading, and validate compatibility with any cluster-level customisations or extensions you have in place.
{% endhint %}

The AKS cluster is now configured with a **pinned Kubernetes version** instead of always defaulting to the latest available. Control plane and worker node pools upgrade to the same explicitly specified version, so platform upgrades no longer carry the risk of an unexpected Kubernetes version bump introducing breakage. Staging and production stay version-consistent.

#### Automated tests for guest learner flows

Automated regression tests now cover the **anonymous learner journey** — the first experience a new learner has with the platform. Coverage includes content discovery, search, course detail pages and player launch for unauthenticated users. Regressions in these flows are now caught before they reach production.

### Upgrade Guide

#### Upgrading from v1.0.0

No API changes are required. Follow the steps below in order.

#### Step 1 — Take an Elasticsearch Backup

Before doing anything else, take a full Elasticsearch backup.

[Elasticsearch backup guide →](https://github.com/Sunbird-Spark/sunbird-spark-installer/blob/main/helmcharts/learnbb/files/migration/README.md)

#### Step 2 — Check and Plan Your Kubernetes Version Upgrade

> **Important:** Kubernetes versions cannot be skipped. You must upgrade one minor version at a time. Check your current cluster version and plan the full upgrade path before proceeding.

[Kubernetes upgrade →](https://github.com/Sunbird-Spark/sunbird-spark-installer/blob/main/opentofu/azure/README.md#aks-kubernetes-version-upgrade)

#### Step 3 — Run the Installer

```bash
git clone https://github.com/Sunbird-Spark/sunbird-spark-installer.git
cd sunbird-spark-installer
git checkout spark-v1.0.1
```

Follow the [installer guide](../../use/getting-started/one-click-installer.md) to complete infrastructure provisioning and service deployment.

#### Step 4 — Run the OpenSearch Migration

Once all services are up, run the search index migration from Elasticsearch 7.10 to OpenSearch.

> **Reindexing required** if upgrading from a version that used Elasticsearch earlier than 7.10.

[OpenSearch Migration Guide →](https://github.com/Sunbird-Spark/sunbird-spark-installer/blob/main/helmcharts/learnbb/files/migration/README.md#step-2--elasticsearch-down-opensearch-up)

Link to Release Tag: [https://github.com/Sunbird-Spark/sunbird-spark-installer/releases/tag/spark-v1.0.1](https://github.com/Sunbird-Spark/sunbird-spark-installer/releases/tag/spark-v1.0.1)

#### JWT Key Regeneration

> **Required:** When upgrading from Spark v1.0.0, JWT keys must be regenerated.

1. Uncomment the relevant line in the keys module — find the path here →
2. Run the installer
3. Comment the line back in — leaving it uncommented will cause keys to be regenerated on every subsequent run

**Why this matters:** Kong acts as the API gateway — every request must carry a JWT with a `kid` (key ID) in the header. Kong uses that `kid` to look up the matching credential, verify the signature, identify the caller type (portal anonymous, portal authenticated, or mobile), and check route permissions.

#### Notes

> **Certificate keys on infrastructure reuse:** Certificate and certificate-signing keys are not regenerated when using the same infrastructure and only updating services. These keys are retained as-is in the existing `global-values.yaml` file. Reuse the existing certificate public key and certificate signing key from the old cluster. Do not regenerate them.

> **Kubernetes version upgrades:** Kubernetes versions cannot be skipped. You must upgrade one minor version at a time. Check your current cluster version and plan your full upgrade path before proceeding.
