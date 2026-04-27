# System Requirements



Before you begin, make sure your infrastructure meets the following requirements to run Sunbird successfully.

***

### Minimum Infrastructure

Sunbird requires a **2-node cluster** to run the base installation.

| Resource       | Requirement                   |
| -------------- | ----------------------------- |
| Nodes          | 2 × Azure Standard\_B16as\_v2 |
| vCPU per node  | 16 vCPU                       |
| RAM per node   | 64 GB                         |
| **Total vCPU** | **32 vCPU**                   |
| **Total RAM**  | **128 GB**                    |

***

### Resource Usage

| Resource | Request    | Limit      | Disk     |
| -------- | ---------- | ---------- | -------- |
| CPU      | \~21 cores | \~50 cores | —        |
| Memory   | \~40 Gi    | \~74 Gi    | —        |
| Disk     | —          | —          | \~219 Gi |

***

### Optional Addons

Addons are optional features you can install on top of the base Sunbird setup.

| Addon                  | What it adds             |
| ---------------------- | ------------------------ |
| DIAL                   | 1 service + 2 Flink jobs |
| Discussion Forum       | 3 services               |
| Video Stream Generator | 1 Flink job              |
| Obsrv                  | Full Obsrv stack         |

> ⚠️ **Obsrv requires 2 additional nodes.** The Obsrv addon runs heavier workloads that cannot fit on the existing 2-node cluster. Please provision 2 extra nodes before installing the Obsrv addon.

> ✅ All other addons (DIAL, Discussion Forum, Video Stream Generator) run on the same 2-node cluster — no extra nodes needed.

***

### What You Need Before Installing

#### Accounts & Credentials

| Requirement                     | Details                                                        |
| ------------------------------- | -------------------------------------------------------------- |
| Domain name                     | A domain pointing to your server                               |
| SSL Certificate                 | Full chain — private key + certificate + CA bundle (mandatory) |
| Google OAuth credentials        | Needed for login via Google                                    |
| Google ReCaptcha v3 credentials | Needed for form protection                                     |
| Email service provider          | For sending system emails                                      |
| MSG91 API token _(optional)_    | For sending OTPs via SMS during registration or password reset |
| YouTube API token _(optional)_  | For uploading video content via YouTube URL                    |

#### CLI Tools

| Tool          | Purpose                         |
| ------------- | ------------------------------- |
| `OpenTofu`    | Provisions cloud infrastructure |
| `Terragrunt`  | Wrapper for OpenTofu            |
| `kubectl`     | Manages the Kubernetes cluster  |
| `helm`        | Installs Kubernetes packages    |
| `rclone`      | Syncs files to cloud storage    |
| `jq`          | Processes JSON output           |
| `yq`          | Processes YAML files            |
| `Python 3`    | Required for scripts            |
| `PyJWT`       | Install via `pip install PyJWT` |
| `Postman CLI` | Runs API setup scripts          |

> Supported on **Linux**, **macOS**, and **GitBash (Windows)**.
>
> For cloud-specific tools, refer to the README in your cloud provider's folder. Example: `opentofu/azure/README.md`

#### Verified Tool Versions

| Tool       | Verified Version |
| ---------- | ---------------- |
| OpenTofu   | v1.11.4          |
| Terragrunt | v0.77.5          |

> These are the versions tested and confirmed to work. If you face issues with other versions, switch to these.

***

### Things to Know Before You Start

> ℹ️ The installer will back up and overwrite the following files:
>
> * `~/.config/rclone/rclone.conf`
> * `~/.kube/config`
>
> Your originals will be saved with a `.bak` extension.

Sunbird-ED is deployed entirely on cloud infrastructure. Before running the installer, make sure your cloud environment is provisioned and all the requirements are satisfied.

***

### Minimum Infrastructure

Sunbird requires a **2-node cluster** to run the base installation.

| Resource       | Requirement                   |
| -------------- | ----------------------------- |
| Nodes          | 2 × Azure Standard\_B16as\_v2 |
| vCPU per node  | 16 vCPU                       |
| RAM per node   | 64 GB                         |
| **Total vCPU** | **32 vCPU**                   |
| **Total RAM**  | **128 GB**                    |

### Resource Usage

| Resource | Request    | Limit      | Disk     |
| -------- | ---------- | ---------- | -------- |
| CPU      | \~21 cores | \~50 cores | —        |
| Memory   | \~40 Gi    | \~74 Gi    | —        |
| Disk     | —          | —          | \~219 Gi |

### Optional Addons

