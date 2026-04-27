# One-click Installer

## Sunbird Spark — Installation Guide

The one-click installer sets up a full Sunbird-ED deployment on your cloud infrastructure. It provisions the cluster, installs all services, and configures the platform end-to-end — no manual steps needed once everything is in place.

Authentication with cloud storage is supported via both **Managed Identity** and **Access Keys**. Use whichever fits your setup.

***

***

### How It Works

The one-click installer sets up a complete Sunbird-ED deployment on your cloud infrastructure. It performs the following end-to-end operations:

* Cloud infrastructure provisioning
* Kubernetes cluster setup
* Sunbird service deployment
* Platform configuration

Once triggered, no manual steps are required.

The installer is hosted in the public repository:\
`Sunbird-Spark/sunbird-spark-installer`

You can run the installer in two ways:

* Via GitHub Actions (using a private configuration repository)
* Directly on a VM using `install.sh`

Both methods produce the same result. The GitHub Actions approach automates the process and keeps secrets secure

***

### Before You Begin

Make sure the following are in place before starting:

* All items in the [**System Requirements**](system-requirements.md) checklist are configured
* You have access to the public installer repo: `Sunbird-Spark/sunbird-spark-installer`
* You have a private repository ready to hold your environment-specific configuration

***

### Architecture Overview

<figure><img src="../../.gitbook/assets/spark-installer (2).png" alt=""><figcaption></figcaption></figure>

### Installation Methods

#### Option A — GitHub Actions

This is the recommended approach for production environments. Your environment configuration lives in a private repository and is never exposed. The GitHub Actions workflow pulls the installer code from the public repo at runtime and applies your config on top.

