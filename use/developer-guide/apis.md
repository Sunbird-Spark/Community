# APIs

#### API reference

Sunbird Spark's API surface is available as a Postman workspace. The workspace includes all APIs for both knowlg-service and lern-service, organised by domain.

[Open in Postman →](https://www.postman.com/project-sunbird)

#### Core API endpoints

{% tabs %}
{% tab title="knowlg-service" %}
**knowlg-service** — content, collections, search, assessments

| Endpoint | Method | Description |
|---|---|---|
| /api/content/v3/read/{do_id} | GET | Read content metadata |
| /api/content/v3/create | POST | Create new content |
| /api/content/v3/publish/{do_id} | POST | Publish content |
| /api/collection/v4/read/{do_id} | GET | Read collection hierarchy |
| /api/collection/v4/create | POST | Create a collection (course, playlist, textbook) |
| /api/content/v1/search | POST | Search content and collections |
| /api/data/v1/dial/assemble | POST | Resolve a DIAL/QR code to linked content |
| /api/questionset/v2/read/{qsId} | GET | Read a question set |
| /api/question/v2/list | POST | List questions from the question bank |
{% endtab %}

{% tab title="lern-service" %}
**lern-service** — users, organisations, enrolment, batches, certificates

| Endpoint | Method | Description |
|---|---|---|
| /api/user/v5/read/{userId} | GET | Read user profile |
| /api/user/v3/create | POST | Create a user |
| /api/user/v2/update | PATCH | Update user profile |
| /api/org/v2/read | GET | Read organisation details |
| /api/course/v1/enrol | POST | Enrol a learner in a course batch |
| /api/course/v1/unenrol | POST | Remove a learner from a course batch |
| /api/content/state/v1/update | PATCH | Update content consumption progress |
| /api/content/state/v1/read | POST | Read content consumption progress |
| /api/course/v1/batch/read/{batchId} | GET | Read batch details |
| /api/certreg/v2/certs/search | POST | Search for a certificate by user or course |
| /api/certreg/v2/certs/download/{certId} | GET | Download a certificate |
| /api/data/v1/telemetry | POST | Submit telemetry events |
{% endtab %}

{% tab title="Form service" %}
**Form service** — dynamic UI configuration

The Form Service is owned and maintained by the Sunbird Spark team. It stores dynamic UI configuration.

| Endpoint | Method | Description |
|---|---|---|
| /api/data/v1/form/read | POST | Read a form configuration by type, subtype, action |
| /api/data/v1/form/create | POST | Create a new form configuration |
| /api/data/v1/form/update | PATCH | Update an existing form configuration |

Full Form API documentation with examples: [Postman — Sunbird-ED Forms →](https://documenter.getpostman.com/view/25186239/2s946pXoZ2)
{% endtab %}
{% endtabs %}
