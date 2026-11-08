# AI-200 exam-coverage checklist

## Source and use

Reviewed on **2026-11-08** against the [official AI-200 study guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-200), whose displayed update date is 2026-10-30. The 27 rows below correspond to its current objective bullets. IDs and short labels are local tracking aids, not Microsoft's identifiers or a substitute for the full guide.

The official guide determines exam scope; the linked learning paths supply study material. Exercises and evidence suggestions here are our project design. Reconcile this checklist with the guide at the start of formal study and before booking: add new objectives, retain prior evidence, and identify changed coverage. Do not assume these rows describe every possible exam question.

For each row, study the relevant module, attempt a bounded exercise, then link to actual evidence in [the learning log](LEARNING_LOG.md) or a completed lab. An official course exercise can supply that evidence; rebuilding it inside this application is not mandatory. Local-only practice does not establish Azure-specific behavior.

Progress uses the learning statuses from `AGENTS.md`: **N** = not assessed; **H** = practiced with help; **D** = demonstrated; **R** = revisit. All rows start at N. Replace the dash with an evidence link after an actual attempt. Keep implementation status and detailed reasoning in the existing log/lab rather than maintaining duplicate notes here.

An agent-generated solution, passing tests, a module completion badge, or an explanation written by the agent does not justify D. Record the learner's explanation and independent variation or diagnosis. If evidence was only self-reported, label it accordingly.

## Containers — S1

| ID | Study focus | Resource/module | Suggested evidence | Progress; evidence |
|---|---|---|---|---|
| C01 | Registry image lifecycle | [P1], registry | Publish two image versions; explain which artifact each deployment runs. | N; — |
| C02 | ACR Tasks | [P1], registry | Run a cloud build; inspect its output and diagnose one build failure. | N; — |
| C03 | App Service runtime settings | [P1], App Service | Deploy the image with environment settings and secrets; diagnose a startup mismatch. | N; — |
| C04 | Container Apps environments/revisions | [P2], deploy/manage | Deploy a revision and explain an environment or revision configuration change. | N; — |
| C05 | KEDA scaling | [P2], scale | Predict and observe a scaler's reaction to workload changes. | N; — |
| C06 | AKS manifests | [P3], deploy/configure | Deploy a small workload and explain its manifests and service exposure. | N; — |
| C07 | AKS/Container Apps diagnosis | [P2], manage; [P3], monitor | Diagnose one failure on each platform using logs, events, and connectivity evidence. | N; — |

## Data services — S2, S3, S4

| ID | Study focus | Resource/module | Suggested evidence | Progress; evidence |
|---|---|---|---|---|
| D01 | Cosmos SDK queries | [P4], queries; S3 | Connect from Python and retrieve a selected subset of the synthetic corpus. | N; — |
| D02 | Cosmos indexing/consistency/RUs | [P4], performance; S3 | Measure query cost before/after a change; explain the consistency tradeoff. | N; — |
| D03 | Cosmos vector retrieval | [P4], vector search; S3 | Store embeddings and inspect relevant and irrelevant retrieval results. | N; — |
| D04 | Cosmos change feed | [P4], vector search; S3 | Process an update and explain how the derived data becomes current. | N; — |
| D05 | PostgreSQL SDK queries | [P5], build/query; S2 | Connect to Azure PostgreSQL from application code; diagnose a connection error. | N; — |
| D06 | PostgreSQL schema/types/indexes | [P5], build/query; S2 | Justify one schema/type choice and measure an index's effect. | N; — |
| D07 | pgvector query/index efficiency | [P5], optimize; S2 | Compare a vector query's latency and resource use before/after tuning. | N; — |
| D08 | PostgreSQL resource sizing | [P5], optimize; S2 | Explain compute/memory/storage choices using a workload measurement or documented sizing exercise. | N; — |
| D09 | Filtered pgvector retrieval/RAG | [P5], vector search; S2 | Retrieve source passages with metadata filtering; explain a relevance failure. | N; — |
| D10 | PostgreSQL connections | [P5], optimize; S2 | Compare bounded connection/pool settings under load and explain the bottleneck. | N; — |
| D11 | Redis cache lifecycle | [P6], data operations; S4 | Reproduce stale data and show expiry or invalidation restoring correctness. | N; — |
| D12 | Redis vector indexes | [P6], vector storage; S4 | Build and query an index; explain the effect of one index choice. | N; — |

