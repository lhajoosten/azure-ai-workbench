# Azure lab template

Copy this file into `labs/<topic>/README.md` when that experiment begins. Replace placeholders for one bounded question; do not fill in future labs or prewrite the learner's answers. Keep scripts beside the lab only when introduced. Follow the teaching loop in [AGENTS.md](../AGENTS.md), using repository-relative links appropriate to the copied location.

This template is not authorization to provision resources. Establish the cloud scope below with the learner before deployment. Reuse an already agreed scope for routine steps within it.

## 1. Question and study link

- **Question:** <one behavior or service decision to investigate>
- **Milestone/objective IDs:** <for example, S5 / I04 from EXAM_COVERAGE.md>
- **Official learning module/documentation:** <current source link and review date>
- **Outcome:** <one observable acceptance condition>
- **Out of scope:** <features deliberately deferred>

## 2. Prerequisites and cloud scope

- **Local baseline:** <relevant working feature and commit; actual required tools/versions>
- **Environment:** <real Azure service, emulator, or local-only; identify what this can establish>
- **Account scope:** <locally verified subscription/tenant aliases and region; do not commit sensitive identifiers>
- **Resources/SKUs:** <exact planned resource types and tiers; shared versus lab-owned>
- **Access:** <who/what connects, identity, minimum roles, authentication, and network exposure>
- **Credentials/configuration:** <where obtained/stored/referenced; never include values>
- **Prerequisite check:** <commands to run and results, initially not run>
- **Agreed scope:** <learner's actual decision; initially not agreed>

Use synthetic documents. Do not publish an unauthenticated processing endpoint. Missing access or prerequisites are blockers to record, not reasons to bypass controls.

## 3. Cost and cleanup plan — before provisioning

| Item | Plan |
|---|---|
| Pricing basis | <official pricing source, date, region, currency, and relevant usage assumptions> |
| Planned duration and usage | <runtime, replicas/capacity, requests, storage, telemetry, and any model calls> |
| Estimated cost and uncertainty | <estimate with assumptions; record unknowns explicitly> |
| Learner's agreed budget | <amount/currency; initially not agreed> |
| Monitoring and stop decision | <how to inspect usage and when to stop or reduce the experiment> |
| Billable resources after traffic stops | <database, cluster, cache, disks, registry, storage, monitoring, or other retained resources> |
| Cleanup ownership and method | <exact lab-owned resources, intended deletion procedure, and how shared resources are preserved> |

A budget alert is not a hard spending cap. Do not assume a free tier, credits, or stopping the app eliminates charges. Use an isolated resource group where appropriate; inspect its contents before deletion. Agree on consequential changes to the scope rather than silently upgrading resources.

## 4. Prediction and division of work

- **Prediction question:** <ask one concrete question before revealing the explanation>
- **Learner prediction/reasoning:** <leave empty until answered>
- **Learner owns:** <the learning-critical decision or implementation>
- **Agent may supply:** <bounded scaffolding, a hint, or previously understood mechanics>

The agent pauses here for the learner's contribution. Do not place the completed solution below the prediction as an answer key.

## 5. Experiment

Agree on one small next step at a time. Add commands/configuration when introduced and explain unfamiliar effects before execution.

| Step | Change/action | Expected observation | Actual observation/evidence |
|---|---|---|---|
| Baseline | <inspect the known state> | <what should be visible> | Not run |
| Change | <one controlled variation> | <prediction> | Not run |
| Check | <focused command/query/request> | <acceptance condition> | Not run |

Capture relevant configuration, sanitized output, and timings with workload context. Do not commit secrets, account identifiers, document bodies, raw traces, or large generated artifacts. A proposed command is not a recorded result.

## 6. Failure and diagnosis

- **Controlled failure:** <one reversible fault, limited to the lab>
- **Learner's initial diagnosis:** <observed answer, not agent-generated>
- **Evidence gathered:** <logs/events/query/results and their interpretation>
- **Correction:** <small justified change>
- **Verification:** <actual observation showing recovery>
- **Remaining limitations:** <what the test did not establish>

## 7. Explain and transfer

- **Learner explanation:** <why the result occurred>
- **Independent variation:** <a different parameter, fault, or service-choice scenario>
- **Observed outcome:** <actual result and amount of help>
- **Design implication:** <when this service/approach fits this application and what it trades off>

An agent demonstration alone leaves learning at not assessed or practiced with help. Follow it with a learner-owned variation before recording demonstrated understanding.

## 8. Teardown and verification

- **Pre-deletion inspection:** <confirm lab-owned resource names and dependencies>
- **Cleanup performed:** <actual commands/actions; initially not performed>
- **Verification:** <actual resource inventory after cleanup, including retained dependencies>
- **Remaining resources and reason:** <what remains billable, who owns it, and its disposition>
- **Observed usage/cost:** <available report and time checked; note reporting delays>
- **Outcome:** <cleaned up / intentionally retained within agreed scope / blocked; initially not assessed>

Do not mark cleanup complete merely because an application stopped or a delete command was issued. Verify the resulting state. Absence of an immediate cost entry does not prove zero charges.

## 9. Checkpoint

- **Implementation:** not started / in progress / verified; <actual verification evidence>
- **Learning:** not assessed / practiced with help / demonstrated / revisit; <learner evidence>
- **Coverage update:** <objective IDs and links; leave other rows unchanged>
- **Learning-log entry:** <link to the short session checkpoint>
- **Next exercise or unresolved question:** <one bounded step>

In a completed lab, replace status alternatives with the observed status. Keep this reusable template uncompleted. Use links instead of copying the same explanation into the coverage checklist and learning log.
