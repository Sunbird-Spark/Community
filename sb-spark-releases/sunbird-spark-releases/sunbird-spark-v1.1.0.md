# Sunbird Spark v1.1.0

**Released:** 17 August 2026 \
**Release type:** Minor — backward compatible with v1.0.2 \
**Installer tag:** `spark-v1.1.0`

v1.1.0 upgrades cleanly from v1.0.2. This release delivers AI-powered captioning and video transcripts across the Sunbird Video Player (web and mobile), introduces Viewer Service backend APIs for Learning Paths, hardens service images with Docker Hardened Images (DHI), and resolves a set of authoring and assessment defects. Follow the Upgrade Guide below.

> **🚀 DPG AI Infrastructure:** With this release, Sunbird Spark becomes a Digital Public Good (DPG) AI infrastructure — **Spark supports AI pipelines from v1.1.0 onward.** The AI-powered auto caption and video transcript capability is the first pipeline built on this foundation, with more AI-driven capabilities to follow.

***

### Overview

<table><thead><tr><th width="40">#</th><th width="274">Major Feature</th><th>Summary</th></tr></thead><tbody><tr><td>1</td><td>AI-Powered Auto Captions &#x26; Video Transcripts (v1)</td><td>Automatic caption generation and video transcripts integrated into the Sunbird Video Player, with multilingual caption support across the web and mobile applications</td></tr><tr><td>2</td><td>Viewer Service — Learning Paths (Backend APIs)</td><td>Backend APIs for structured learning programs — ordered Levels bundling Courses and Practice Question Sets, tracked and certified as a single journey, with adaptive skip (waive) via an entry diagnostic. Backend APIs only; no UI in this release</td></tr><tr><td>3</td><td>QuML Editor &#x26; Player Stability</td><td>Editor/Player defects resolved and assessment consumption in SCORM content fixed</td></tr><tr><td>4</td><td>QTI Support (Proof of Concept)</td><td>QTI support plan and design completed, with an initial QTI Player proof of concept</td></tr><tr><td>5</td><td>Security &#x26; Infrastructure Hardening</td><td>Docker Hardened Images (DHI) rolled out as the base image across existing services for a smaller, more secure runtime footprint</td></tr><tr><td></td><td><strong>Private cluster access via VPN or Azure Bastion</strong> </td><td>Added support for fully private AKS clusters with no public IP exposure. Developer/CI access is now configurable</td></tr></tbody></table>

***

### 1. AI-Powered Auto Captions & Video Transcripts (v1)

Sunbird Spark now generates captions and video transcripts automatically and surfaces them in the Sunbird Video Player. Learners can view synchronized captions and full transcripts for video content, with support for multiple languages.

**1.1 Auto Caption Generation & Transcript APIs** A new set of AI-powered caption APIs generate captions and transcripts for video content. The APIs are wired into the content playback pipeline and were finalized through multiple review cycles.

**1.2 Transcript & Multilingual Caption Support in the Sunbird Video Player** Transcript and multilingual caption support is integrated into the Sunbird Video Player on both the **web application** and the **mobile application**, giving learners a consistent captioned playback experience across platforms.

**1.3 Captioning UI** The caption and transcript UI is implemented in the Player, with all reviewed changes merged for the release.

***

### 2. Viewer Service — Learning Paths (Backend APIs)

This release introduces the **Viewer Service** backend APIs for **Learning Paths**, extending Sunbird beyond the single course (enroll, consume, complete) to structured learning programs.

A Learning Path provides:

* **Structure** — ordered Levels, each containing Courses and Practice Question Sets.
* **One tracked journey** — progress, completion, and certification across the entire path, not just per course.
* **Adaptivity** — a learner who can already demonstrate a skill can skip (waive) the courses that teach it, determined by a diagnostic taken at entry or by prior learning.

The Viewer Service reuses the existing course machinery (batch, enrolment, consumption, assessment, and certificate flows), rather than introducing a duplicate program-tracking stack.

> **Note:**&#x20;
>
> * Only the **backend APIs** are delivered in this release. **No UI is available** for Learning Paths in v1.1.0.
> * We have generalized the implementation of `user_content_consumption`. Previously, it was tightly coupled with `course_id` and `batch_id`. We have now decoupled it from these specific identifiers and generalized the functionality as part of the `ViewerService` implementation.

***

### 3. QuML Editor & Player Stability

* Resolved outstanding defects in the **QuML Editor and Player**.
* Fixed **assessment consumption in SCORM content** — implemented, deployed, and verified.

***

### 4. QTI Support (Proof of Concept)

The **QTI support** plan and design are completed, along with an initial **QTI Player** proof of concept, establishing the basis for QTI-based assessment content in a future release.

***

### 5. Security & Infrastructure Hardening

**Docker Hardened Images (DHI)** Service base images are migrated to **Docker Hardened Images (DHI)** across existing services, replacing the previous Node base images with hardened, minimal `dhi.io` images. This reduces the container attack surface and image footprint and standardizes the runtime across services.

**Private cluster access via VPN or Azure Bastion** — Added support for fully private AKS clusters with no public IP exposure. Developer/CI access is now configurable via `vpn_enabled` in `global-values.yaml`: set to `true` for Pritunl VPN + WireGuard (self-hosted, VM gets a public IP just for the VPN endpoint), or `false` for Azure Bastion (browser-based SSH through the Azure Portal, no VPN client, no public IP on the VM at all)

***

### Bug Fixes

<table><thead><tr><th width="160">Area</th><th>Fix</th></tr></thead><tbody><tr><td>QuML Editor</td><td>Editor/Player defects resolved (authoring and playback stability)</td></tr><tr><td>QuML Player</td><td>Editor/Player defects resolved (authoring and playback stability)</td></tr><tr><td>Assessment / SCORM</td><td>Assessment consumption in SCORM content corrected — implemented, deployed, and verified</td></tr><tr><td>Portal</td><td>Portal defects resolved</td></tr><tr><td></td><td></td></tr></tbody></table>

***

### Upgrade Guide

#### Upgrading from v1.0.2

Upgrading from **v1.0.2** to this release introduces a **private cluster** instead of a public cluster.

Please follow the steps below in order:

**Step 1 — Migration**\
Follow the migration steps in the [Velero Migration Guide](https://github.com/Sunbird-Spark/sunbird-spark-installer/blob/main/migration/RELEASE-MIGRATION.md#1-standard-upgrade-velero-backup--restore)

**View Service Migration**\
Follow the steps in the [View Service Migration Guide](https://github.com/Sunbird-Spark/sunbird-spark-installer/blob/main/migration/RELEASE-MIGRATION.md#2-release-specific-migrations)

**Step 3 — Deploy** Deploy the services as per the installer guide. DHI-based service images are pulled automatically as part of the standard deployment.

**Link to Release Tag:** [spark-v1.1.0](https://github.com/Sunbird-Spark/sunbird-spark-installer/releases/tag/spark-v1.1.0)