## Integration — S5

| ID | Study focus | Resource/module | Suggested evidence | Progress; evidence |
|---|---|---|---|---|
| I01 | Service Bus delivery | [P7], Service Bus | Exercise queue and topic/subscription routing, then inspect a dead-lettered message and its recovery. | N; — |
| I02 | Event Grid routing/retries | [P7], Event Grid | Publish a custom event, vary a filter, and observe delivery failure/retry behavior. | N; — |
| I03 | Functions triggers/bindings | [P7], Functions | Implement an endpoint or handler and explain its trigger and binding behavior. | N; — |
| I04 | Functions configuration/deployment | [P7], Functions | Deploy the function app and diagnose a missing setting or access failure. Local execution alone is insufficient. | N; — |

## Security and observability — S6, S7

| ID | Study focus | Resource/module | Suggested evidence | Progress; evidence |
|---|---|---|---|---|
| O01 | Key Vault retrieval/rotation | [P8], secrets; S6 | Retrieve and rotate a test secret; explain the identity and permission path without exposing values. | N; — |
| O02 | App Configuration | [P8], settings; S6 | Store/retrieve application settings and explain an environment-specific configuration change. | N; — |
| O03 | OpenTelemetry | [P9], instrument; S7 | Trace one document across process/service boundaries and diagnose missing propagation. | N; — |
| O04 | KQL logs/metrics analysis | [P9], telemetry; S7 | Write and vary a query that identifies failed jobs or a latency pattern. | N; — |

## Learning resources

Each resource is an official learning path. The module cues in the tables identify the relevant part of that path; use the current page's module list if a title changes.

| Ref | Official learning path | Milestone |
|---|---|---|
| P1 | [Implement container application hosting on Azure][P1] | S1 |
| P2 | [Deploy and manage apps on Azure Container Apps][P2] | S1 |
| P3 | [Deploy and monitor applications on Azure Kubernetes Service][P3] | S1 |
| P4 | [Develop AI solutions with Azure Cosmos DB for NoSQL][P4] | S3 |
| P5 | [Develop AI solutions with Azure Database for PostgreSQL][P5] | S2 |
| P6 | [Enhance AI solutions with Azure Managed Redis][P6] | S4 |
| P7 | [Integrate backend services for AI solutions][P7] | S5 |
| P8 | [Manage application secrets and configuration for AI solutions][P8] | S6 |
| P9 | [Observe and troubleshoot apps on Azure][P9] | S7 |

Course extensions such as App Service sidecars, Container Apps dynamic sessions, Redis messaging, and Durable Functions are useful learning opportunities. They do not replace any of the tracked objectives. The custom reliability exercise, frontend, and CI are project work rather than separate exam objectives.

## Readiness review

- [ ] Reconcile this file with the current official guide; record the new review date and any changes.
- [ ] Review every N/H/R row and assign the next study action. Do not mark unsupported confidence as evidence.
- [ ] Try a fresh diagnosis or design choice for previously demonstrated areas; move a row to R when needed.
- [ ] Use the practice assessment linked from the [certification page](https://learn.microsoft.com/en-us/credentials/certifications/azure-ai-cloud-developer-associate/); review why alternatives are wrong, not just the answer.
- [ ] Record remaining weaknesses and decide whether to book or defer the exam. Completing this project or reaching a practice score does not guarantee a pass.

[P1]: https://learn.microsoft.com/en-us/training/paths/implement-container-app-hosting-azure/
[P2]: https://learn.microsoft.com/en-us/training/paths/deploy-manage-apps-azure-container-apps/
[P3]: https://learn.microsoft.com/en-us/training/paths/deploy-monitor-apps-azure-kubernetes-service/
[P4]: https://learn.microsoft.com/en-us/training/paths/develop-ai-solutions-azure-cosmos-db/
[P5]: https://learn.microsoft.com/en-us/training/paths/develop-ai-solutions-azure-database-postgresql/
[P6]: https://learn.microsoft.com/en-us/training/paths/enhance-ai-solutions-azure-managed-redis/
[P7]: https://learn.microsoft.com/en-us/training/paths/integrate-backend-services-ai-solutions/
[P8]: https://learn.microsoft.com/en-us/training/paths/manage-app-secrets-configuration/
[P9]: https://learn.microsoft.com/en-us/training/paths/observe-troubleshoot-apps/
