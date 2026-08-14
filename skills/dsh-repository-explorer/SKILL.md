---
name: dsh-repository-explorer
description: Map a DeepSeek Harness repository before changing it. Use when locating Profile, Bundle, Patch, plugin, Service, Event, Session, tool, host, client, or Agent Loop ownership; tracing a feature across packages; or preparing an implementation plan grounded in the actual checkout.
---

# Explore a DeepSeek Harness Repository

Build an evidence-backed map of the checked-out revision before proposing changes.

## Workflow

1. Read repository-level `AGENTS.md`, contribution guidance, package manifests, and workspace configuration.
2. Record the current revision and detect local changes. Never discard or overwrite unrelated work.
3. Identify the active Profile and trace its ordered Bundles, local dependencies, and Patch layers.
4. Follow configuration rows to plugin entry points. Record each plugin's `inject`, provided Services, Events, and registered Effects.
5. Classify every relevant package as host, client, shared contract, runtime configuration, or test infrastructure.
6. Trace one end-to-end path from user action or session event through the Agent Loop, tool/model boundary, persistence, and UI observation.
7. Produce a compact change map: entry points, owners, dependencies, lifecycle, contracts, tests, and likely risks.

## Search Order

Prefer fast repository search over directory-by-directory browsing:

- locate workspace and package manifests;
- find `dsh.bundle`, Profile definitions, and `cordis.patch.yml`;
- find plugin `name`, `inject`, `apply`, Service declarations, and Event maps;
- find the Session event type before treating any UI projection as authoritative;
- find existing tests and neighboring plugins before inventing a pattern.

## Required Output

Report:

- exact files and symbols that own the behavior;
- the runtime plugin path from Profile to live Fiber;
- Service and Event dependencies;
- host/client boundary and remote contract, when applicable;
- persisted Session events and derived state;
- cleanup behavior and failure boundaries;
- test commands already used by the repository;
- facts tied to the current revision versus inferences or unstable preview behavior.

## Safety Boundaries

- Treat `inject` as dependency visibility, not an operating-system sandbox.
- Treat runtime-generated JavaScript, install scripts, subprocesses, network access, and direct Node.js imports as privileged behavior.
- Do not expose credentials or copy private Session content into reports.
- Do not assume documentation from another revision matches the checkout; verify names and paths locally.

## Completion Check

Finish only when another developer can locate the change surface without repeating the exploration and every uncertain claim is explicitly labeled.

## Official Contracts

- https://github.com/deepseek-ai/deepseek-harness
- https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md
- https://github.com/deepseek-ai/deepseek-harness/blob/master/AGENTS.md
