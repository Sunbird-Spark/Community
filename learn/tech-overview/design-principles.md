---
description: >-
  Sunbird Spark is built on five core architectural principles. Understanding
  these principles helps adopters make good decisions about how to deploy,
  configure, and extend the platform.
---

# Design Principles

### 1. Microservices thinking

Spark is not a pre-packaged monolithic application. It is a set of generalised microservices — "Lego blocks" — that can be composed into many different solutions.

Each microservice exposes a well-defined API. An adopter can use the content APIs to build a custom learner-facing application in React, while still using Spark's backend services for content management, search, and tracking. Another adopter might use only the question bank building block to add assessments to an existing application.

The reference implementation — the Spark portal and mobile app — is one way to compose these services. It is not the only way.

### 2. Unbundling for diversity

Spark exposes hundreds of APIs covering content creation, collection management, user management, enrolment, progress tracking, certificate issuance, search, telemetry, and more. Each API is designed to be general-purpose — not tied to a specific domain or workflow.

For example, the Collection API is abstract enough to represent a school textbook, a corporate training course, a healthcare protocol library, or a government policy document archive. The concept of "collection" is not specific to education.

This unbundling means adopters can reimagine solutions using Spark's building blocks in ways that the original designers didn't anticipate — and they regularly do.

### 3. Loose coupling for evolution

Spark's building blocks are not tightly dependent on each other. The Knowlg building block (content management) can be deployed and used without the Lern building block (user management and enrolment), and vice versa.

This decoupling means:

* Adopters can start with a subset of building blocks and add more over time
* Individual building blocks can be upgraded independently
* A failure in one building block does not cascade to others
* Adopters can replace individual building blocks with their own implementations, as long as the API contracts are honoured

### 4. Observability through emit, not extract

Every Spark microservice is built to **emit** telemetry as a first-class design concern — not as an afterthought. Every user interaction, every API call, and every system event generates a structured telemetry event that flows through the telemetry pipeline automatically.

This is contrasted with "extract" architectures, where data is pulled out of application databases and logs after the fact. Extraction is brittle, privacy-risky, and operationally expensive. Emission is native, consistent, and privacy-safe (PII is masked at the point of emission).

**The result**: a Spark deployment generates comprehensive usage data automatically, without any additional instrumentation work by the adopter.

### 5. Lean by default, scale on demand

Spark ships with the minimum viable set of services running. Features that are not universally needed — the full analytics pipeline, discussion forums, QR code infrastructure, project and observation management — are disabled by default and can be enabled as add-ons.

A fresh Spark installation runs approximately 60% fewer services than Sunbird ED 8.x. This dramatically reduces both the infrastructure cost (target: \~$600/month for 100k users / 10k DAU) and operational complexity for the adopter.

When an adopter needs to scale — or when they need a specific add-on capability — those services are available and have been proven at population scale (DIKSHA: 260M users, 2B+ events/day). The architecture does not need to be redesigned to scale; it needs more infrastructure.
