# Configuration Guide

#### Environment variables

All Spark services are configured via environment variables injected at deployment time by the installer.

#### Key variables:

<table><thead><tr><th width="346.1015625">Variable</th><th>Description</th></tr></thead><tbody><tr><td>DOMAIN</td><td>Your deployment domain (e.g., learn.your-org.com)</td></tr><tr><td>CLOUD_PROVIDER</td><td>Cloud storage provider: aws, azure, gcp, or oci</td></tr><tr><td>CLOUD_STORAGE_BUCKET</td><td>Name of your cloud storage bucket</td></tr><tr><td>CLOUD_STORAGE_REGION</td><td>Region of your cloud storage bucket</td></tr><tr><td>KEYCLOAK_ADMIN_PASSWORD</td><td>Keycloak admin password</td></tr><tr><td>SUNBIRD_ADMIN_EMAIL</td><td>Email address for the default root organisation admin</td></tr><tr><td>SMTP_HOST</td><td>SMTP server hostname</td></tr><tr><td>SMTP_PORT</td><td>SMTP server port (typically 587)</td></tr><tr><td>SMTP_USERNAME</td><td>SMTP authentication username</td></tr><tr><td>SMTP_PASSWORD</td><td>SMTP authentication password</td></tr><tr><td>VIDEO_STREAMING_ENABLED</td><td>true or false — adaptive bitrate streaming for videos. Default: false</td></tr><tr><td>ECAR_GENERATION_ENABLED</td><td>true or false — generate ECAR packages for offline use. Default: true</td></tr></tbody></table>

#### Feature flags (add-ons)

| Variable                   | Description                                         |
| -------------------------- | --------------------------------------------------- |
| OBSRV\_ENABLED             | Enable the Obsrv analytics pipeline. Default: false |
| DISCUSSION\_FORUM\_ENABLED | Enable NodeBB discussion forums. Default: false     |
| GROUPS\_ENABLED            | Enable Groups / cohort management. Default: false   |
| DIAL\_ENABLED              | Enable DIAL/QR code support. Default: false         |

### Multi-Tenancy (Channel) Configuration

Spark supports logical multi-tenancy via **Channels**. Each channel (tenant) is an isolated configuration unit with its own:

* **Channel ID** — a unique slug (e.g., `tn` for Tamil Nadu)
* **Framework and taxonomy** — independent curriculum or category hierarchy
* **Content pool** — content is tagged with a channel ID and scoped accordingly
* **Theming** — logo, colours, and layout overrides
* **Administrator** — a designated admin with permissions scoped to that channel

To create a new channel, use the Organisation API:

```http
POST /api/org/v2/create
```

For full setup instructions, see: [Reference Guides › Multi-Tenancy Explained](../../misc/reference-guides/telemetry-pipeline/multi-tenancy-explained.md)

***

### Cloud Storage Configuration (OIDC)

{% hint style="info" %}
**New in Spark:** Cloud storage access uses OIDC (Workload Identity Federation) instead of static access keys, eliminating long-lived credentials and reducing credential exposure risk.
{% endhint %}

#### AWS — IRSA (IAM Roles for Service Accounts)

```yaml
cloudStorage:
  provider: aws
  region: ap-south-1
  bucket: your-bucket-name
  irsaRoleArn: arn:aws:iam::123456789:role/spark-cloud-storage
```

#### Azure — Workload Identity

```yaml
cloudStorage:
  provider: azure
  storageAccount: yourstorageaccount
  container: spark-content
  workloadIdentityClientId: your-managed-identity-client-id
```
