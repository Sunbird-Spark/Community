# Spark Infra, Deployment & Platform Config

This section is for DevOps engineers ready to deploy Sunbird Spark on their own infrastructure. It covers everything from provisioning a cluster to getting the platform up and running.

***

### How Spark works

Sunbird Spark is a three-part process.

**1. Provision your infrastructure**

Sunbird Spark runs on a Kubernetes cluster. Before anything is installed, you need a cluster ready on your cloud provider — Azure AKS, GCP GKE, or on-premises. The cluster needs to meet minimum size requirements, support persistent volumes, and have a registered domain with DNS control.

**2. Run the installer**

Once your infrastructure is ready, the one-click installer takes over. It provisions cloud resources, deploys all Sunbird services onto the cluster in the correct order, configures certificates, waits for DNS propagation, and seeds the platform with initial data — all in a single run.

**3. Enable add-ons** `(optional)`

After the base installation is complete, optional add-ons can be deployed independently to extend the platform's capabilities — such as QR code–based content linking, community discussion forums, and video streaming.

***

### Get started

{% stepper %}
{% step %}
#### Check the minimum infrastructure specifications before provisioning your cluster

[**System Requirements →**](../use/getting-started/system-requirements.md)
{% endstep %}

{% step %}
#### Step-by-step guide to deploying Sunbird Spark on your infrastructure

[**One-Click Installer →**](../use/getting-started/one-click-installer.md)
{% endstep %}

{% step %}
#### Install optional features on top of the base platform

[**Enabling Add-ons →**](../use/getting-started/enabling-add-ons.md)
{% endstep %}
{% endstepper %}
