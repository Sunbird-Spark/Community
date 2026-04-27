# Dependencies

{% tabs %}
{% tab title="Sunbird Knowlg" %}
Provides content lifecycle management, collection management, taxonomy, search, and question bank APIs.

| Category     | API endpoint                          | Description                        |
| ------------ | ------------------------------------- | ---------------------------------- |
| Content      | /api/content/v3/read/{id}             | Read content metadata              |
| Collection   | /api/collection/v4/hierarchy/{id}     | Read collection hierarchy          |
| Search       | /api/content/v1/search                | Search content                     |
| Assessment   | /api/questionset/v2/read/{id}         | Read question set                  |
| Taxonomy     | /api/framework/v1/read/{id}           | Read taxonomy framework            |
| Media        | /api/content/v3/upload/{id}           | Upload media asset                 |
| Question     | /api/question/v2/read/{id}            | Read a question                    |
| Question Set | /api/questionset/v2/read/{id}         | Read a question set                |
| Players      | QuestionSet Player (V2 web component) | Render assessments in learner apps |
{% endtab %}

{% tab title="Sunbird Lern" %}
Provides user management, organisation management, course enrolment, progress tracking, and certificate issuance.

| Category     | API endpoint                   | Description             |
| ------------ | ------------------------------ | ----------------------- |
| User         | /api/user/v5/read/{id}         | Read user profile       |
| Org          | /api/org/v2/read               | Read organisation       |
| Enrolment    | /api/course/v1/enrol           | Enrol in a course batch |
| Progress     | /api/content/state/v1/update   | Update lesson progress  |
| Certificate  | /api/certreg/v2/certs/search   | Search certificates     |
| Notification | /api/user/v1/notification/send | Send notification       |
{% endtab %}

{% tab title="Sunbird Obsrv (add-on)" %}
Provides telemetry ingestion and analytics. Only active when the Obsrv add-on is enabled.

| Category  | API endpoint             | Description               |
| --------- | ------------------------ | ------------------------- |
| Telemetry | /api/data/v1/telemetry   | Submit telemetry events   |
| Report    | /api/data/v1/report/list | List available reports    |
| Device    | /api/device/v3/register  | Register a learner device |
{% endtab %}
{% endtabs %}
