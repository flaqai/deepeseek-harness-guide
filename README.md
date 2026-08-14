# DeepSeek Harness Guide

[English](README.md) | [简体中文](README_zh.md) | [繁體中文](README_tw.md) | [日本語](README_ja.md) | [한국어](README_ko.md) | [Deutsch](README_de.md) | [Español](README_es.md) | [Français](README_fr.md) | [Italiano](README_it.md) | [Português](README_pt.md) | [Русский](README_ru.md) | [العربية](README_ar.md) | [Bahasa Indonesia](README_id.md) | [ไทย](README_th.md) | [Tiếng Việt](README_vi.md)

📘 [Read the technical architecture guide →](GUIDE.md)

> A community-maintained, multilingual guide to understanding, extending, and building plugins for [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness).

DeepSeek Harness (`dsh`) is an open-source agent harness developed by DeepSeek AI. Its central idea is unusually strong: **everything is a plugin**. Model adapters, tools, the agent loop, session storage, permissions, sandboxing, telemetry, and user interfaces can all be composed or replaced through configuration.

This repository turns that architecture into practical explanations, plugin recipes, reusable development workflows, and a curated path from “what is a harness?” to publishing a production-ready `dsh` plugin.

> [!IMPORTANT]
> This is an independent community guide, not an official DeepSeek repository. DeepSeek Harness is currently in developer preview and may introduce breaking changes. Always verify implementation details against the [official repository](https://github.com/deepseek-ai/deepseek-harness) and [official documentation](https://deepseek-harness.github.io/deepseek-harness/).

## Why this guide exists

An AI model alone does not read a repository, run commands, call tools, request approval, preserve a session, or recover from failure. The **harness** supplies that operating environment and coordinates the loop between the user, the model, tools, and application state.

DeepSeek Harness is interesting because it does not treat extensibility as a thin add-on layer. Its own built-in behavior is assembled from the same plugin mechanism available to developers. This gives teams a path to:

- replace a model provider, tool implementation, sandbox, storage layer, or subagent provider without forking the whole application;
- build focused coding, research, operations, or domain agents from reusable components;
- distribute configuration and plugins as versioned bundles;
- override deployments through profiles and patch layers;
- unload or hot-replace plugins while their registered effects are cleaned up.

## DeepSeek Harness in one minute

### 1. Cordis is the composition layer

DeepSeek Harness is powered by [Cordis](https://github.com/cordiverse/cordis), a meta-framework for spatiotemporal composability. Plugins contribute services, typed events, and reversible effects to a shared context. Dependencies are declared rather than manually ordered, and registrations can be unwound when a plugin is removed.

### 2. A running harness is a plugin tree

A `dsh` process starts from a **profile**, stacks one or more **bundles**, then applies user and command-line patch layers. The built-in `web` and `headless` experiences are compositions, not special hard-coded cores.

| Concept | Meaning |
| --- | --- |
| Plugin | A TypeScript module, object, or service class mounted into a Cordis context. |
| Bundle | An npm package that contributes a configuration layer through `dsh.bundle`. |
| Profile | A runnable composition that lists bundles and stores local plugin dependencies. |
| Patch | A YAML overlay that inserts or replaces configuration rows. Later layers win. |
| Service | A typed capability provided by one plugin and consumed by others. |
| Event | A durable fact or live extension point used to observe or intercept the agent flow. |

### 3. The agent loop is replaceable too

The default flow builds prompts and tool schemas, streams a model response, executes tool calls through a guarded pipeline, records durable session events, and continues until no further work is owed. The loop, model adapter, tool registry, and session log are all plugin-provided seams.

### 4. Host and client are separate faces

The official monorepo separates Node.js host packages from browser client packages. Host services can expose generated remote APIs to the Web UI, while client plugins can add interface behavior. This matters when choosing whether a plugin belongs in the runtime, the browser, or both.

## What this project will cover

- **Getting started** — install and run the Web UI, configure a model, select a workspace, and understand permissions.
- **Architecture** — Cordis contexts, reversible effects, services, events, scopes, session logs, and the turn/step lifecycle.
- **Plugin fundamentals** — `apply(ctx)`, dependency injection, configuration schemas, cleanup, and hot replacement.
- **Tool plugins** — typed inputs, canonical outputs, model-facing rendering, policy hooks, and guarded execution.
- **Capability providers** — model adapters, filesystems, subprocesses, sandboxes, storage, telemetry, and subagents.
- **Web extensions** — host/client boundaries, remote APIs, UI contributions, and build order.
- **Packaging** — bundles, profiles, `cordis.patch.yml`, npm/Git installation, versioning, and publishing.
- **Quality and security** — tests, permission boundaries, secrets, install scripts, dependency review, and release checks.

## Quick start with the official project

Requirements: a supported Node.js release and a DeepSeek API key (or another configured model endpoint).

```bash
npx @deepseek-ai/dsh web
```

The Web UI is served at `http://127.0.0.1:3080` by default. In **Settings → Models**, add a model credential, then choose a workspace and start a session.

To explore the source:

```bash
git clone https://github.com/deepseek-ai/deepseek-harness.git
cd deepseek-harness
pnpm install
pnpm run build
pnpm dsh web
```

See the official [Web UI guide](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/guide/index.md) and [development guide](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/development.md) for current prerequisites and commands.

## The smallest possible plugin

```ts
import type { Context } from '@deepseek-ai/cordis'

export const name = 'hello-plugin'

export function apply(ctx: Context) {
  ctx.effect(() => {
    console.log('[hello-plugin] loaded')
    return () => console.log('[hello-plugin] unloaded')
  })
}
```

A local patch can mount the module:

```yaml
- insert:
    - id: hello
      name: '/absolute/path/to/hello-plugin.ts'
```

Run it with:

```bash
pnpm dsh web --patch ./cordis.patch.yml
```

Real plugins should also declare consumed services through `inject`, expose configurable values with a Schemastery `Config` schema, and register tools or services through `ctx` so their effects follow the plugin lifecycle. Start with the official [first plugin tutorial](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.md).

## Plugin ideas and extension points

| Area | Example plugin | Primary seam |
| --- | --- | --- |
| Tools | Issue tracker, database query, deployment, code search | `ctx.tools` |
| Models | OpenAI-compatible or private inference provider | `ctx.llm` |
| Runtime | Remote filesystem, container sandbox, cloud subprocess | capability services |
| Agent behavior | Planning, validation, routing, handoff, subagents | `agent/*` events and agent services |
| Persistence | Team session store, audit archive, transcript exporter | session events and `ctx.sessions` |
| UI | Domain-specific panels, settings, tool-result cards | client plugins and remote APIs |
| Governance | Approval policy, redaction, telemetry, cost controls | policy and telemetry events |

## Planned agent Skills

In this guide, a **Skill** means a reusable instruction workflow for AI coding agents; it is not the same thing as a DeepSeek Harness runtime plugin. The following Skills are planned and are **not yet shipped**:

| Skill | Intended use |
| --- | --- |
| `dsh-repository-explorer` | Map packages, plugin rows, services, events, and host/client ownership before making a change. |
| `dsh-plugin-scaffold` | Create a minimal plugin, configuration schema, patch, tests, and package metadata. |
| `dsh-tool-builder` | Build a typed tool with validation, rendering, policy hooks, and lifecycle-safe registration. |
| `dsh-plugin-review` | Review dependency injection, effect cleanup, permissions, secrets, packaging, and compatibility risk. |

Each future Skill will state when to use it, required context, ordered steps, safety boundaries, verification, and links to the corresponding official contract.

## Recommended learning path

1. Run `dsh web` and complete one normal repository task.
2. Read the official [architecture guide](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md).
3. Build the minimal plugin from the official tutorial.
4. Add a typed tool and a configuration schema.
5. Inspect the effective tree with `dsh --profile web --dump-config`.
6. Package the plugin as a bundle and install it into a disposable profile.
7. Add tests and review permissions before publishing.

## Authoritative resources

- [DeepSeek Harness source](https://github.com/deepseek-ai/deepseek-harness)
- [DeepSeek Harness documentation](https://deepseek-harness.github.io/deepseek-harness/)
- [Architecture](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)
- [Plugin tutorial](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.md)
- [Tool tutorial](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/tool.md)
- [Plugin packaging](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.md)
- [Cordis](https://github.com/cordiverse/cordis)
- [Spatiotemporal composability paper](https://github.com/cordiverse/paper)

## Contributing

Contributions are welcome: corrections, translations, examples, plugin case studies, and reusable Skills. Keep claims linked to official source or reproducible code, clearly label preview APIs, and never include real credentials in examples.

- [Technical guide](GUIDE.md)
- [Contribution guide](CONTRIBUTING.md)
- [Project roadmap](ROADMAP.md)

Current documentation layout:

```text
deepeseek-harness-guide/
├── README*.md        # introductions and language entry points
├── GUIDE*.md         # multilingual technical architecture guides
├── CONTRIBUTING*.md  # source, review, and translation policy
├── ROADMAP*.md       # staged project evolution
└── LICENSE
```

## License

[MIT](LICENSE)
