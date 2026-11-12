# Sunbird Spark v1.1.2

## Sunbird Spark v1.1.2

**Released:** November 2026\
**Release type:** Patch — backward compatible with v1.1.1\
**Installer tag:** spark-v1.1.2

v1.1.2 upgrades cleanly from v1.1.1. This release delivers the first production workflow on the AI Pipeline Framework designed in v1.1.1: automatic video transcripts with multi-language translation. It also introduces Dynamic Assessment, which builds assessments from a question pool so each learner gets their own set of questions. It fixes cloud security (CSPM) findings across the installer's infrastructure and resolves critical and high-severity vulnerability alerts across the platform. Follow the Upgrade Guide below.

> 🔐 **Security Hardening:** With this release, Sunbird Spark fixes the cloud security (CSPM) findings on its Azure infrastructure and resolves critical and high-severity Dependabot and code-scanning alerts across the knowledge platform.

### Overview

1. **AI Pipeline — Transcripts & Translation**\
   The first workflow built on the AI Pipeline Framework: videos get transcripts and captions automatically on publish, translated into every configured language.
2. **Dynamic Assessment**\
   Two new content types build an assessment from a question pool by skill and difficulty, so different learners get partly different questions.
3. **Infrastructure Security — CSPM Findings**\
   Cloud security findings are fixed across the installer's Azure infrastructure, with secure defaults for access and deployment workflows.
4. **Platform Security — Vulnerability Remediation**\
   Critical and high-severity Dependabot and code-scanning alerts are resolved in the knowledge platform services and Flink jobs.

### 1. AI Pipeline — Transcripts & Translation

v1.1.1 delivered the technical design for the AI Pipeline Framework. v1.1.2 delivers its first production workflow: turning a published video into transcripts and captions in multiple languages.

**1.1 Transcript Workflow**\
When a creator requests a transcript for a video, the workflow starts automatically when the content is published. Whisper speech-to-text detects the spoken language and creates the transcript. The transcript is then translated into every configured language, and English is always included, so non-English videos also get an English transcript. Each transcript is saved as a transcript file and as WebVTT captions, so the player can show captions. Long videos are processed in smaller chunks to keep memory use low; a 67-minute video completes in about 9 minutes.

**1.2 Portal & Editor**\
Creators can request and view transcripts in the editor. The workspace has a new Transcripts filter, and a caption badge shows on content that has a Live transcript.

**1.3 Pipeline Engine & Enrichment APIs**\
The earlier PyFlink jobs (enrichment-router and caption-generator) are replaced by a durable, workflow-based AI pipeline built on Restate. Workflows resume after a failure and don't redo work that is already finished. The pipeline is installed as its own optional component. A new generic set of Enrichment APIs creates, lists, updates, approves and rejects AI outputs such as transcripts, so new output types, such as summaries, can be added later without new APIs.

### 2. Dynamic Assessment

Fixed assessments give every learner the same questions. Dynamic Assessment builds the assessment from a pool of questions chosen by skill and difficulty.

**2.1 New Content Types**\
Dynamic Course Assessment (ECML) and Dynamic QuestionSet build an assessment from a question pool instead of a fixed list. The creator picks the skills and how many easy, medium and difficult questions each learner gets.

**2.2 Backend Generation**\
New refresh APIs in knowlg-service start a knowlg-publish job. The job picks Live questions tagged with the chosen skills and difficulty levels, and builds a pool twice the size of the target. Changes to a published assessment are made on a draft copy, so learners keep the live version until the change is published again.

**2.3 Editor Experience**\
Both editors have Save & Generate and Regenerate. They block saving when there aren't enough questions for a skill, and show the minimum needed per skill. A new Difficulty picker lets questions created in the editor join the pool.

**2.4 Course Support**\
The collection editor now lists both Dynamic types in its Library, so they can be added to a course.

**2.5 Learner Experience**\
Learners get the number of questions the creator set, picked from a pool twice that size. A learner always gets the same questions, but different learners get partly different sets. Creators and reviewers can see the whole pool.

### 3. Infrastructure Security — CSPM Findings

Cloud security posture (CSPM) scans flagged findings in the infrastructure the installer creates. v1.1.2 fixes them so new environments are secure by default.

**3.1 Azure**\
SSH access to the installer runner VM is limited to admin IPs instead of the whole internet. The runner VM gets automatic OS security patches, and public access to its OS disk is blocked. AKS audit logs and the subscription Activity Log can be sent to Log Analytics; this is optional and off by default to avoid extra cost.

**3.2 CI/CD & Code Quality**\
Deployment workflows install tools only after verifying their checksums. GitHub Actions are pinned to exact versions, and each job gets only the permissions it needs. SonarCloud findings in the installer scripts, Helm charts and Keycloak login theme are fixed, vulnerable dependencies flagged by Dependabot are updated, and the unused NLWeb components are removed.

