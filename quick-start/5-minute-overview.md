# 5-minute overview

_For product managers, programme directors, and organisational decision-makers evaluating Sunbird Spark._

***

**What is Sunbird Spark?**

Sunbird Spark is a free, open-source learning management platform. Any organisation — a government ministry, a hospital network, a corporate HR team, a non-profit — can deploy it to run structured learning and capacity-building programmes. It supports course creation, learner enrolment, progress tracking, and verifiable certificate issuance, with mobile and web access for learners and a full administrative console for operators.

Spark is a **Digital Public Good**: free to use, free to customise, and free to deploy.

***

**What Can You Build with Spark?**

{% tabs %}
{% tab title="State school teacher training" %}
A state education department deploys Spark to run mandatory annual training for 500,000 teachers. Teachers complete self-paced courses on their smartphones, take assessments at the end of each module, and receive a digitally verifiable certificate on completion. The state administrator monitors course-by-course completion rates and identifies districts with low engagement.
{% endtab %}

{% tab title="Hospital staff induction" %}
A hospital network deploys Spark to onboard new clinical and non-clinical staff across 40 hospitals. Induction courses are mandatory; completion is tracked per department. Staff access content on shared tablets in break rooms or on personal phones via the offline mobile app. Compliance reports are generated monthly from the built-in admin dashboard.
{% endtab %}

{% tab title="Corporate upskilling" %}
An enterprise deploys Spark to deliver a structured data literacy programme to 10,000 employees globally. Employees self-enrol in learning paths, managers view team-level progress reports, and employees who complete the programme receive a shareable, verifiable certificate.
{% endtab %}
{% endtabs %}

***

**What Does It Cost to Run?**

> **Infrastructure cost target:** \~$800/month for up to 100,000 registered users (10,000 daily active users) on a standard cloud configuration.

This is achievable because Spark's **add-on architecture** means you only run the services your deployment actually needs. A typical base installation requires **3 Kubernetes nodes**.

For larger deployments, the architecture scales horizontally — additional nodes expand capacity in line with your user base.

***

**What Does It Take to Deploy?**

| Requirement              | Detail                                                                          |
| ------------------------ | ------------------------------------------------------------------------------- |
| Infrastructure           | A Kubernetes cluster on any cloud (AWS, Azure, GCP, OCI) or on-premises         |
| Team                     | 1–2 DevOps engineers for initial setup; 1 for ongoing operations                |
| Time to first deployment | \~60 minutes with the one-click installer                                       |
| Domain and SSL           | Required — one domain for the portal, one for the API gateway                   |
| Cloud storage            | Required — for content assets and backups (S3, Azure Blob, GCP Storage, or OCI) |
| Email (SMTP)             | Required — for user registration and notification emails                        |

***

**What Is Included by Default vs. Available as an Add-on?**

| Feature                                           | Default |  Add-on  |
| ------------------------------------------------- | :-----: | :------: |
| Web portal (React)                                |    ✓    |     —    |
| Mobile app (Android)                              |    ✓    |     —    |
| Content creation workspace                        |    ✓    |     —    |
| Course and batch management                       |    ✓    |     —    |
| Certificate issuance and verification             |    ✓    |     —    |
| Built-in admin reports                            |    ✓    |     —    |
| Learner user report                               |    ✓    |     —    |
| Multilingual support (I18N)                       |    ✓    |     —    |
| Full telemetry analytics pipeline (Obsrv)         |    —    | Optional |
| Discussion forums                                 |    —    | Optional |
| Groups / cohort management                        |    —    | Optional |
| QR code / phygital discovery (DIAL)               |    —    | Optional |
| Project and observation management (Manage Learn) |    —    | Optional |

***

