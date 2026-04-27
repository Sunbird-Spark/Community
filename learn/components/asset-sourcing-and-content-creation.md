# Asset sourcing and content creation

Spark supports two pathways for bringing content onto the platform.

### Direct content creation

Content creators log into the web portal workspace and create content directly:

* **Interactive content** created using the ECML Editor, with drag-and-drop authoring and support for simulations, games, and branching scenarios
* **Collections** created using the Collection Editor to organise content into courses, playlists, or textbooks
* **Uploads** for PDF, video (`MP4` and `WebM`), `H5P`, `EPUB`, and audio files

All content goes through a configurable review and publish workflow before it becomes visible to learners.

### Crowdsourcing via CoKreat

For large-scale content programmes, such as textbook digitisation or national skills libraries, Spark supports the CoKreat crowdsourcing workflow:

{% stepper %}
{% step %}
#### **A programme administrator creates a project**

A programme administrator creates a project in CoKreat and defines the content that needs to be created.
{% endstep %}

{% step %}
#### Contributors upload content

Contributors, such as individuals, NGOs, and subject-matter experts, nominate themselves and upload content against defined slots.
{% endstep %}

{% step %}
#### Administrators review and approve contributions

Administrators review and approve contributions.
{% endstep %}

{% step %}
#### Approved content is synced into Spark

Approved content is automatically synced into the main Spark platform via the publish pipeline.
{% endstep %}
{% endstepper %}

CoKreat runs as a separate deployment and connects to Spark through the content publish pipeline.
