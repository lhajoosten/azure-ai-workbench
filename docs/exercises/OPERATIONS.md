# Security and observability exercises — S6, S7

Follow [the session rules](README.md#session-rules), the agreed cloud scope, and current official modules in [the coverage checklist](../EXAM_COVERAGE.md). Use fake secret values and synthetic documents. Sanitize evidence; never put credentials, raw traces, or cloud account identifiers into the repository.

## O01

**Assignment: retrieve and rotate a secret safely.**

**Start:** an approved Key Vault and an application identity with narrowly scoped test access. Use a synthetic secret unrelated to any real credential. Distinguish the identity used locally from the deployed workload identity.

**Predict:** after a new secret version is created, when will your application use it, and what determines that behavior?

**Tasks:**

1. Retrieve the test secret through the SDK and verify successful use without printing its value.
2. Rotate it and observe version/cache/reference behavior using safe metadata or a non-reversible comparison result.
3. Under the agreed test scope, reproduce a denied retrieval with an identity that lacks permission; diagnose without granting broad access as a shortcut.

**Done:** explain identity, access, retrieval, version selection, and the observed rotation delay or refresh behavior. No secret appears in source, logs, or evidence.

**Transfer:** explain a version-pinned versus current-version reference and test one controlled difference. Remove lab-owned secrets/resources as planned.

## O02

**Assignment: change configuration without changing the image.**

**Start:** approved App Configuration access and an app with a harmless setting such as a display label. Keep environment-specific labels separate from secret values.

**Predict:** if a remote setting changes, will your running application see it immediately, on refresh, or after restart?

**Tasks:**

1. Store a setting with two environment labels; retrieve the intended variant from application code.
2. Change one variant and observe the application's configured refresh/restart behavior.
3. Deliberately select a missing or wrong label; diagnose the outcome and define safe handling rather than masking it with an unexplained default.

**Done:** show settings are selected as intended and explain when changes become visible. Keep the image unchanged for the successful configuration-only change.

**Transfer:** justify which settings belong here versus Key Vault, then investigate a supported Key Vault reference if within scope. A configuration reference does not automatically grant secret access.

## O03

**Assignment: follow one document across boundaries.**

**Start:** a deployed API and worker, preferably I01's messaging slice. Agree on telemetry ingestion, retention, sampling, and cost. Inspect supported current OpenTelemetry/Azure integration for the chosen runtimes.

**Predict:** which observations would prove that an API request and worker operation belong to the same processing flow?

**Tasks:**

1. Instrument one request and its downstream job/message/worker path; submit a synthetic document and inspect related spans.
2. Add one controlled processing delay and locate it from the trace.
3. Deliberately break context propagation at one boundary; inspect the resulting trace discontinuity before correcting it.

**Done:** explain parent/context propagation across processes, distinguish identifiers in logs from actual trace continuity, and show the correction. Do not include document contents or secrets in attributes.

**Transfer:** locate a differently placed delay without being told where it was injected. Record sampling or asynchronous-link limitations honestly.

## O04

**Assignment: answer an operational question with KQL.**

**Start:** O03 telemetry and a controlled dataset: a known number of successful jobs, one deliberate failure, and one delayed operation. Inspect the actual available tables/fields; do not assume a copied query matches the workspace schema.

**Predict:** which records and time window should answer "Which jobs failed, and where was time spent?"

**Tasks:**

1. Write a query selecting the relevant time window and failure records; connect results to known submitted jobs.
2. Aggregate an appropriate duration or metric by operation, documenting units and whether samples are missing.
3. Change the window or grouping and explain why results differ. Distinguish logs, metrics, and span data where relevant.

**Done:** results are checked against the controlled run; explain unexpected counts or missing records using ingestion delay, sampling, or query scope rather than inventing data.

**Transfer:** answer a new question, such as which dependency contributed most latency, without copying the prior query unchanged. Clean up retained telemetry resources according to the lab plan.
