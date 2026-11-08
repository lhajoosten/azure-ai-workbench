# Data-service exercises — S2, S3, S4

Follow [the session rules](README.md#session-rules). Use the shared synthetic corpus and approved cloud scope. Suggested order: S2 uses D05, D06, D09, D07, D08, D10; S3 uses D01–D04; S4 uses D11–D12. IDs follow the exam checklist rather than execution order.

Embedding exercises require an explicitly selected model/provider, compatible vector dimensions, and agreed access/cost. No credential values belong in the repository. Reuse approved embeddings where compatible; deterministic fake vectors test plumbing, not semantic relevance. Three documents do not support performance claims.

## D01

**Assignment: query a small Cosmos document store.**

**Start:** approved Cosmos DB for NoSQL access; the three corpus documents, each with its document ID and metadata. Choose and explain the container's partition key before creating it.

**Predict:** how should selecting only `kind=agreement` change the result compared with all documents?

**Tasks:** connect from Python using the current SDK; insert the corpus without duplicating IDs; retrieve one item; query agreements and a supplier-filtered subset. Capture relevant request diagnostics safely.

**Done:** results match the known corpus and filters; missing data is distinguished from a connection/access error. Explain the resource and partition context used by each operation.

**Transfer:** add a fourth document and adjust a parameterized query without interpolating raw user text into its structure.

## D02

**Assignment: measure a Cosmos query tradeoff.**

**Start:** D01 works. Generate a larger synthetic set with a recorded distribution; choose a query and measure its baseline with the current indexing policy.

**Predict:** which index or consistency change might affect query behavior or cost, and which observation would test that claim?

**Tasks:** capture returned items and request-unit consumption; vary one relevant index policy or consistency setting within the account's supported configuration; wait for any required index transformation; repeat under comparable conditions.

**Done:** preserve result correctness and report measured cost/latency with limitations. Distinguish an observation from an unsupported claim about all workloads or immediate freshness.

**Transfer:** choose a different query pattern and explain whether the earlier change still helps. Restore or document the retained policy.

## D03

**Assignment: retrieve relevant passages with Cosmos vectors.**

**Start:** D01, an approved embedding model, and a container configured for the required vector capabilities. Recheck supported settings before provisioning; do not assume an existing container can accept every policy change in place.

**Predict:** for corpus queries A and B, which documents should be useful, and why might a delivery note be an irrelevant match?

**Tasks:** store passage embeddings and their model/dimension metadata; run vector similarity retrieval; compare top results with a metadata-filtered query. Inspect passages and scores together.

**Done:** document expected versus actual relevance, including a mismatch if present. No generated answer is required. Incompatible dimensions fail clearly rather than being padded or truncated silently.

**Transfer:** run query C and explain why a nearest neighbor alone does not establish that a hotline number exists.

## D04

**Assignment: update derived data from the change feed.**

**Start:** D03 and an approved change-feed processor setup, including its checkpoint/lease requirements. Use a separate derived output or a deliberate marker/filter to avoid self-triggering an endless update loop.

**Predict:** if an agreement changes while the processor is stopped, what must happen after it resumes?

**Tasks:** insert a new corpus item and update an existing agreement's renewal terms; process those changes into derived metadata or refreshed embeddings; stop/resume the processor and inspect catch-up behavior.

**Done:** both inserted and updated items produce the intended derived state; demonstrate that processing its own output does not create a loop. Label any replay behavior actually observed.

**Transfer:** explain how you would keep repeated processing from duplicating derived records. Do not claim exactly-once processing.

## D05

**Assignment: connect application code to Azure PostgreSQL.**

**Start:** a working local schema and approved Azure Database for PostgreSQL scope. Establish authenticated, encrypted connectivity and a least-privilege application identity using current documentation.

**Predict:** what evidence could distinguish a bad database name from missing network access?

**Tasks:** connect from Python; apply the small schema through the agreed migration procedure; write and read one synthetic document. Introduce one incorrect non-secret connection setting and investigate its error.

**Done:** show successful database operations and a justified diagnosis of the broken configuration. Keep connection secrets and account identifiers out of logs and committed output.

**Transfer:** run the same bounded operation from the intended deployed environment and explain any change in identity or connectivity. Local success alone does not prove deployed access.

## D06

**Assignment: justify a schema and an ordinary index.**

**Start:** D05; tables for synthetic documents and derived passages. Prepare a parameterized metadata query and a sufficiently large synthetic dataset, recording its shape.

**Predict:** which part of the query might an index help, and what write/storage cost could it introduce?

**Tasks:** justify the IDs, metadata types, and relationship between documents and passages; inspect the query plan; introduce one suitable index through a migration; compare the same query and results before/after.

**Done:** show the plan and measurements rather than merely asserting that an index exists. Explain when a planner choosing a sequential scan could still be reasonable.

**Transfer:** vary filter selectivity or the queried metadata field and explain whether the index remains useful.

## D07

**Assignment: compare pgvector retrieval configurations.**

**Start:** D09 works; a larger approved synthetic embedding set and fixed query cases. Record the existing distance metric, index configuration, dataset size, and resource settings.

**Predict:** could faster approximate retrieval return different useful passages from an exact baseline?

**Tasks:** establish the baseline; change one supported vector index or search parameter; inspect plans and repeat measurements with controlled query cases. Measure relevance/recall against the chosen baseline as well as latency.

**Done:** report result changes and resource/latency observations, including uncertainty. Do not extrapolate a production speedup from three documents or change several parameters together.

**Transfer:** add a selective metadata filter and investigate whether the earlier configuration still meets the retrieval requirement.

## D08

**Assignment: choose resources from evidence.**

**Start:** D07 measurements and the approved Azure PostgreSQL budget. Inspect current service capabilities, metrics, pricing, and workload requirements.

**Predict:** which measurement would make you suspect memory, compute, or storage is the current constraint?

**Tasks:** collect a representative workload's relevant metrics; propose one sizing change with its expected benefit and estimated cost. If within the agreed scope, change only that resource dimension and remeasure. Otherwise complete a documented sizing exercise without provisioning it.

**Done:** connect the recommendation to evidence and cost assumptions. Label projected benefits as projections, not observed improvements.

**Transfer:** explain how your recommendation would change for twice the corpus size versus twice the concurrent requests. Restore resized resources or document their agreed retention.

## D09

**Assignment: retrieve filtered source passages with pgvector.**

**Start:** D05/D06, enabled vector support, approved embeddings, and the corpus. Choose a chunking approach and preserve document/chunk/model identifiers.

**Predict:** which passages should queries A and B retrieve, and what should query C fail to establish?

**Tasks:** store embeddings; retrieve passages by similarity; combine retrieval with `kind=agreement` or supplier filtering. Inspect actual source text, not just numeric scores. Add a bounded retrieval interface; answer generation is optional and separate.

**Done:** source IDs remain traceable; filters hold; dimension mismatches are rejected; relevance expectations and observed deviations are recorded. Explain how this retrieval could feed RAG without inventing missing facts.

**Transfer:** change chunk boundaries and explain a changed result. A successful SQL query alone does not demonstrate semantic quality.

## D10

**Assignment: observe connection contention.**

**Start:** D05's application operations and a safe load limit. Record the application's current pool settings, concurrency, and database connection metrics.

**Predict:** where should requests wait when application concurrency exceeds the configured pool capacity?

**Tasks:** run a small bounded concurrent workload; measure latency, timeouts, and connections; change one pool limit or timeout and compare. Check that sessions/transactions are released on success and error paths.

**Done:** distinguish waiting for a connection from slow query execution. Explain results using measurements; do not solve contention by blindly raising every limit.

**Transfer:** deliberately fail one operation and verify subsequent requests can still acquire connections. Return to the agreed workload and pool settings.

## D11

**Assignment: observe and repair stale cached data.**

**Start:** approved Azure Managed Redis access and a simple metadata lookup backed by the primary datastore. Choose keys, expiry, and safe cache-miss behavior before integrating it.

**Predict:** if the agreement changes in the primary datastore, what could a cached lookup return?

**Tasks:** populate and read the cache; modify the source; reproduce staleness; implement a bounded expiry/invalidation policy and observe the next read. Exercise a miss and a temporary cache failure without fabricating a successful source read.

**Done:** show hit/miss and invalidation behavior while preserving source correctness; justify the freshness tradeoff.

**Transfer:** explain whether the same policy would fit job status, then test a shorter-lived value. Avoid adding a generic caching framework.

## D12

**Assignment: compare vector indexing in Redis.**

**Start:** approved Redis vector capabilities and compatible corpus embeddings. Use the selected service's current supported data structure and index configuration.

**Predict:** what must match between the indexed vectors and the query vector for similarity search to be meaningful?

**Tasks:** create an index; load the corpus; query A/B and inspect source IDs; choose one supported index/data-structure variation and compare. Deliberately submit an incompatible query vector and handle its failure visibly.

**Done:** explain the vector type/dimensions, metric, and index choices used; document results and the service-specific error rather than silently coercing vectors.

**Transfer:** compare this experiment with pgvector or Cosmos using the same corpus and query, explaining one operational tradeoff. Avoid speed claims without comparable measurements.
