# Project brief

## Purpose and success

Build a professional, deliberately small document-processing backend that provides a practical setting for AI-200 study. The learner should finish able to justify service choices, implement a change without copying a completed solution, and diagnose failures using evidence.

There are two deliverables: a working application and recorded demonstrations of understanding. The application alone does not establish completion of the learning goals.

## Product scenario

A single user submits synthetic documents to an inbox. The system durably records each submission, processes it outside the request, exposes job progress and extracted text, and supports deliberate recovery experiments. Semantic retrieval is added after the processing foundation works.

Example document: a fictional supplier's maintenance agreement describing service hours, renewal terms, and contact procedures. Later, a query such as "Which agreements renew automatically?" should return relevant source passages with document and chunk identifiers.

Start with deterministic text extraction and metadata. There is no requirement for an LLM, invoice recognition, OCR, or generated answers in the warm-up. Embeddings and optional generated answers come later and have separate validation requirements.

## Scope by stage

| Stage | Required outcome | Deferred |
|---|---|---|
| Before formal study | Local API, persistence, separate worker, observable job states, failure/retry exercise, repeatable local setup | Azure, paid AI calls, PDF/OCR, UI polish |
| During study | Cloud deployment and focused exercises for each exam area | A production platform or exhaustive implementation of every Azure feature |
| After study, optional | Small React inbox, richer extraction, deployment automation, stronger operational controls | Multi-tenancy, ERP integrations, real business documents |

Suggested warm-up budget: 8–12 hours. This is a planning estimate, not a deadline. If worker recovery takes longer, carry it into formal study instead of having the agent complete it invisibly.

## Functional requirements

### Submission and inspection

- Accept bounded UTF-8 `.txt` and `.md` files. Propose a 1 MiB initial limit; validate content as well as the filename. Reject unsupported, empty, malformed, or oversized input with a useful error.
- Persist accepted content and metadata, then return document and job identifiers. In the asynchronous version, return HTTP 202 only after the submission and pending job are committed.
- For the local warm-up, store the bounded text in PostgreSQL to avoid introducing file-storage consistency before its lesson. Blob Storage can be a later extension, not a hidden prerequisite.
- List documents and inspect a document, its latest processing result, and its job attempts. Bound list responses; introduce pagination with the list exercise.
- Keep original content separate from derived extraction results. Do not present unprocessed content as a completed result.

### Processing lifecycle

- Use `pending`, `processing`, `succeeded`, and `failed` for job state. Track attempt number, timestamps, and a safe error summary. A document and its processing attempt have different identities.
- Run processing in a separate process. The API must remain able to answer status requests while a job runs.
- Define legal transitions and retry behavior with the learner before implementation. Retrying a failed job should preserve prior attempt history and reject conflicting retries.
- A second worker must not corrupt or duplicate a published result. A crashed worker must not leave work stuck forever. Design and test the claiming, recovery, and stale-worker behavior during W2/W3.
- Temporary local simplifications must be recorded with their limits. Do not claim exactly-once execution or production readiness from an idempotent result alone.

### Retrieval, later

- Add chunking, embedding generation, and pgvector similarity search as a separate milestone. Preserve source identifiers and embedding model/version/dimension metadata.
- Use a small synthetic reference corpus with relevance expectations and negative examples. Return source passages; an optional answer generator must cite them and handle insufficient evidence.
- Compare retrieval results and latency before claiming an optimization. Synthetic deterministic vectors can test plumbing but do not prove semantic retrieval quality.

## Proposed API surface

These routes express desired behavior; finalize their contracts during the relevant lesson rather than generating them all at once.

| Route | Purpose | Introduce |
|---|---|---|
| `GET /health/live` | Process liveness | W0 |
| `GET /health/ready` | Required dependency readiness | W1 |
| `POST /documents` | Validate and accept a document | W1; asynchronous in W2 |
| `GET /documents` | Bounded document list | W1 |
| `GET /documents/{id}` | Metadata and available result | W1 |
| `GET /jobs/{id}` | State and attempt history | W2 |
| `POST /jobs/{id}/retry` | Explicit recovery of a failed job | W3 |
| `POST /search` | Retrieve source passages | S2 |

Specify response schemas, missing-resource behavior, and state conflicts in the current exercise. Do not build speculative versioning or a generic workflow engine.

## Architecture and approach

Use one backend codebase with separately runnable API and worker entry points, one PostgreSQL instance, and Docker Compose locally. The warm-up may use a PostgreSQL-backed pending-job mechanism. Investigate atomic claims, short transactions, lease/recovery semantics, and safe result publication; these are learning tasks, not a supplied algorithm.

During Azure study, introduce Service Bus in a bounded exercise. Explicitly examine the failure between committing a submission and publishing its message, duplicate deliveries, and acknowledgement timing. Choose a demonstrated consistency strategy before claiming reliable end-to-end delivery. An outbox is an option to evaluate, not boilerplate to generate on day one.

Keep a clear dependency direction without generating a framework: HTTP parses requests, application code coordinates processing, persistence owns database interaction, and the worker owns execution. Do not create an interface for every class or start with microservices.

Proposed future directories, created only when needed: `backend/`, `frontend/`, `infra/`, and `labs/`. Keep this repository's root `AGENTS.md` authoritative for the learning workflow.

Three approaches were considered: disconnected service demos, one application using every service simultaneously, and one small application with focused comparison labs. This brief proposes the third: it provides continuity while allowing independent experiments and manageable cost. Revisit that choice if a lesson cannot be explored cleanly within it.

## Quality and operational requirements

- Dependency versions and commands become documented when introduced, with lockfiles. Add CI for actual checks once an executable slice exists.
- Verify happy paths and the relevant failure path per exercise. Include real database tests for persistence and concurrency. Avoid tests that merely assert helper calls.
- Database state survives application restarts; schema changes use migrations. Separate disposable test databases from development data.
- Add structured logs with request/job correlation early. During observability study, extend correlation across processes and services with OpenTelemetry.
- Document timeouts, cancellation, retry limits, and shutdown behavior as those capabilities are introduced. No long database transaction around network or model calls.
- Retain synthetic fixtures only. Apply payload limits and safe errors. Establish authentication/authorization and secure secret handling before exposing the service beyond localhost.
- Before cloud work, record resources, estimated cost basis, allowed budget, region, access, and teardown steps. Verify deletion and remaining billable resources after each comparison lab.

## Completion evidence

The learner can submit and inspect a document, explain the transaction and worker lifetimes, reproduce a failure, recover safely, and show evidence for the outcome. Later milestones add deployment, retrieval, messaging, and observability demonstrations.

Each milestone has an implementation result and a separate learning assessment in the log. Do not substitute application completion for coverage of the official course and exam objectives.
