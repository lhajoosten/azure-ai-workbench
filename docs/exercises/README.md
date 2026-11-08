# Exercise catalogue

These are assignments to work through with the agent, not a list for the agent to implement. Start with **W0-01**. Each assignment has a starting state, prediction, concrete tasks, observable completion conditions, and a transfer challenge. There are deliberately no completed implementations or answer keys.

## Choose the next assignment

| Stage | Assignments | Prerequisite |
|---|---|---|
| W0–W3, local warm-up | [Six warm-up exercises](WARMUP.md) | Documentation-only repository; inspect local tools first |
| S1, containers | [C01–C07](CONTAINERS.md) | Warm-up, plus agreed Azure access and budget |
| S2–S4, data | [D01–D12](DATA_SERVICES.md) | Working synthetic corpus; see each exercise's starting state |
| S5, integration | [I01–I04](INTEGRATION.md) | Warm-up worker; bounded Azure lab prerequisites |
| S6–S7, operations | [O01–O04](OPERATIONS.md) | A deployed workload; identity/access setup before exposure |

The 27 Azure IDs match [EXAM_COVERAGE.md](../EXAM_COVERAGE.md). Follow the official module linked there alongside the assignment. The six warm-up exercises prepare the application and do not independently establish Azure exam coverage.

## Session rules

1. Read the starting state. If a prerequisite is missing, identify the smallest required setup; do not generate an entire platform.
2. Ask the prediction question and wait. For a task with several parts, agree on one 20–45-minute slice; the full assignment may take several sessions.
3. Let the learner own the logic or decision under study. Give progressive hints on request under [AGENTS.md](../../AGENTS.md). Routine mechanics already understood can be delegated explicitly.
4. Run the described observations. The learner chooses appropriate tests; do not ship solved test suites before the attempt.
5. Try the transfer challenge, record actual evidence, and stop. Update the learning log and the relevant coverage row only after an attempt.

The tasks specify observable behavior, not the implementation algorithm. A valid alternative can satisfy the assignment. Do not require the learner to reproduce an agent's preferred architecture.

For cloud work, copy [AZURE_LAB_TEMPLATE.md](../AZURE_LAB_TEMPLATE.md) before provisioning. Use synthetic data, authenticated access, current official documentation, and the agreed resource/cost scope. Inspect and clean up resources after the experiment. A local substitute must be labeled; it cannot prove Azure-specific behavior.

Official course labs can satisfy an assignment when they produce its evidence and transfer task. Reuse that evidence rather than repeating the same work in this application. These are original practice briefs, not reproduced Microsoft labs or exam questions, and are not evidence of a completed or validated lab environment.

## Shared synthetic corpus

During W1-01, create three small local UTF-8 Markdown fixtures with these exact facts; wording is your choice:

| Document | Metadata | Facts |
|---|---|---|
| Harbor maintenance | `kind=agreement`, `supplier=harbor` | Automatic renewal; cancellation requires 30 days' notice; weekday support |
| Meadow equipment | `kind=agreement`, `supplier=meadow` | Ends after 12 months; renewal requires written agreement; weekend support |
| Harbor delivery | `kind=delivery`, `supplier=harbor` | Three replacement filters delivered; contains no renewal terms |

Use this corpus in later retrieval exercises. Query A: "Which agreements renew automatically?" Query B: "Which supplier provides weekend support?" Query C: "What is the emergency hotline number?" Record relevance expectations before seeing search output. No document supplies an emergency hotline number.

For performance experiments, generate a larger synthetic set during the exercise and record its size and distribution. Three documents can illustrate retrieval behavior but cannot establish production performance. Keep large generated corpora and outputs out of git.

## First action

Open [W0-01](WARMUP.md#w0-01), ask its prediction question, and wait. No exercise or objective is completed merely because this catalogue exists.
