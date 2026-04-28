# Completed

> **Note:** Further simplification in progress.

### Completed —&#x20;

**Cost Efficiency and Simplification**

* **API microservices merged** — 8+ services consolidated into `knowlg-service` and `lern-service`
* **YugabyteDB migration complete** across all building blocks (Lern, Knowlg, inQuiry, Keycloak, Sunbird RC)
* **Publish pipeline simplified** — jobs merged, asset enrichment removed, video streaming made configurable
* **Flink jobs pruned** — \~15 unused jobs removed from codebase and installer
* **DIKSHA-specific features** (DIAL/QR codes) made optional add-ons
* **Kong API gateway** upgraded

### **Tech Stack Upgrades**

| Component                 | Before             | After              |
| ------------------------- | ------------------ | ------------------ |
| Scala                     | 2.11 / 2.12        | 2.13.14            |
| Node.js                   | 12 / 14            | 22 LTS             |
| NodeBB (Discussion Forum) | 3.x                | 4.x                |
| Kafka                     | 2.x / 3.x          | 4.x                |
| Akka                      | —                  | Apache Pekko 1.0.3 |
| Play Framework            | 2.x                | 3.0.5              |
| Promtail                  | —                  | Grafana Alloy      |
| Terraform                 | —                  | OpenTofu           |
| Cloud auth                | Static access keys | OIDC               |

### Release Highlights

> Platform, mobile, and infrastructure improvements across the board.

### **New Consumption Apps**

* **New web portal** — React JS, UX-first, AI-assisted screen generation
  * Authentication flows
  * Workspace
  * Course management
  * Batch management
  * User profile
* **Admin Reports Dashboard**\
  Built-in reporting with **no Obsrv dependency** required. Access analytics directly from the default install.
* **Security Patches**\
  Patches applied across **all dependency chains** to keep the platform secure and up to date.
* **Obsrv as Optional Add-on**\
  Obsrv is now **decoupled from the default install** — enable it only when needed via the add-on framework.
* **End-to-End Functional Testing Suite**\
  A full test suite covering **critical user flows** across the platform.
* **API Benchmarking**\
  Performance validated against a **10,000 DAU target** load to ensure scalability.
* **DevOps One-Click Installer**\
  Add-on enablement simplified via a **one-click installer** workflow for easier DevOps management.

### Mobile & Experience

* **Offline Learning — Android**\
  Users can now **download and learn without an internet connection** on the Android mobile app.
* **Internationalisation (I18N) & Multilingual Support**\
  Full **multilingual support** rolled out across the platform for global accessibility, now supporting **English, Français, Português, and العربية**.
* **Instrumentation & Telemetry**\
  Usage tracking and telemetry added to the **new portal and mobile app** for better insights.<br>

{% hint style="info" %}
All changes are backwards compatible. Refer to the migration guide for upgrade steps.
{% endhint %}

***
