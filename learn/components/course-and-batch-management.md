# Course and batch management

### Creating a course

A course in Spark is a **trackable collection**. It is a structured set of content units, or lessons, organised into a hierarchy and associated with one or more batches.

{% stepper %}
{% step %}
#### A content creator builds the course structure

A content creator builds the course structure in the Collection Editor and adds lessons and assessments.
{% endstep %}

{% step %}
#### The creator submits the course for review

The creator submits the course for review.
{% endstep %}

{% step %}
#### A content reviewer approves and publishes the course

A content reviewer approves and publishes the course.
{% endstep %}

{% step %}
#### A course mentor creates batches

A course mentor creates one or more batches for the course and defines enrolment windows and certificate rules.
{% endstep %}
{% endstepper %}

### Batch management

Each course batch is an independent enrolment and tracking context:

* Learners enrol in a specific batch, not just a course
* Progress is tracked per batch
* Certificate rules are defined per batch
* A course can run multiple simultaneous or sequential batches for different cohorts, regions, or time periods

### Progress tracking

Progress is tracked at the individual lesson level:

* Lesson completion is recorded when a learner reaches the end of a content item
* Course-level completion is calculated as the percentage of lessons completed
* Certificate issuance is triggered when a learner's completion percentage meets the batch threshold, typically `100%`
* If an assessment is part of the course, its score can also be used as a certificate criterion
