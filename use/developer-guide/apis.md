# APIs

#### API reference

Sunbird Spark's API surface is available as a Postman workspace. The workspace includes all APIs for both knowlg-service and lern-service, organised by domain.

[Open in Postman →](https://www.postman.com/project-sunbird)

#### Core API endpoints

#### knowlg-service (content, collections, search, assessments)

<table><thead><tr><th width="283.05078125">Endpoint</th><th width="111.5703125">Method</th><th>Description</th></tr></thead><tbody><tr><td>/api/content/v3/read/{do_id}</td><td>GET</td><td>Read content metadata</td></tr><tr><td>/api/content/v3/create</td><td>POST</td><td>Create new content</td></tr><tr><td>/api/content/v3/publish/{do_id}</td><td>POST</td><td>Publish content</td></tr><tr><td>/api/collection/v4/read/{do_id}</td><td>GET</td><td>Read collection hierarchy</td></tr><tr><td>/api/collection/v4/create</td><td>POST</td><td>Create a collection (course, playlist, textbook)</td></tr><tr><td>/api/content/v1/search</td><td>POST</td><td>Search content and collections</td></tr><tr><td>/api/data/v1/dial/assemble</td><td>POST</td><td>Resolve a DIAL/QR code to linked content</td></tr><tr><td>/api/questionset/v2/read/{qsId}</td><td>GET</td><td>Read a question set</td></tr><tr><td>/api/question/v2/list</td><td>POST</td><td>List questions from the question bank</td></tr></tbody></table>

#### lern-service (users, organisations, enrolment, batches, certificates)

<table><thead><tr><th width="324.89453125">Endpoint</th><th width="103.140625">Method</th><th></th></tr></thead><tbody><tr><td>/api/user/v5/read/{userId}</td><td>GET</td><td>Read user profile</td></tr><tr><td>/api/user/v3/create</td><td>POST</td><td>Create a user</td></tr><tr><td>/api/user/v2/update</td><td>PATCH</td><td>Update user profile</td></tr><tr><td>/api/org/v2/read</td><td>GET</td><td>Read organisation details</td></tr><tr><td>/api/course/v1/enrol</td><td>POST</td><td>Enrol a learner in a course batch</td></tr><tr><td>/api/course/v1/unenrol</td><td>POST</td><td>Remove a learner from a course batch</td></tr><tr><td>/api/content/state/v1/update</td><td>PATCH</td><td>Update content consumption progress</td></tr><tr><td>/api/content/state/v1/read</td><td>POST</td><td>Read content consumption progress</td></tr><tr><td>/api/course/v1/batch/read/{batchId}</td><td>GET</td><td>Read batch details</td></tr><tr><td>/api/certreg/v2/certs/search</td><td>POST</td><td>Search for a certificate by user or course</td></tr><tr><td>/api/certreg/v2/certs/download/{certId}</td><td>GET</td><td>Download a certificate</td></tr><tr><td>/api/data/v1/telemetry</td><td>POST</td><td>Submit telemetry events</td></tr></tbody></table>

#### Form service APIs

The Form Service is owned and maintained by the Sunbird Spark team. It stores dynamic UI configuration.

| Endpoint                 | Method | Description                                        |
| ------------------------ | ------ | -------------------------------------------------- |
| /api/data/v1/form/read   | POST   | Read a form configuration by type, subtype, action |
| /api/data/v1/form/create | POST   | Create a new form configuration                    |
| /api/data/v1/form/update | PATCH  | Update an existing form configuration              |

Full Form API documentation with examples: [Postman — Sunbird-ED Forms →](https://documenter.getpostman.com/view/25186239/2s946pXoZ2)

