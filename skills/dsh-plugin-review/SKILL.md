---
name: dsh-plugin-review
description: Review a DeepSeek Harness plugin, Bundle, Profile, or Patch for correctness, compatibility, lifecycle, and security. Use for pre-install audits, code review, publication readiness, version upgrades, or investigating leaks, unsafe permissions, broken replay, and plugin-order problems.
---

# Review a DeepSeek Harness Plugin

Produce prioritized, evidence-backed findings for the exact revision under review.

## Workflow

1. Record the plugin and Harness revisions, source, license, package metadata, lockfile state, install scripts, and build provenance.
2. Resolve the full configuration order: Profile, Bundles, local Patch, user Patch, and command-line overrides.
3. Trace plugin entry points, `inject`, provided Services, Events, Effects, Session writes, remote APIs, and client code.
4. Review correctness: dependency availability, schema validation, error propagation, cancellation, deterministic ordering, and replay behavior.
5. Review lifecycle: listener removal, Service withdrawal, timers, file handles, subprocesses, network connections, temporary files, and remount behavior.
6. Review security: credentials, filesystem scope, subprocess arguments, generated code, network destinations, telemetry, data retention, approvals, sandbox enforcement, and supply chain.
7. Review tool contracts: strict schemas, authorization, idempotency, output bounds, redaction, canonical results, and model/UI rendering.
8. Run existing tests and the narrowest safe reproduction. Use a disposable Profile for installation tests and inspect `--dump-config`.
9. Report findings by severity with exact file and line evidence, impact, triggering conditions, and a bounded remediation.

## Severity Guide

- Critical: likely credential disclosure, arbitrary code execution across an asserted boundary, or destructive behavior without meaningful control.
- High: realistic unauthorized side effect, persistent data corruption, replay breakage, or resource leak affecting other plugins.
- Medium: correctness or compatibility defect with a bounded trigger or recovery path.
- Low: maintainability, observability, or documentation gap that materially increases future risk.

## Mandatory Questions

- Does every required Service appear in `inject`, and what happens when it disappears?
- Does every owned Effect dispose on unload, failure, and remount?
- Is every model-visible state transition reconstructable from durable Session events?
- Are dependency visibility, policy approval, and OS sandboxing described separately?
- Can installation or startup execute unreviewed code?
- Are secrets excluded from logs, Events, errors, tool results, and UI payloads?
- Is the package compatible with the exact preview revision being used?

## Safety Boundaries

- Keep review actions read-only unless the user explicitly requests fixes.
- Never execute untrusted install scripts or plugins merely to inspect them.
- Never include real credentials or private Session content in findings.
- Do not infer safety from popularity, inclusion in a list, or a clean manifest.

## Completion Check

If no actionable issue is found, state that explicitly and list residual risks, untested paths, and revision assumptions. Do not convert uncertainty into a clean bill of health.

## Official Contracts

- https://github.com/deepseek-ai/deepseek-harness
- https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md
- https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.md
