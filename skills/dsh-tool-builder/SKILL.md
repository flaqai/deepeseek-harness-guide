---
name: dsh-tool-builder
description: Design, implement, and verify a DeepSeek Harness tool plugin. Use when adding model-callable tools, JSON schemas, canonical results, policy or approval hooks, sandboxed execution, tool-result rendering, or Session event integration.
---

# Build a DeepSeek Harness Tool

Build a predictable tool contract before optimizing its implementation.

## Workflow

1. Inspect the tool registry, neighboring tools, execution pipeline, policy hooks, result types, and Session event contracts in the target revision.
2. Write the tool's single responsibility, intended caller, side effects, and explicit non-goals.
3. Choose a stable name and concise model-facing description. Define a strict, bounded input schema with meaningful field descriptions.
4. Separate validation, authorization, execution, canonical result creation, model rendering, and UI rendering.
5. Route sensitive actions through the existing policy, approval, and sandbox boundaries. Default ambiguous or destructive actions to denial or explicit approval.
6. Make retries safe where possible. Add idempotency keys, preconditions, or duplicate detection for external side effects.
7. Bound output size and redact secrets. Store large artifacts outside the prompt and return a stable reference when the host supports it.
8. Persist every result later visible to the model or reconstructed by the UI as the appropriate Session event.
9. Test valid input, schema rejection, denied action, cancellation, timeout, partial failure, redaction, deterministic rendering, and cleanup.
10. Inspect the resolved plugin tree and run focused plus repository-level verification.

## Contract Checklist

- Input schema is strict enough for the model to call reliably.
- Canonical result is transport-neutral and preserves error structure.
- Model-facing rendering is concise, deterministic, and secret-free.
- UI rendering does not become the source of truth.
- Permission and sandbox claims match actual enforcement.
- Cancellation and timeout release owned resources.
- Repeated execution has an explicit safety story.

## Safety Boundaries

- Never bypass the repository's approval path to make a demo easier.
- Never treat schema validation as authorization.
- Do not return raw environment variables, credentials, unbounded files, subprocess output, or third-party responses.
- Treat network, shell, filesystem mutation, browser control, and generated code execution as privileged categories.

## Completion Check

Finish when the tool contract is understandable without reading the implementation, failure behavior is tested, and a reviewer can distinguish dependency injection, policy, sandbox, and user approval boundaries.

## Official Contracts

- https://github.com/deepseek-ai/deepseek-harness
- https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md
- https://github.com/deepseek-ai/deepseek-harness/tree/master/packages