{% hint style="warning" %}
Refer the [README](https://github.com/Sunbird-Spark/sunbird-spark-installer/blob/develop/private-repo-setup/README.md) for more information
{% endhint %}

**Step 1 — Set up the public installer repo**

Clone the installer and create a new branch from the latest tag or the `develop` branch:

```bash
git clone https://github.com/Sunbird-Spark/sunbird-spark-installer.git
git checkout develop
git checkout -b <your-env-name>   # e.g. demo-dev
```

Push this branch to GitHub.

**Step 2 — Set up the private config repo**

Create a new **private** repository to store your environment configuration. This repo will contain secrets such as API tokens, passwords, and cloud credentials.

Inside the private repo, create a branch named after your environment (e.g. `my-dev`). The branch must have the following structure:

```
sunbird-spark-installer/
├── .github/
│   └── workflows/
│       └── sunbird-spark-platform.yaml        # Single workflow — infra, deploy, platform
|       └── sunbird-spark-addons.yaml
├── configs/
│   └── dev/
│       └── global-values.yaml         # Main config (Ansible Vault encrypted)b 
└── scripts/
    ├── setup-azure-oidc.sh            # One-time OIDC setup for infra/deploy service principals
    └── setup-installer-vm.sh          # OIDC setup script (VM creation steps disabled)
```

Copy the below files into the private repository created:

* &#x20;[workflows/](https://github.com/Sunbird-Spark/sunbird-spark-installer/tree/develop/private-repo-setup/.github/workflows)  - workflows, infra, deploy and platform creation
* &#x20;[scripts/](https://github.com/Sunbird-Spark/sunbird-spark-installer/tree/develop/private-repo-setup/scripts)  - azure authentication and client ID creation

***

* **`sunbird-spark-platform.yaml`** — The file provisions, deploys and configures spark in **\~1hr 25min**.
* **`sunbird-spark-addons.yaml`** — (OPTIONAL)  deploys and configures the additional building blocks in spark.
* **`global-values.yaml`** — contains your environment name, cloud subscription details, domain, credentials, and all platform settings. Modify this file from the repo and fill in your values.

Encrypt `global-values.yaml` using Ansible Vault before committing:

```bash
ansible-vault encrypt configs/<env>/global-values.yaml
```

**Step 3** **— Configure the OIDC Setup Script**

Before running the script, you need to fill in the configuration variables at the top of `scripts/setup-azure-oidc.sh`.

```bash
cd spark-devops-dev
nano scripts/setup-azure-oidc.sh
```

Fill in these values at the top of the file: (example below)

```bash
TENANT_ID="12345678-abcd-1234-abcd-123456789abc"           # Your Azure AD Tenant ID
SUBSCRIPTION_ID="87654321-dcba-4321-dcba-cba987654321"     # Your Azure Subscription ID
BUILDING_BLOCK="ed"                                      # Short prefix (matches global-values.yaml)
ENVIRONMENT="dev"                                           # Environment name (matches configs/dev/)
RESOURCE_GROUP="m"                                  # Azure resource group name
GITHUB_REPO="my-org/spark-devops-dev"                      # Your private repo (org/repo format)
GITHUB_ENVIRONMENT="dev"                                    # GitHub Actions environment name
```

**How to Get These Values**

<table><thead><tr><th width="314">Variable</th><th>How to Get It</th></tr></thead><tbody><tr><td><code>TENANT_ID</code></td><td>Run: <code>az account show --query tenantId -o tsv</code></td></tr><tr><td><code>SUBSCRIPTION_ID</code></td><td>Run: <code>az account show --query id -o tsv</code></td></tr><tr><td><code>BUILDING_BLOCK</code></td><td>Choose a short prefix for your org (2-8 chars, lowercase)</td></tr><tr><td><code>ENVIRONMENT</code></td><td>Must match your <code>configs/</code> folder name</td></tr><tr><td><code>RESOURCE_GROUP</code></td><td>Format: <code>{BUILDING_BLOCK}-{ENVIRONMENT}</code></td></tr><tr><td><code>GITHUB_REPO</code></td><td>Your private repo in <code>org/repo</code> format</td></tr><tr><td><code>GITHUB_ENVIRONMENT</code></td><td>Same as <code>ENVIRONMENT</code></td></tr></tbody></table>

**Step 4 — Create Azure service principals (OIDC)**

The workflow uses **two separate service principals** — one for infrastructure provisioning and one for deployment. Run `scripts/setup-azure-oidc.sh` in terminal:

```bash
# Infra SP — Contributor role (Terraform operations)
APP_NAME="ed-dev-github-infra" 
bash scripts/setup-azure-oidc.sh

# Deploy SP — AKS Cluster Admin Role (helm/kubectl)
APP_NAME="ed-dev-github-deploy"
bash scripts/setup-azure-oidc.sh
```

Copy the printed both `CLIENT_ID` values and `TENANT_ID`, add them to your private repository's github secrets.

Set these under **Settings → Secrets and variables → Actions** and under the `demo-dev` environment (**Settings → Environments → dev**):

Copy the output values into your private repository's **Secrets**:

| Secret Name              | Value                                             |
| ------------------------ | ------------------------------------------------- |
| `AZURE_INFRA_CLIENT_ID`  | Client ID for the infra service principal         |
| `AZURE_DEPLOY_CLIENT_ID` | Client ID for the deploy service principle        |
| `AZURE_TENANT_ID`        | Your Azure Tenant ID                              |
| `AZURE_SUBSCRIPTION_ID`  | Your Azure Subscription ID                        |
| `ANSIBLE_VAULT_PASSWORD` | The password used to encrypt `global-values.yaml` |

**Step 5 — Provision infrastructure + Service Deployment**

Trigger the `sunbird-spark-platform.yaml` workflow and select inputs under the **Infra** section. Only jobs with at least one checkbox selected will execute.

| Input                 | Description                                                                           |
| --------------------- | ------------------------------------------------------------------------------------- |
| `create_tf_backend`   | Create Terraform backend (storage account + container)                                |
| `backup_configs`      | Backup kube and rclone configs                                                        |
| `create_tf_resources` | Create all Terraform resources (network, storage, AKS, workload-identity, keys, etc.) |

This job runs under `AZURE_INFRA_CLIENT_ID` with Contributor permissions for Terraform state, network, AKS, and storage provisioning.

Once infrastructure is ready, trigger the same `sunbird-spark-platform.yaml` workflow and select inputs under the **Deploy** and **Platform** sections.

**Deploy inputs:**

| Input              | Description/Services                                            |
| ------------------ | --------------------------------------------------------------- |
| `install_helm`     | Enable Helm bundle installation                                 |
| `helm_mode`        | `selective` — pick bundles below / `all` — install every bundle |
| `monitoring`       | Install monitoring bundle                                       |
| `edbb`             | Install edbb bundle (player, echo, router, kong)                |
| `player_image_tag` | Player image tag — leave empty to use `images.yaml` default     |
| `learnbb`          | Install learnbb bundle                                          |
| `knowledgebb`      | Install knowledgebb bundle                                      |
| `obsrvbb`          | Install obsrvbb bundle                                          |
| `inquirybb`        | Install inquirybb bundle                                        |
| `additional`       | Install additional bundle                                       |

**Platform inputs** (post-deploy configuration):

| Input                  | Description                                       |
| ---------------------- | ------------------------------------------------- |
| `restart_workloads`    | Restart workloads using keycloak keys             |
| `certificate_config`   | Configure certificate keys                        |
| `dns_mapping`          | DNS mapping (waits up to 20 mins for propagation) |
| `generate_postman_env` | Generate Postman environment file                 |
| `run_post_install`     | Run post-install Postman collection               |
| `create_client_forms`  | Create client forms                               |

Deploy and platform jobs both run under `AZURE_DEPLOY_CLIENT_ID` with AKS Cluster Admin Role permissions.

***

#### Option B — VM via install.sh

Use this method to run the installer directly on a Azure VM.

**Step 1 — Clone the installer onto the Virtual Machine**

```bash
git clone https://github.com/Sunbird-Spark/sunbird-spark-installer.git
cd sunbird-spark-installer
git checkout develop
## Create a new <your-env-name> on top of develop
```

**Step 2 — Create your environment directory**

Navigate to your cloud provider directory and copy the template:

```bash
cd opentofu/<cloud-provider>   # e.g. azure, gcp, aws
cp -r template <your-env-name>
cd <your-env-name>
```

**Step 3 — Configure values**

Fill in all required values in `global-values.yaml,` Refer the Step-4 README.

{% hint style="warning" %}
**Also refer the SSL Certificate Setup from the** [README](https://github.com/Sunbird-Spark/sunbird-spark-installer/tree/develop#ssl-certificate-setup-and-renewal-lets-encrypt-integration)
{% endhint %}

**Step 4 — Log in to your cloud provider**

{% tabs %}
{% tab title="Azure" %}
**Required tools and permissions:**

Refer to the [README](https://github.com/Sunbird-Spark/sunbird-spark-installer/blob/develop/opentofu/azure/README.md) for a full walkthrough.

{% hint style="info" %}
Make sure you replace the AZURE\_TENANT\_ID with the tenant id from Azure Console.
{% endhint %}
{% endtab %}
{% endtabs %}

**Step 5 — Run the installer**

bash

```bash
time ./install.sh
```

The script provisions infrastructure, deploys all Sunbird services, and configures the platform end-to-end.

***

### GCP, AWS Cloud Deployments

{% tabs %}
{% tab title="Google Cloud" %}


**Required tools and permissions**

1. Google Cloud CLI ([https://cloud.google.com/sdk/docs/install](https://cloud.google.com/sdk/docs/install))
2. Ensure that the user or service account running the Terraform script has the necessary privileges as [listed here](https://registry.terraform.io/providers/hashicorp/google/latest/docs/guides/provider_reference#authentication).

**Note:** We will overwrite the following files. Please take a backup of your existing files in the following locations:

* `~/.config/rclone/rclone.conf`

#### Authentication

Post installation of the CLI tool and providing necessary permissions, use the following commands to login to GCP via CLI, Refer [README](https://github.com/Sunbird-Spark/sunbird-spark-installer/blob/develop/opentofu/gcp/README.md)

```
gcloud auth login
```

#### **GCP Infra Setup**

Post login, update the `opentofu/gcp/<env>/global-values.yaml` file with the variables as per your environment:

```
global:
  building_block: "" 
  env: "" 
  environment: "" # environment name
  release_version: "release-7.0.0"
  # Cluster-level variables for GKE setup and networking
  gke_node_pool_instance_type: "n2d-standard-16"
  create_network: "true"
  gke_node_default_disk_size_gb: 100
  object_storage_endpoint: storage.googleapis.com
  checkpoint_store_type: "gcs"
  cloud_storage_provider: "gcp"
  cloud_service_provider: "google"
  sunbird_cloud_storage_provider: "gcloud"
  cloud_storage_region: ""
  zone: ""
  cloud_storage_project: ""
  # Provide your domain name
  # Example: sunbird.com
  domain: "REPLACE_ME"
  lets_encrypt_ssl: false #enable this if you are using  let's encrypt ssl certificate
 # Provide values for the following variables
  sunbird_google_captcha_site_key: 
  google_captcha_private_key: 
  sunbird_google_oauth_clientId: 
  sunbird_google_oauth_clientSecret: 
  mail_server_from_email: ""
  mail_server_password: ""
  mail_server_host: smtp.sendgrid.net
  mail_server_port: "587"
  mail_server_username: apikey
  sunbird_msg_91_auth: ""
  sunbird_msg_sender: ""
  youtube_apikey: ""
  mail_from_certbot_notifications: ""  #The "from" email address used by the Certbot renewal job for sending SSL renewal status notifications via SendGrid SMTP
  youtube_apikey: ""
```
{% endtab %}

{% tab title="AWS" %}
### **Coming Soon**
{% endtab %}
{% endtabs %}

***

#### Default Users <a href="#default-users" id="default-users"></a>

The installer creates the following users automatically. You can update passwords using the **Forgot Password** flow or create new users via the admin API.

| Role             | Email                                                             | Default Password |
| ---------------- | ----------------------------------------------------------------- | ---------------- |
| Admin            | [admin@yopmail.com](mailto:admin@yopmail.com)                     | Admin@123        |
| Content Creator  | [contentcreator@yopmail.com](mailto:contentcreator@yopmail.com)   | Creator@123      |
| Content Reviewer | [contentreviewer@yopmail.com](mailto:contentreviewer@yopmail.com) | Reviewer@123     |
| Book Creator     | [bookcreator@yopmail.com](mailto:bookcreator@yopmail.com)         | Bookcreator@123  |
| Book Reviewer    | [bookreviewer@yopmail.com](mailto:bookreviewer@yopmail.com)       | bookReviewer@123 |
| Public User 1    | [user1@yopmail.com](mailto:user1@yopmail.com)                     | User1@123        |
| Public User 2    | [user2@yopmail.com](mailto:user2@yopmail.com)                     | User2@123        |

***

### Destroying the resources

To permanently remove all provisioned infrastructure for an environment, navigate to your environment directory and run:

bash

```bash
./install.sh destroy_tf_resources
```

> **This action is irreversible.** All resources provisioned by the installer for that environment will be permanently deleted. Ensure you have a complete backup before proceeding.

## Note:

SMS and email providers will support only MSG91 and SendGrid.
