# Azure AI Workbench

A study-oriented document-processing application for learning how to build, deploy, and operate AI backends on Azure while preparing for AI-200.

**The deliverable is both a working application and the ability to explain, change, and troubleshoot it independently.** An agent completing the roadmap on the learner's behalf would defeat the purpose of this repository.

## Current status

Documentation only. There is no application, development environment, or deployed infrastructure yet. The first implementation session starts with a small local exercise. Commands, dependencies, and CI will be added when they actually exist.

## The application

Build a document-processing inbox: submit a synthetic document, receive a processing job, inspect its progress and extracted content, recover from failures, and eventually search across documents using embeddings.

Start with UTF-8 text and Markdown. Add PDF extraction and semantic retrieval after the processing lifecycle is understood. Use a CLI or the API first; a small React interface is a later exercise.

## Start here

1. Read [the project brief](docs/PROJECT_BRIEF.md) for the product, scope, and technical expectations.
2. Read [the learning roadmap](docs/LEARNING_ROADMAP.md) for milestones and evidence of understanding.
3. Open a local coding agent and use [the first-session prompt](docs/START_HERE.md).
4. Keep [the learning log](docs/LEARNING_LOG.md) updated so the next session resumes at the right point.

[AGENTS.md](AGENTS.md) defines the agent's teaching contract. It applies from the first session, including environment setup.

## Working agreement

- One small learning objective at a time.
- Predict a behavior, implement or inspect a small change, run it, explain the result, and try a variation.
- Ask for hints, a demonstration, pairing, or a review when useful.
- Track implementation progress separately from demonstrated understanding.
- Leave cloud infrastructure until the relevant lesson, with an explicit budget and teardown plan.

## Study scope

The project provides exercises for containers, vector-enabled data services, messaging, serverless execution, secrets, configuration, and observability. It supplements the official course; it is not a complete exam-preparation product or a guarantee of certification readiness.

Azure alternatives belong in focused comparison labs. The main application does not need to run three databases or three hosting platforms simultaneously.

## Official references

- [AI-200T00 course](https://learn.microsoft.com/en-us/training/courses/ai-200t00)
- [AI-200 study guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-200)
- [Azure AI Cloud Developer Associate](https://learn.microsoft.com/en-us/credentials/certifications/azure-ai-cloud-developer-associate/)

Curriculum mapping reviewed on 2026-11-08. Recheck the official objectives before the December study period and before booking the exam.

## License

[MIT](LICENSE).
