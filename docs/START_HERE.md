# Start a learning session

## First session prompt

Paste this into your local coding agent after cloning the repository:

```text
Read AGENTS.md, README.md, docs/PROJECT_BRIEF.md,
docs/LEARNING_ROADMAP.md, and docs/LEARNING_LOG.md.
Inspect the repository and git status before changing anything.

Act as my mentor and pairing partner. I want to learn by implementing
and troubleshooting this application. Do not build the complete project,
scaffold future milestones, or give me the answer before I attempt the exercise.

Start at W0 unless recorded evidence shows it is already covered.
Propose one small exercise with its learning objective, acceptance condition,
and division of work between me and you. Ask one prediction question and
wait for my answer before writing application code or installing dependencies.

Keep the warm-up to the reduced single-worker scope. Leave automatic crash
recovery and competing-worker scenarios for formal study alongside messaging.

Give hints progressively. If you demonstrate something, follow it with
a different exercise that I own. Keep implementation progress separate
from demonstrated understanding. End with a short checkpoint in the log.
```

## First exercise

W0 starts with a minimal executable API slice. The first question is:

> If the application process is running but PostgreSQL cannot be reached, what should a liveness endpoint report, and how would you distinguish that from readiness?

Discuss the behavior before creating the endpoint. The exercise is small enough to observe immediately. There is no need to implement the database connection or production health checks to answer it.

## Resume prompt

```text
Follow AGENTS.md. Read the latest learning-log entry and inspect the current
implementation and git status. Summarize the observed state and select the
next bounded exercise. If useful, start with one short transfer task for a
recent concept. Do not advance multiple milestones. Ask one question and wait.
During formal study, consult docs/EXAM_COVERAGE.md and use the Azure lab
template for cloud experiments. Record evidence only when actually observed.
```

## Useful requests during a session

- "Give me one hint, without the implementation."
- "Show a small separate example, then let me apply it here."
- "Review my change and tell me which behavior I have not verified."
- "Help me design a failure experiment before fixing the problem."
- "I know this part; let me demonstrate it so we can move on."
- "Automate this repeated setup step; leave the worker-claiming logic to me."

If you deliberately delegate an entire task, say which task. Record it as implemented but not yet demonstrated, then schedule a small variation to check understanding. The workflow supports asking for help without treating generated code as evidence of learning.

## What a useful session produces

A small coherent change when appropriate, an actual observed result, a learner explanation or diagnosis, and a clear next step. Some sessions may be experiments with no permanent code change. Avoid turning the log into a second project.
