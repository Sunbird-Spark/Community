# Overview

### **What is Sunbird Spark?**

Sunbird Spark is open source, free-to-use solution infrastructure that can be used for learning, education and skilling use cases — a Digital Public Good (DPG) that any organisation can adopt, configure, and deploy to run learning, skilling and capacity building programmes at scale.

Sunbird Spark is **domain-agnostic**. The same platform works equally well for:

* School and higher education
* Government civil service training
* Corporate workforce development and L\&D
* Healthcare worker training
* Agricultural extension programmes
* Any domain where structured learning and capacity building is a primary need

It is the next evolution of the Sunbird building block ED — redesigned to be leaner, faster to deploy, and more cost-effective, without compromising on its ability to scale when needed. The same core architecture that powers DIKSHA — India's national school education platform serving 260 million+ users — now packages into an infrastructure footprint that a mid-size organisation can run for approximately **$800 per month for 100,000 users**.

{% hint style="info" %}
**Open source and free:** Sunbird Spark is licensed as a Digital Public Good. The platform, all source code, and all deployment tooling are free to use, adopt, and customise. There is no licence fee.
{% endhint %}

***

### **Who is using Sunbird?**

{% tabs %}
{% tab title="DIKSHA" %}
<div data-full-width="true"><figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure></div>

**Digital Infrastructure for Knowledge Sharing**\
_By NCERT, Ministry of Education, India_

India's national school education platform. Deployed across all 36 states and Union Territories, impacting nearly every student and teacher in the country. Processes 2 billion+ telemetry events per day at peak.

[Learn more](https://diksha.gov.in/)
{% endtab %}

{% tab title="iGOT Karmayogi" %}
<p align="center"><img src="../.gitbook/assets/image (1).png" alt="" data-size="original"></p>

**Integrated Government Online Training**\
_By the Government of India_

India's civil service capacity building platform, enabling structured professional development for government employees across all departments and levels of administration.

[Learn more](https://igotkarmayogi.gov.in/)
{% endtab %}

{% tab title="Lex by Infosys" %}
**Enterprise Learning Experience Platform**\
&#xNAN;_&#x42;y Infosys_

A cloud-first, mobile-first enterprise L\&D platform built on the Sunbird building blocks. Delivers professional development to Infosys's global workforce — accessible anytime, on any device.
{% endtab %}
{% endtabs %}

***

### **What does Spark provide?**

Spark gives you a fully functional platform with two consumer-facing applications, a content creation and management workspace, course consumption, management and tracking, capabilities to issue and verify credentials, and built-in analytics — all deployable on Kubernetes on any major cloud provider.

**Two applications for learners:**

* **Web Portal** — Angular 19, fully responsive, works in any modern browser. Supports anonymous browsing and authenticated learning.
* **Mobile App** — Ionic/Capacitor, available for Android (API 28+). Supports full offline content download and consumption.

**For content creators and administrators:**

* A content creation workspace with editors for interactive, video, PDF, and collection content
* Course and batch management capabilities with enrolment windows, progress tracking, and automated certificate issuance based on criteria
* A configurable framework and taxonomy system that can be set up for any domain and use case
* Built-in platform analytics and learner progress reports — no separate analytics infrastructure required for basic metrics and reports

**For platform operators:**

* A Kubernetes-native deployment with a one-click installer
* An add-on architecture — features you don't need don't run, keeping costs low
* Cloud-provider-agnostic — deploy on AWS, Azure, GCP, OCI, or on-premises

***

### Key Functionalities

| Functionality          | Description                                                 |
| ---------------------- | ----------------------------------------------------------- |
| Learning Apps          | Web portal and mobile app (Android)                         |
| Asset Sourcing         | Crowdsource, contribute, and curate digital content         |
| Organised Collections  | Courses, playlists, and digital textbooks                   |
| Search and Discovery   | ElasticSearch-powered full-text and faceted search          |
| User Engagement        | Notifications, passbook, groups, discussion forums (add-on) |
| Rich Content Formats   | PDF, video, H5P, EPUB, interactive (ECML), audio            |
| Question Bank          | QuML-based assessments, quizzes, surveys, and observations  |
| Observability          | Built-in reports; full Obsrv analytics pipeline as add-on   |
| Course Management      | Batch enrolment, progress tracking, completion certificates |
| Verifiable Credentials | SVG certificates issued via Sunbird RC, publicly verifiable |
| Targeted Programmes    | Projects, observations, surveys — available as add-on       |

{% hint style="info" %}
**New in Spark:** Several features that were part of the default Sunbird ED installation are now **optional add-ons** in Spark — disabled by default to reduce infrastructure cost and complexity. These include: Obsrv (analytics pipeline), Discussion Forums, Groups, DIAL/QR codes, and Manage Learn. They can be enabled at any time. See [Getting Started › Enabling Add-ons](../use/getting-started/enabling-add-ons.md).
{% endhint %}
