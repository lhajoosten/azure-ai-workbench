# Learning roadmap

## How to use this roadmap

Work through one exercise at a time. A milestone can take several sessions. The agent must not execute this document as one autonomous implementation plan.

Use `AGENTS.md` for the session loop and evidence rules. The prediction and variation prompts below are starting points, not an answer key. Adapt difficulty to demonstrated knowledge. Do not pre-generate solutions, future test suites, or cloud resources.

Use [EXAM_COVERAGE.md](EXAM_COVERAGE.md) during formal study to track each published objective. Copy [AZURE_LAB_TEMPLATE.md](AZURE_LAB_TEMPLATE.md) for a cloud exercise, linking its evidence back to the coverage row and learning log.

## Warm-up before December

Allow roughly 8–12 hours for the reduced local foundation, adjusting to actual progress. The exit is one document flowing through one worker, one observed failure, and manual recovery. Automatic crash recovery, competing workers, and extensive reliability testing are deferred to formal study. Do not turn this estimate into a deadline or silently automate unfinished lessons.

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

**Learner owns:** the single-worker state transitions and placement of commits. Introduce short transactions and bounded database connections through observed behavior; leave competing claims to formal study.

**Verify:** stop the worker, submit a document, confirm pending state, then start the single worker and inspect the committed result. Confirm the API can report status during processing.

**Transfer:** explain why an in-process background task does not provide the durability claimed by this workflow. Identify the unresolved crash and multiple-worker scenarios; do not implement automatic recovery in this exercise.

### W3 — Observe one failure and recover manually

**Build:** a recorded processing failure, explicit retry with attempt history, and a repeatable Docker Compose setup for the API, database, and one worker. Reject retry of an active attempt. Document the single-worker limit and the manual procedure for interrupted work. CI can wait until S1.

**Predict:** after an extraction failure, which state and attempt history should a manual retry preserve?

**Learner owns:** one failure experiment, the retry transition, and an explanation of the current limits. Failure injection must be local/test-only and explicit.

**Verify:** deliberately fail a processing attempt, inspect the recorded error, correct its cause, retry manually, and inspect the result and preserved history. Confirm an active attempt cannot be retried and stored documents survive application restarts. Use real PostgreSQL. A crashed attempt may require manual inspection after its worker is stopped; automatic reclamation is deferred.

**Transfer:** diagnose a different processing failure from logs and state, then explain when manual retry is safe and which failure cases remain unsupported.

**Warm-up exit:** the learner can submit and persist a document, process it in one separate worker, inspect status/result, trigger one failure, recover manually, and explain request/transaction/worker lifetimes. No embeddings, paid inference, Azure resources, or production reliability claim are required.

## Formal study extensions

Use the official learning modules alongside these exercises. Readiness comes from covering the objectives, not from finishing the repository alone. The order below can follow the course's sequence when more convenient.

| Milestone | Application exercise | Evidence of understanding |
|---|---|---|
| S1 — Containers | Build/tag/publish images with ACR and ACR Tasks; deploy to App Service and Container Apps; explore revisions and KEDA; compare an AKS deployment in a temporary lab; add CI for existing local checks | Diagnose image-pull, startup, configuration, or connectivity failures; justify a hosting choice and show cleanup |
| S2 — PostgreSQL retrieval | Add pgvector chunk retrieval, metadata filters, index experiments, connection pooling, and resource/performance measurements | Explain a relevance failure, compare a measured query before/after a change, and distinguish index cost from retrieval quality |
| S3 — Cosmos DB | Separate Python SDK lab for queries, vector retrieval, consistency, indexing/RU behavior, and change feed processing | Predict the effect of a query or indexing change, measure it, and explain one consistency tradeoff |
| S4 — Managed Redis | Separate exercises for expiry/invalidation, vector indexing, and messaging; integrate a cache only when its purpose is clear | Reproduce stale data, repair invalidation, and explain the different delivery properties of pub/sub and Streams |
| S5 — Messaging and serverless | Move a processing step to Service Bus; compare queues/topics and Event Grid routing/retries; build a Functions trigger/binding and configure/deploy its function app; explore a Durable Functions workflow | Reproduce duplicate delivery and a delivery failure, inspect the dead-letter path, explain the database-to-message publication gap, and diagnose a deployed function's configuration failure |
| S6 — Security and configuration | Key Vault, App Configuration, scoped identities, configuration refresh/rotation, and protected application access | Reproduce and diagnose a denied operation; rotate a test secret or configuration and explain the identity and permission path |
| S7 — Observability | Trace API, message, worker, database, and optional model calls; write KQL queries and inspect logs/metrics | Find a slow or failed processing step from evidence, identify missing trace propagation, and verify the correction |

These are study exercises based on the [course](https://learn.microsoft.com/en-us/training/courses/ai-200t00) and [exam guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-200), checked on 2026-11-08. Some course exercises extend beyond the guide's explicitly listed bullets. The official guide is the source for exam scope; recheck it at the start of formal study.

### Formal-study reliability exercise — alongside S5

Revisit the warm-up's explicit limits when studying message processing. Investigate worker termination, lease expiry, duplicate delivery, acknowledgement timing, concurrent retries, and stale result publication. Compare a database-backed approach with Service Bus rather than automatically implementing both completely.

**Learner owns:** a prediction about one failure window and a recovery or idempotency decision.

**Verify:** reproduce that failure, inspect committed state and delivery history, implement a bounded correction, and vary the scenario. Use real PostgreSQL or the actual Azure service for the property being claimed. Avoid arbitrary sleeps and claims of exactly-once execution.

This exercise supports engineering understanding. Completing a custom queue, a full outbox framework, or every concurrency scenario is not an exam prerequisite. Keep the experiment bounded and prioritize uncovered official objectives when study time is limited.

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
