---
name: dsh-plugin-scaffold
description: Create or extend a DeepSeek Harness runtime plugin and its packaging. Use when adding a lifecycle-safe plugin, configuration schema, Service provider or consumer, Bundle, Profile entry, Patch, tests, or publish metadata in a DSH-compatible project.
---

# Scaffold a DeepSeek Harness Plugin

Create the smallest plugin that follows the conventions of the target checkout.

## Workflow

1. Inspect the target repository, revision, package manager, existing plugins, tests, and local instructions.
2. Define one responsibility and decide whether it belongs in the host, client, or both.
3. Reuse the nearest maintained plugin pattern. Declare a stable plugin name and typed `Config` schema.
4. Declare required Services through `inject`; provide new Services only when multiple consumers need a stable capability seam.
5. Register listeners, Services, timers, handles, and other resources through lifecycle-aware `ctx` helpers. Return cleanup for anything registered outside those helpers.
6. Add typed Events for observation or interception instead of hidden cross-plugin calls.
7. Add focused tests for startup, missing dependencies, behavior, disposal, and remount where relevant.
8. Add a local Patch for development. Add Bundle and package metadata only when distribution is requested.
9. inspect the resolved configuration with `dsh --profile <profile> --dump-config` before running the target Profile.
10. Run repository formatting, type-checking, and tests. Report commands and unresolved preview-version assumptions.

## Design Rules

- Keep the initial plugin narrow; split independent capabilities into separate plugins.
- Use configuration for deployment choices and Services for runtime capabilities.
- Keep model-visible names, descriptions, and schemas stable because they affect prompt prefixes and cache reuse.
- Persist model-visible outcomes as Session events when later replay or UI reconstruction depends on them.
- Separate host implementation from client presentation and define a typed remote boundary when both are needed.

## Packaging Boundary

A plugin is executable behavior. A Bundle distributes ordered configuration rows. A Profile composes Bundles and local dependencies. A Patch is a deployment-time override. Do not present these as interchangeable artifacts.

## Safety Boundaries

- Never place real credentials in examples, fixtures, defaults, or committed configuration.
- Do not enable dependency build scripts without reviewing the pinned source.
- Ask before adding telemetry, network egress, broad filesystem access, subprocesses, or destructive tools.
- Do not claim cleanup is transactional; compensate external side effects explicitly.

## Completion Check

The plugin must mount, perform its narrow responsibility, dispose all owned resources, pass focused tests, and appear in the expected position of the dumped plugin tree.

## Official Contracts

- https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.md
- https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.md
- https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md
