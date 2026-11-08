# Integration exercises — S5

Use [the session rules](README.md#session-rules) and current official modules linked in [the coverage checklist](../EXAM_COVERAGE.md). Scope queue/topic/function/event resources and their cleanup before provisioning. Reuse the document worker in small steps rather than rewriting the application.

## I01

**Assignment: route work and investigate failed delivery.**

**Start:** the warm-up worker and approved Service Bus access. Put document/job references in messages; do not copy document bodies or secrets into logs. Agree on a small message count and safe dead-letter settings.

**Predict:** how should two queue consumers differ from two subscriptions receiving a published topic message?

**Tasks:**

1. Send a bounded job reference to a queue, consume it, and connect its delivery to committed processing state.
2. In a separate slice, publish to a topic with two subscriptions and observe each route.
3. Deliberately fail handling of a test message; inspect its delivery/dead-letter outcome and perform a controlled recovery after correcting the cause.

**Done:** show observed routing, message settlement, failure reason, and recovery; explain the window between committing a document and publishing its message. Do not claim reliable publication until that window has been addressed by a separately demonstrated strategy.

**Transfer:** reason about a crash after writing a result but before settlement. Try one bounded duplicate-delivery or idempotency experiment from the roadmap's reliability exercise; do not build a full queue framework.

## I02

**Assignment: route an event and inspect delivery policy.**

**Start:** approved Event Grid resources and a protected handler reachable through a supported delivery mechanism. Use a synthetic document-completed event with metadata, not private content. Confirm the delivery identity and endpoint requirements before setup.

**Predict:** which subscribers should receive an event when its metadata matches one filter but not another?

**Tasks:**

1. Publish a custom event with documented schema and type; configure one filter and observe a match and non-match.
2. Introduce a reversible handler failure; inspect configured retry policy and delivery evidence within a bounded observation window.
3. Restore handling and verify the actual outcome. Do not infer unseen retries or eventual success from policy settings alone.

**Done:** connect event properties to routing and explain observed failure/retry behavior. State what the observation window could not establish.

**Transfer:** explain when you would choose Event Grid versus the work queue from I01. Change one filter condition and verify its effect before cleanup.

## I03

**Assignment: implement a small Functions integration.**

**Start:** the relevant official Functions module, a checked local Functions environment, and a deliberately small operation from the application. Choose a supported Python programming model and one relevant binding; do not assume every service has every binding.

**Predict:** what invokes this function, and which work is performed by the binding versus your Python code?

**Tasks:**

1. Implement one HTTP endpoint or event handler using the chosen trigger and a relevant additional binding, with synthetic input and explicit configuration.
2. Exercise valid input and one malformed/unsupported case. Observe the trigger/binding outputs and errors.
3. Introduce one incorrect binding-related setting and diagnose the effect using the current runtime's output.

**Done:** explain invocation, binding configuration, and application logic separately. Identify any emulator/local-only behavior and what remains unverified on Azure.

**Transfer:** modify an input or binding setting and predict the new behavior before running it. Deployment is a separate assignment, I04.

## I04

**Assignment: deploy and diagnose the function app.**

**Start:** I03 works locally. Agree on Azure hosting plan, runtime compatibility, identity, network access, resource dependencies, expected costs, and cleanup. Review the current official deployment guidance for that combination.

**Predict:** which local settings will need to be supplied explicitly in the deployed app?

**Tasks:**

1. Configure and deploy the function app using a documented method; invoke it through the agreed protected path.
2. Omit or alter one non-secret required setting, observe the deployed failure, and diagnose it using configuration and runtime evidence.
3. Restore the setting and verify a real invocation and its downstream effect.

**Done:** record observed Azure behavior, identity/configuration path, and evidence of correction. A successful deployment command or local invocation alone does not establish that the deployed function works.

**Transfer:** change one safe setting without changing source and verify when it takes effect. Inspect and remove the app's lab-owned dependencies as well as the function app itself.
