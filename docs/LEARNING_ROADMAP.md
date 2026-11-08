# Learning roadmap

## How to use this roadmap

Work through one exercise at a time. A milestone can take several sessions. The agent must not execute this document as one autonomous implementation plan.

Use `AGENTS.md` for the session loop and evidence rules. The prediction and variation prompts below are starting points, not an answer key. Adapt difficulty to demonstrated knowledge. Do not pre-generate solutions, future test suites, or cloud resources.

## Warm-up before December

### W0 — Establish a minimal executable slice

**Build:** agree on the initial layout, dependency management, a minimal FastAPI liveness endpoint, and one HTTP behavior test. Introduce only the tooling needed to run and check this slice. No database, worker, React app, or cloud yet.

**Predict:** what should liveness report if PostgreSQL is unavailable, and why?

**Learner owns:** the endpoint behavior, the test expectation, and an explanation of the process serving it. The agent may demonstrate a familiar packaging step if explicitly delegated.

**Verify:** run the endpoint and test using the commands actually introduced. Change the route behavior and observe the test detecting it before restoring the intended contract.

**Transfer:** explain how readiness will differ from liveness when the database is added.

### W1 — Persist one document

**Build:** PostgreSQL, the first migration, bounded text/Markdown submission, document inspection, and a bounded list. A synchronous processing step is acceptable here if clearly labeled; asynchronous acceptance begins in W2.

**Predict:** which records should remain if the request fails halfway through persistence?

**Learner owns:** a schema decision, the transaction boundary, and one rejection case.

**Verify:** run against PostgreSQL; restart the application and inspect the retained submission. Exercise invalid input and a rolled-back write.

**Transfer:** diagnose a database-connection failure and distinguish it from an application validation error. Explain why a mock-only test would not establish rollback behavior.

### W2 — Separate acceptance from processing

**Build:** committed pending jobs, HTTP 202 acceptance, a separately runnable worker, status inspection, and structured correlation logs. Use a deliberately small database-backed job mechanism locally.

**Predict:** what happens if the API restarts immediately after accepting the document?

**Learner owns:** the state transitions, claim strategy, and placement of commits. Introduce short transactions and bounded database connections through observed behavior.

**Verify:** stop the worker, submit a document, confirm pending state, then start processing. Run two workers against the same pending job and inspect the committed result.

**Transfer:** explain why an in-process background task does not provide the durability claimed by this workflow. Identify what remains unsolved about worker crashes; solve recovery in W3.

### W3 — Recover from failure and reproduce the setup

**Build:** recorded failed attempts, controlled retry, worker-crash recovery, protection against stale result publication, and a repeatable Docker Compose setup. Add CI for the checks now present.

**Predict:** how could a restarted or delayed worker overwrite a newer attempt's successful result?

**Learner owns:** one recovery policy and a failure experiment. Failure injection must be local/test-only and explicit.

**Verify:** exercise a processing failure, conflicting retry, worker termination, lease/recovery behavior, and delayed stale completion using real PostgreSQL. Verify persistence across restarts. Avoid timing assertions based on arbitrary sleeps.

**Transfer:** diagnose an unfamiliar failed attempt using logs and database state, then explain the recovery invariant without relying on the agent's explanation.

**Warm-up exit:** the learner can operate and explain the complete local processing flow. No embeddings, paid inference, or Azure resources are required.

## Formal study extensions

Use the official learning modules alongside these exercises. Readiness comes from covering the objectives, not from finishing the repository alone. The order below can follow the course's sequence when more convenient.

| Milestone | Application exercise | Evidence of understanding |
|---|---|---|
| S1 — Containers | Build/tag/publish images with ACR and ACR Tasks; deploy to App Service and Container Apps; explore revisions and KEDA; compare an AKS deployment in a temporary lab | Diagnose image-pull, startup, configuration, or connectivity failures; justify a hosting choice and show cleanup |
| S2 — PostgreSQL retrieval | Add pgvector chunk retrieval, metadata filters, index experiments, connection pooling, and resource/performance measurements | Explain a relevance failure, compare a measured query before/after a change, and distinguish index cost from retrieval quality |
| S3 — Cosmos DB | Separate Python SDK lab for queries, vector retrieval, consistency, indexing/RU behavior, and change feed processing | Predict the effect of a query or indexing change, measure it, and explain one consistency tradeoff |
| S4 — Managed Redis | Separate exercises for expiry/invalidation, vector indexing, and messaging; integrate a cache only when its purpose is clear | Reproduce stale data, repair invalidation, and explain the different delivery properties of pub/sub and Streams |
| S5 — Messaging and serverless | Move a processing step to Service Bus; compare queues/topics and Event Grid routing/retries; build a Functions trigger/binding; explore a Durable Functions workflow | Reproduce duplicate delivery and a delivery failure, inspect the dead-letter path, and explain the database-to-message publication gap |
| S6 — Security and configuration | Key Vault, App Configuration, scoped identities, configuration refresh/rotation, and protected application access | Reproduce and diagnose a denied operation; rotate a test secret or configuration and explain the identity and permission path |
| S7 — Observability | Trace API, message, worker, database, and optional model calls; write KQL queries and inspect logs/metrics | Find a slow or failed processing step from evidence, identify missing trace propagation, and verify the correction |

These are study exercises based on the [course](https://learn.microsoft.com/en-us/training/courses/ai-200t00) and [exam guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-200), checked on 2026-11-08. Some course exercises extend beyond the guide's explicitly listed bullets. The official guide is the source for exam scope; recheck it at the start of formal study.

## Boundaries for comparison labs

- Keep one main application and one primary datastore. Place alternative service experiments in `labs/<topic>/` when introduced.
- Each lab needs a question, prerequisites, observable success/failure, a comparison with the current design, and teardown instructions.
- Provision one bounded experiment at a time. AKS, Redis, and database resources may continue charging when application traffic stops. Verify current pricing and resource deletion.
- Reuse a known image and corpus where useful. Do not rewrite the application or introduce a framework to make every provider interchangeable.
- Use real service behavior for service-specific evidence. A local emulator or mock is useful practice but must be labeled accordingly.

## December pacing

Suggested allocation, not a booking or commitment:

- **Week 1:** official course and related hands-on exercises; use the warmed-up application for concrete experiments.
- **Week 2:** finish uncovered objectives, focus on weak areas, and practice troubleshooting without a generated solution.
- **Week 3:** practice assessment, targeted review, exam if ready and an appointment is available, plus buffer and rest.

Do not expand the application just to fill the holiday. Keep study sessions bounded and reserve time away from the project. If a service is new, spend the time there rather than polishing a familiar frontend.

## Optional product extension

After the backend lessons, add a small React/TypeScript inbox with upload, status, result, and retry views. Learn the actual backend contract before generating a client. UI work is useful but does not replace Azure study coverage.
