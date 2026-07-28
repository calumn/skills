---
name: sdlc-delivery-observability
description: Guide observability implementation. Use when adding or reviewing logging, structured events, request IDs, traces, metrics, health checks, readiness checks, error reporting, or cross-service diagnostics.
---

# SDLC Delivery Observability

Use this skill when adding or reviewing operational visibility. Observability should make service behaviour diagnosable without mixing telemetry concerns into domain logic.

## Signals To Cover

- Logs: structured events for important service actions and failures.
- Traces: request and job flow across service seams.
- Metrics: counters, durations, queue lag, error rates, and resource usage.
- Health/readiness: simple endpoints for process health and dependency readiness.

OpenTelemetry is the preferred vocabulary for traces, metrics, logs, context propagation, resources, and instrumentation scope.

## Core Rules

- Add correlation/request IDs at the edge and propagate them across service calls and queue messages.
- Log meaningful events, not every line of execution.
- Include stable fields such as service name, environment, request ID, workspace ID when safe, analysis run ID, model version, and status.
- Avoid secrets, raw tokens, signed URLs, and unnecessary personal data in logs.
- Keep logs structured so they can be searched and aggregated.
- Make async workflows observable through job IDs and state transitions.

## Service Boundaries

- Core API should record incoming request context, authorization outcomes, upload-access creation, and analysis-request submission.
- Analysis Service should record job receipt, model version selection, image retrieval outcome, result persistence, tagged-output creation, and completion/failure.
- Queue messages should carry correlation IDs and domain IDs needed for tracing.
- Object-storage access should log object keys or references only when safe; never log signed URLs.

## Health And Readiness

- Health means the process is alive.
- Readiness means the service can perform useful work with required dependencies.
- Keep `/healthz` cheap and dependency-light.
- Add readiness checks separately when databases, queues, object storage, or model artifacts are required.

## Testing And Review

- Test that important workflow paths emit expected observable events when the code owns the logger/telemetry interface.
- Do not make tests brittle by asserting full log text.
- Review telemetry fields for privacy and operational usefulness before adding them.

