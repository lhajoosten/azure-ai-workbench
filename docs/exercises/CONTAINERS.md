# Container exercises — S1

Use [the session rules](README.md#session-rules) and a bounded Azure lab. C01–C07 map directly to the [coverage checklist](../EXAM_COVERAGE.md). Protect application access before cloud exposure. Separate deployments and diagnoses into learner-sized steps; do not provision all platforms simultaneously.

## C01

**Assignment: identify exactly which image is running.**

**Start:** a working local image from W3-02, approved registry access, and two distinguishable application versions.

**Predict:** if a tag is moved to a new image, does every existing deployment immediately change?

**Tasks:** build and push both versions to ACR; inspect tags and digests; select one version for a bounded deployment. Change the tag association and inspect the actual running version rather than assuming it updated.

**Done:** show the registry artifacts, deployment reference, and observed version; explain the difference between a tag and image identity. Keep credentials out of evidence.

**Transfer:** describe and demonstrate a rollback to the prior known artifact. Justify the versioning convention you would use in this repository.

## C02

**Assignment: move an image build into ACR Tasks.**

**Start:** C01's registry and a local build context that contains no credentials, uploads, or generated data.

**Predict:** which build inputs must exist remotely if the task cannot see arbitrary files on your laptop?

**Tasks:** build with ACR Tasks using the current official workflow; inspect its run and output artifact. In a disposable context, reference one missing build input, observe the failure, and correct it.

**Done:** distinguish build failure from registry authentication and deployment failure using actual evidence. Confirm the successful artifact is in ACR.

**Transfer:** change one source file and explain how you establish that the new artifact contains that change.

## C03

**Assignment: deploy the image to App Service.**

**Start:** an image from C01, approved App Service scope, and protected access. A liveness-only image can keep this first deployment independent of database networking.

**Predict:** will a local `.env` file automatically supply configuration to the hosted container?

**Tasks:** deploy; configure required environment settings and a safe test secret reference; verify behavior without exposing secret values. Introduce one incorrect startup/runtime setting and investigate logs before correcting it.

**Done:** observe the application and the deliberately failing version; identify the configuration responsible. Explain image settings versus host configuration.

**Transfer:** change a non-secret setting without changing source code and verify when the new value takes effect. Clean up the hosting resources as planned.

## C04

**Assignment: inspect a Container Apps revision change.**

**Start:** approved Container Apps environment and registry-pull access. Reuse a known image after cleaning up the earlier hosting lab where practical.

**Predict:** which change should create a new revision, and how would you confirm that from the platform rather than memory?

**Tasks:** deploy a protected app; inspect its environment, revision, and replica state; update its image or a revision-scoped setting. Observe the new revision's health before directing traffic to it under the chosen revision mode.

**Done:** connect a visible application version to its revision and settings; distinguish the environment from the app and replica.

**Transfer:** make the new revision fail safely, diagnose it, and restore a working revision. Record your actual revision/traffic behavior.

## C05

**Assignment: observe workload-driven scaling.**

**Start:** C04 works. Choose one supported KEDA event source and explicitly bound replica limits, workload, and cost. Provision any source only within the agreed scope.

**Predict:** when a small burst arrives, which scaling signal should change first, and can scaling be instantaneous?

**Tasks:** configure one event-based scaling rule; record the starting replicas; produce a controlled burst; inspect metric, replica, and processing changes over time. Stop the producer and observe the configured scale-down behavior.

**Done:** show a timeline with workload context and explain observed delays and minimum/maximum limits. Do not treat an HTTP load test alone as evidence of the chosen event scaler.

**Transfer:** vary one threshold or burst size and compare. Do not infer zero cost from zero replicas.

## C06

**Assignment: deploy and explain a small AKS workload.**

**Start:** the previous hosting lab is cleaned up or explicitly retained; AKS cost, access, and cleanup are agreed. Use the smallest suitable application slice and protected connectivity.

**Predict:** what will happen if the Service forwards to a port where the application does not listen?

**Tasks:** write and apply a Deployment and Service manifest; introduce required runtime configuration; inspect pods and endpoints. Observe the wrong-port failure, then correct it.

**Done:** the workload is reachable through the agreed access path; explain each manifest's responsibility and which evidence identified the fault.

**Transfer:** update the image and inspect rollout behavior. Justify whether AKS is worth its operational overhead for this application. Complete cluster/dependency teardown verification.

## C07

**Assignment: diagnose deployment failures from symptoms.**

**Start:** reuse approved Container Apps and AKS lab windows; do the two platforms separately. The agent selects one reversible fault on each platform and does not disclose the cause immediately.

**Predict:** before changing anything, which evidence would distinguish an image-pull failure from a healthy process with broken connectivity?

**Tasks:** collect relevant logs, events, revision/pod status, and connectivity observations; form a hypothesis; make one justified correction; verify it.

**Done:** record a cause-and-evidence explanation for each platform, not a sequence of speculative setting changes. Cleanup is verified for each lab.

**Transfer:** diagnose a different fault category with reduced help. A completed Container Apps diagnosis alone does not complete the AKS portion.
