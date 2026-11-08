# Local warm-up exercises

Follow [the session rules](README.md#session-rules). These six assignments cover W0–W3. Around 8–12 hours is an estimate for the reduced foundation; split work into small sessions and carry unfinished work into study. Do not add automatic crash recovery or multiple workers.

## W0-01

**Assignment: make one API behavior observable. Milestone W0.**

**Start:** a documentation-only repository. Inspect installed tools and agree on a minimal backend layout. No database is needed yet.

**Predict:** if PostgreSQL is unreachable but the API process is running, what should its liveness endpoint report?

**Tasks:**

1. Introduce the minimal Python/FastAPI environment and only the tooling needed for this slice. Identify which setup steps you already understand before delegating them.
2. Implement `GET /health/live` with a documented response contract.
3. Write one HTTP behavior test; deliberately violate that contract and observe the test failing, then restore the intended behavior.

**Done:** the endpoint runs locally; the test detects the deliberate change and then passes. Record the actual commands. Explain which process handled the request and what the test establishes.

**Transfer:** describe a separate readiness check and a failure it would detect that liveness would not. Do not implement a database connection until W1-01.

## W1-01

**Assignment: persist and inspect a document. Milestone W1.**

**Start:** W0-01 works. Start a local PostgreSQL instance and introduce a migration for the smallest useful document schema. Agree on the database connection settings without committing secrets.

**Predict:** what should remain if a document write fails before its transaction commits?

**Tasks:**

1. Create the three fixtures described in [the corpus](README.md#shared-synthetic-corpus).
2. Implement text-document submission and inspection using real PostgreSQL. Persist original content separately from any derived result; processing may be synchronous at this stage.
3. Exercise a deliberate transaction failure, restart the API, and inspect accepted documents.
4. Introduce `GET /health/ready` and observe readiness while PostgreSQL is stopped, alongside liveness.

**Done:** accepted content survives an API restart; the failed write leaves no partial document; unknown IDs have a documented error; readiness reflects the dependency failure. Show database-backed evidence rather than a mocked session.

**Transfer:** propose and apply one small metadata/schema change through a migration, preserving previously accepted documents.

## W1-02

**Assignment: reject bad input and bound a list. Milestone W1.**

**Start:** W1-01 works. Adopt the brief's proposed 1 MiB file limit or record an explicit alternative before testing.

**Predict:** should a filename ending in `.md` be enough to accept a file whose bytes are not valid UTF-8?

**Tasks:**

1. Handle empty content, an unsupported extension, malformed UTF-8, and a payload above the agreed limit.
2. Choose documented client-error responses and verify that rejected submissions create no document records.
3. Implement `GET /documents` with a bounded page size and deterministic ordering. Use a fixture set larger than one page.

**Done:** each invalid case is observable through the API, leaves no partial data, and returns a safe error. List results stay within the requested maximum; advancing through a static dataset neither omits nor repeats entries.

**Transfer:** test a file exactly at the limit and explain whether your implementation accepts it. Change the page size and verify ordering remains coherent. Do not introduce full-text search or a UI.

## W2-01

**Assignment: process work outside the request. Milestone W2.**

**Start:** W1 works. Use one API process, one PostgreSQL database, and at most one worker. Pick deterministic processing such as extracting a title and word count.

**Predict:** after a successful acceptance response, what happens to the submission if the API restarts before processing begins?

**Tasks:**

1. Record the document and pending job durably, then return HTTP 202 with their IDs. Observe a failed-write case that would otherwise leave only one record.
2. Add a separately runnable worker, visible state transitions, and `GET /jobs/{id}`.
3. Stop the worker, submit a fixture, restart the API, inspect pending state, then start the worker and inspect the result. Introduce controlled local delay to observe status while processing.

**Done:** acceptance depends on committed state; pending work survives the API restart; status requests remain responsive during processing. Logs correlate submission, job, and result without logging document bodies.

**Transfer:** explain what a worker crash still leaves unresolved. Record that limitation; do not solve leases, competing claims, or automatic recovery in this assignment.

## W3-01

**Assignment: diagnose a failed attempt and retry manually. Milestone W3.**

**Start:** W2-01 works with one worker. Agree on a reversible, local/test-only way to make processing fail. Do not add a public fault-injection endpoint.

**Predict:** when a failed job is retried, which information must remain visible from its earlier attempt?

**Tasks:**

1. Trigger the failure and inspect status, safe error details, and correlated logs.
2. Correct the cause and add an explicit retry operation that preserves attempt history.
3. Retry, inspect the successful result, and verify that retrying an active attempt is rejected.

**Done:** failure is observable rather than silently converted into success; manual retry produces a traceable new attempt; original content and previous error history remain inspectable.

**Transfer:** have the agent vary the controlled failure, disclose only the symptom, and let you diagnose it before seeing its cause. For interrupted jobs, explain why manual reset requires ensuring the original worker has stopped. Automatic crash recovery is deferred.

## W3-02

**Assignment: reproduce the local flow with containers. Milestone W3.**

**Start:** the API and worker work separately. PostgreSQL may already use a container; now make the complete local setup reproducible.

**Predict:** which addresses should the API use to reach PostgreSQL from inside a container versus from your host?

**Tasks:**

1. Containerize the backend and define local API, single-worker, and database services with explicit configuration and persistent development storage.
2. Document the actual build, migration, startup, observation, and safe shutdown commands. Separate application startup from schema migration as appropriate for this small setup.
3. Submit a fixture, restart application containers, and confirm its content and history remain. Diagnose one deliberately incorrect database address.

**Done:** a clean terminal session can follow your instructions to run the flow; restart preserves data; logs explain the bad-address failure; secrets and generated data are absent from the diff.

**Transfer:** change an application setting and show how it reaches the container. Explain the difference between rebuilding an image and restarting with different configuration. Stop here: CI, Azure, and advanced recovery belong to formal study.