{% stepper %}
{% step %}
<mark style="color:$info;">**Add-ons are optional features that are disabled by default**</mark><mark style="color:$info;">.</mark> They can be enabled at any time after the base installation.
{% endstep %}

{% step %}
Even with all add-ons running at once, <mark style="color:$info;">**no additional nodes are required**</mark><mark style="color:$info;">.</mark> The base cluster has enough capacity to handle them comfortably.
{% endstep %}

{% step %}
Sunbird allows you to turn on <mark style="color:$info;">**extra features like Discussion Forums, Video Stream Generator, Obsrv and QR Code (DIAL) support**</mark><mark style="color:$info;">.</mark>
{% endstep %}
{% endstepper %}

| Addon                  | What it adds             |
| ---------------------- | ------------------------ |
| DIAL                   | 1 service + 2 Flink jobs |
| Discussion Forum       | 3 services               |
| Video Stream Generator | 1 Flink job              |
| Obsrv                  | Full Obsrv stack         |

{% hint style="info" %}
<mark style="color:$info;">To install Sunbird Spark addons, you can use the existing 2-node cluster for DIAL, Discussion Forum, and Video Stream Generator, but</mark> <mark style="color:$info;">**you must provision 2 additional nodes (4 total) specifically for the Obsrv addon due to its heavier workload.**</mark>
{% endhint %}

***

### Requirements Before Installing

| Requirement                                               | Description                                                                                                                     |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Domain name                                               | A registered domain that will point to your Sunbird-ED instance                                                                 |
| SSL certificate                                           | A FullChain certificate — includes the private key, certificate, and CA bundle                                                  |
| Google OAuth credentials                                  | Used for user authentication — [set up here](https://developers.google.com/workspace/guides/create-credentials#oauth-client-id) |
| Google reCAPTCHA v3 credentials                           | Used to protect public-facing forms — [set up here](https://www.google.com/recaptcha/admin)                                     |
| Email service provider                                    | Any SMTP-compatible email service for sending platform emails                                                                   |
| <mark style="color:$info;">MSG91 API token (opt)</mark>   | <mark style="color:$info;">Required if you want to send OTPs via SMS during registration or password reset</mark>               |
| <mark style="color:$info;">YouTube API token (opt)</mark> | <mark style="color:$info;">Required if you want users to upload video content via YouTube URL</mark>                            |

***

### CLI Tools

<table><thead><tr><th width="197">Tool</th><th>Description</th></tr></thead><tbody><tr><td><code>jq</code></td><td>Command-line JSON processor — <a href="https://jqlang.github.io/jq/download/">install</a></td></tr><tr><td><code>yq</code></td><td>Command-line YAML processor — <a href="https://github.com/mikefarah/yq#install">install</a></td></tr><tr><td><code>rclone</code></td><td>Cloud storage sync tool — <a href="https://rclone.org/">install</a></td></tr><tr><td><code>OpenTofu</code></td><td>Infrastructure provisioning tool — <a href="https://opentofu.org/docs/intro/install/">install</a> | v1.11.4</td></tr><tr><td><code>Terragrunt</code></td><td>Wrapper for OpenTofu to manage environments — <a href="https://terragrunt.gruntwork.io/docs/getting-started/install/">install</a> | v0.77.5</td></tr><tr><td><code>kubectl</code></td><td>Kubernetes command-line tool — <a href="https://kubernetes.io/docs/tasks/tools/">install</a></td></tr><tr><td><code>helm</code></td><td>Kubernetes package manager — <a href="https://helm.sh/docs/intro/quickstart/#install-helm">install</a></td></tr><tr><td><code>Postman CLI</code></td><td>Runs the post-install API setup — <a href="https://learning.postman.com/docs/getting-started/installation/installation-and-updates/">install</a></td></tr><tr><td>Python 3</td><td>Required for helper scripts</td></tr><tr><td>PyJWT</td><td>Python package — install via <code>pip install PyJWT</code></td></tr></tbody></table>

> For cloud-specific tools such as the Azure CLI, AWS CLI, or GCP, follow the instructions in the provider-specific README inside the repository. Example: `opentofu/azure/README.md`

***

### Things to Know Before You Start

{% hint style="info" %}
The installer will back up and overwrite the following files:

* `~/.config/rclone/rclone.conf`
* `~/.kube/config`

Your originals will be saved with a `.bak` extension.
{% endhint %}

For further reference, please visit [ReadME](https://github.com/Sunbird-Spark/sunbird-spark-installer/blob/develop/INFRA_DETAILS.md).
