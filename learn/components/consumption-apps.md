# Consumption apps

Spark provides two learner applications that work across devices and connectivity conditions, both sharing a common design system. They can be white-labelled and themed to match your organisation's brand.

{% tabs %}
{% tab title="Web portal" %}
The Spark web portal is built on React JS.

**It provides:**

* A personalised home dashboard for logged-in learners, showing enrolled courses, progress, and recommendations
* Full anonymous browsing, so visitors can discover and consume public content without creating an account
* A role-aware navigation sidebar that adapts to the user's assigned roles, such as learner, content creator, reviewer, and administrator
* A content creation workspace for creators and reviewers
* Admin reports and user management for administrators
* Multilingual support via a persistent language switcher available on every page
{% endtab %}

{% tab title="Mobile app" %}
The Spark mobile app is built on Ionic + React + Capacitor. It is available for Android.

**Key capabilities:**

* Full offline learning. Learners can download courses and content, then consume them without an internet connection. Progress and telemetry sync when connectivity returns.
* Push notifications for course reminders, batch updates, and programme announcements
* The same content players as the web portal, ensuring a consistent experience across devices
{% endtab %}
{% endtabs %}

### Common capabilities

Both apps share the following:

| Capability | Detail |
|---|---|
| Content players | All V2 web component players (PDF, Video, H5P, ECML, EPUB, Audio) |
| Telemetry | All user interactions emit structured telemetry events automatically |
| White-labelling | Logo, colours, and layout configurable at the channel (tenant) level |
| Multi-tenancy | Each app supports multiple organisations via channel-based configuration |
