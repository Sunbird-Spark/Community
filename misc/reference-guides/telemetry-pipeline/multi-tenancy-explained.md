# Multi-tenancy explained

Sunbird Spark supports logical multi-tenancy via a Channel model. This page explains how multi-tenancy works and how to configure it.

#### How multi-tenancy works

In Spark, a "tenant" is called a Channel or Organisation. Each channel has:

* A unique channel ID — a short slug (e.g., tn for Tamil Nadu, acme for ACME Corporation)
* Its own framework — a taxonomy configuration specific to the channel's domain
* Its own content pool — content is tagged with a channel ID and is only visible to users of that channel (unless explicitly shared)
* Its own theme — logo, brand colours, and layout configuration
* Its own administrator — users with the ORG\_ADMIN role for that channel

#### What is shared across channels?

* The platform infrastructure (all channels share the same Kubernetes cluster, databases, and services)
* Content that is explicitly published to the OPEN channel (public content visible to all channels)
* System-level configuration (API gateway, authentication service)

Logical multi-tenancy is enforced at the application layer — queries include a channel filter, and the API gateway validates that the requesting user belongs to the claimed channel.

This is not physical multi-tenancy (separate databases per tenant). For deployments requiring strict data isolation between tenants, physical separation (separate Spark deployments per tenant) is recommended.

#### Creating a new channel (tenant)

```
# Create the organisation (channel) via API
curl -X POST https://your-domain.com/api/org/v2/create \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <admin-token>" \
  -d '{
    "request": {
      "orgName": "ACME Academy",
      "channel": "acme",
      "isRootOrg": true,
      "slug": "acme"
    }
  }'
```

After creating the channel:

1. Create an administrator user and assign the ORG\_ADMIN role for the new channel
2. Create or import a framework for the channel using the Framework API
3. Configure the channel's theme in the Form Service
4. Configure the channel's homepage sections in the Form Service