### 4. Platform Security — Vulnerability Remediation

Critical and high-severity Dependabot and code-scanning alerts are resolved in knowledge-platform and knowledge-platform-jobs. This includes updates to Netty, Jackson, lz4, Logback, commons-configuration2 and htmlunit, and the removal of end-of-life Jetty. Errors are logged through the platform logger instead of being printed to the console, and secure random numbers are used. Container images install only required packages, and sync-tool runs as a non-root user.

### Bug Fixes

| Area               | Fix                                                                                        |
| ------------------ | ------------------------------------------------------------------------------------------ |
| Knowledge Platform | Deleted draft documents no longer reappear in search results after a publish               |
| Knowledge Platform | Cloud storage endpoint is passed correctly to the storage SDK                              |
| Knowledge Platform | Text fields are no longer read back as lists from the graph database                       |
| Knowledge Platform | QuestionSet add/remove no longer fails for new question sets                               |
| Collection Editor  | ECML content preview fixed                                                                 |
| Portal             | Workspace cards no longer show stale entries when a filter is toggled                      |
| AI Pipeline        | Transcription of long videos no longer runs out of memory                                  |
| DevOps             | knowlg-publish now uses the configured content-published topic instead of a silent default |

### Upgrade Guide

#### Upgrading from v1.1.1

v1.1.2 is a patch release and upgrades in place from v1.1.1.

**Step 1 — Deploy**\
Deploy the services as per the installer guide using the spark-v1.1.2 tag.

**Step 2 — Reindex JanusGraph**\
v1.1.2 uses more JanusGraph indexes when it validates content. If your graph has data created before these indexes were added, the indexes do not include that data, and content creation fails with `ERR_EMPTY_EDGE_PROPERTY_LIST`. A reindex adds the existing data to the indexes. It does not change or delete any data, and it is safe to run more than once. New installations can skip this step.

1. Open the Gremlin console where JanusGraph is installed.

On Kubernetes:

```
kubectl exec -it -n sunbird deploy/janusgraph -- /opt/bitnami/janusgraph/bin/gremlin.sh
```

On a standalone server:

```
cd <janusgraph-install-dir>
bin/gremlin.sh
```

2. Connect to the graph using the same properties file as your JanusGraph server. On Kubernetes it is `/opt/bitnami/janusgraph/conf/janusgraph.properties`; on a standalone server it is usually `<janusgraph-install-dir>/conf/janusgraph.properties`.

```
graph = JanusGraphFactory.open('<path-to>/janusgraph.properties')
```

3. Check whether a reindex is needed. Run both counts. If the second count is 0 or lower than the first, continue to the next step; if they match, no reindex is needed.

```
g = graph.traversal()
g.V().has('IL_FUNC_OBJECT_TYPE','License').count()
g.V().has('graphId','domain').has('IL_FUNC_OBJECT_TYPE','License').has('IL_SYS_NODE_TYPE','DATA_NODE').has('status','Live').count()
graph.tx().rollback()
```

4. Reindex all enabled indexes. Each index prints `done` when finished. Small graphs take a few seconds per index; large graphs take longer.

```
graph.openManagement().getGraphIndexes(Vertex.class).collect { it.name() }.each { n ->
  m = graph.openManagement(); idx = m.getGraphIndex(n)
  if (idx.getIndexStatus(idx.getFieldKeys()[0]).toString() == 'ENABLED') {
    m.updateIndex(idx, SchemaAction.REINDEX).get(); m.commit(); println "done ${n}"
  } else { m.rollback(); println "skip ${n}" }
}
```

5. Verify and close. Run the counts from step 3 again; both should now match. Then close the console.

```
graph.close()
:exit
```

**Step 3 — Add new categories and forms**\
From the [sunbird-spark-collection-v1.json](https://github.com/Sunbird-Spark/sunbird-spark-installer/blob/v1.1.2/postman-collection/sunbird-spark-collection-v1.json) Postman collection, run the following APIs in this order.

In the **2 - Knowlg** folder:

1. ObjectCategory - Dynamic QuestionSet
2. ObjectCategory - Dynamic Course Assessment
3. ObjectCategory - Transcript
4. ObjectCategoryDefinition - Dynamic QuestionSet
5. ObjectCategoryDefinition - Dynamic Course Assessment
6. ObjectCategoryDefinition - Transcript

In the **3 - Forms** folder:

1. 11 - Dynamic Course Assessment Save
2. 12 - Dynamic Course Assessment Review

**Step 4 — Enable the AI pipeline (optional)**\
To use transcripts, set `ai_pipeline_enabled: "true"` in `global-values.yaml`, provide the model API key, and run `./install.sh install_component ai-pipeline`.

**Link to Release Tag:** [spark-v1.1.2](https://github.com/Sunbird-Spark/sunbird-spark-installer/releases/tag/spark-v1.1.2)
