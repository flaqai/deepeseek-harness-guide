# DeepSeek Harness Usage Handbook

[简体中文](USAGE_zh.md) · [Language index](#other-languages)

This handbook turns the DeepSeek Harness architecture into operating steps. Commands and package contracts may change while the official project remains in developer preview; verify them against the revision you deploy.

## Choose the right entry point

| Goal | Start with | Why |
|---|---|---|
| Use DSH interactively | Official Web Profile | Fastest way to configure a model and inspect sessions |
| Run automation or embed a runtime | Headless Profile or official Python SDK | Removes the browser from the execution path |
| Add one capability | Runtime plugin | Smallest lifecycle-managed extension |
| Distribute plugins and configuration | Bundle | Versioned set of ordered configuration rows |
| Assemble a product or environment | Profile | Named composition of Bundles and local dependencies |
| Override a deployment | Patch | Late configuration insertion or replacement |
| Change only agent behavior | Preset, prompt, or policy plugin | Avoids creating another process architecture |
| Add reusable coding-agent instructions | Agent Skill | Guides a coding agent; does not run inside DSH by itself |

## 1. Run the official Web UI

Use a currently supported Node.js release listed by the official project, then start the published package:

```bash
npx @deepseek-ai/dsh web
```

Open `http://127.0.0.1:3080`, configure a model service in Settings, create a disposable workspace, and run a harmless prompt before enabling external plugins.

Baseline checks:

```bash
dsh --help
dsh --profile web --dump-config
```

The first command confirms the installed CLI surface. The second prints the resolved plugin tree and is the primary check for Bundle and Patch order.

## 2. Run from source

Use this path for source reading, plugin development, or revision-pinned testing:

```bash
git clone https://github.com/deepseek-ai/deepseek-harness.git
cd deepseek-harness
pnpm install
pnpm run build
pnpm dsh web
```

Before changing code, read the repository `AGENTS.md`, record the commit, inspect the workspace packages, and run the nearest existing tests. Developer-preview documentation and third-party tutorials may describe a different commit.

## 3. Use the Python SDK

The official SDK is intended for programmatic embedding and includes its runtime. Check the official SDK guide for supported Python versions and platforms before installation:

```bash
python -m venv .venv
. .venv/bin/activate
python -m pip install deepseek-harness-sdk
```

Keep credentials in environment or secret storage, not source files. Start with a minimal non-destructive request, set timeouts, capture structured errors, and define how the host cancels and closes the runtime.

## 4. Understand the module map

### Runtime composition

- **Cordis Context** controls capability visibility and lifecycle ownership.
- **Service** is a typed capability boundary between providers and consumers.
- **Fiber** is one mounted plugin instance.
- **Effect** records reversible cleanup owned by that Fiber.
- **Event** supports notification and interception across plugins.
- **Loader** reconciles ordered configuration into the live plugin graph.

### Agent execution

- **Model adapter** converts canonical requests and streaming responses.
- **Prompt/context assembly** constructs stable model-visible input.
- **Agent Loop** alternates model output, tool execution, and continuation.
- **Tool registry and pipeline** validate, authorize, execute, and render tools.
- **Policy, approval, and sandbox** are separate controls; one does not imply the others.
- **Subagents and workflows** coordinate additional execution graphs.

### State and presentation

- **Session event log** is the durable source of model-visible history.
- **Compaction and memory** derive smaller context without silently rewriting facts.
- **Host** owns runtime capabilities and privileged operations.
- **Client/Surface** observes remote state and renders Web or other interfaces.
- **Telemetry and operations** expose health, traces, failures, and cleanup signals.

## 5. Select an extension category

| Category | Typical modules | Key review question |
|---|---|---|
| Workflow and Agent | Loop, research, planning, teams, automation | Who owns cancellation, budget, and completion? |
| Context and Session | memory, compaction, input, rewind, prompt management | Can model-visible state be replayed from events? |
| Tools and integration | filesystem, shell, browser, MCP, external APIs | Where are validation, approval, and sandbox enforced? |
| Browser, vision, and UI | Web client, remote API, OCR, cards, notifications | Is the UI derived from canonical host state? |
| Client and distribution | desktop, TUI, mobile remote, container image | Is this a plugin, a client, a bridge, or a runtime fork? |
| Theme and presentation | client styles and visual components | Does it avoid runtime privileges it does not need? |
| Development and operations | scaffolds, validators, payload inspection, observability | Does diagnostic output redact secrets and session data? |

Do not call every ecosystem project a plugin. A runtime fork repackages DSH, a client connects to it, a bridge translates protocols, and a plugin mounts inside its configured graph. Their installation, trust, update, and rollback models differ.

## 6. Install an external plugin safely

Use a disposable Profile first. Review the source, license, package scripts, requested Services, direct Node.js access, network destinations, subprocesses, data retention, and compatibility with your pinned DSH revision.

```bash
dsh plugin --profile demo add <package-or-git-spec>
dsh --profile demo --dump-config
```

For Git sources, pin a commit instead of a moving branch. If the package manager asks to authorize build scripts, stop and inspect them before approval. Then:

1. confirm the expected rows and ordering in the dumped configuration;
2. start only the disposable Profile;
3. test startup, normal use, denial, timeout, unload, restart, and data persistence;
4. verify logs and UI results contain no secrets;
5. document removal and rollback before promoting the plugin.

Inclusion in an ecosystem list is discovery, not a security endorsement.

## 7. Develop a plugin

Start with a narrow plugin and a local Patch:

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

```yaml
- insert:
    - id: hello
      name: /absolute/path/to/hello-plugin.ts
```

Run the source checkout with the overlay:

```bash
pnpm dsh web --patch ./path/to/cordis.yml
```

Production work should also add a typed configuration schema, explicit `inject`, lifecycle-safe registrations, focused tests, and packaging metadata. Inspect the real neighboring plugin pattern because preview APIs may move.

## 8. Build a model-callable tool

Treat a tool as six contracts:

1. strict input schema;
2. authorization and approval decision;
3. bounded execution with cancellation and timeout;
4. canonical structured result;
5. concise, deterministic model rendering;
6. durable Session event when the result remains model-visible.

Test schema rejection, denied actions, repeated calls, partial failure, secret redaction, oversized output, cancellation, and disposal. Schema validation is not authorization, and dependency injection is not sandboxing.

## 9. Troubleshoot in graph order

1. Pin and record the DSH and plugin revisions.
2. Dump the resolved configuration.
3. Confirm Profile, Bundle, Patch order, and the plugin row.
4. Confirm required Services are available and `inject` names are correct.
5. Inspect Fiber mount, Event registration, and Effect disposal.
6. Inspect Agent Loop, tool pipeline, and Session events.
7. Inspect remote APIs and client projections last.

This order separates configuration failures from lifecycle, execution, persistence, and presentation failures.

## 10. Use the repository Skills

This project ships four coding-agent Skills under [`skills/`](skills/):

- `dsh-repository-explorer` maps packages, configuration, Services, Events, Session ownership, and host/client boundaries.
- `dsh-plugin-scaffold` creates a narrow lifecycle-safe plugin and optional Bundle/Profile/Patch packaging.
- `dsh-tool-builder` designs typed, policy-aware, replayable tools.
- `dsh-plugin-review` audits lifecycle, compatibility, supply chain, permissions, secrets, and replay risk.

Copy a complete Skill directory into the Skill location supported by your coding agent, or keep it project-local when that agent discovers repository Skills. A Skill guides development work; it is not installed with `dsh plugin` and does not become a runtime plugin.

## Release checklist

- [ ] Pin DSH and dependency versions.
- [ ] Verify the resolved plugin tree.
- [ ] Test startup, failure, cancellation, unload, remount, and restart.
- [ ] Confirm model-visible state is durably reconstructable.
- [ ] Separate dependency injection, approval, policy, and sandbox claims.
- [ ] Review install scripts, filesystem, network, subprocess, telemetry, and retention.
- [ ] Remove credentials, private sessions, screenshots, QR codes, and contact details from documentation and fixtures.
- [ ] Document compatibility, license, installation, removal, migration, and rollback.

## Other languages

[简体中文](USAGE_zh.md) · [繁體中文](USAGE_tw.md) · [日本語](USAGE_ja.md) · [한국어](USAGE_ko.md) · [Deutsch](USAGE_de.md) · [Español](USAGE_es.md) · [Français](USAGE_fr.md) · [Italiano](USAGE_it.md) · [Português](USAGE_pt.md) · [Русский](USAGE_ru.md) · [العربية](USAGE_ar.md) · [Bahasa Indonesia](USAGE_id.md) · [ไทย](USAGE_th.md) · [Tiếng Việt](USAGE_vi.md)

## Sources

- [Official DeepSeek Harness repository](https://github.com/deepseek-ai/deepseek-harness)
- [Official architecture documentation](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)
- [Official first-plugin tutorial](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.md)
- [Official plugin packaging guide](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.md)
- [Community ecosystem classification reference](https://github.com/libukai/awesome-deepseek-harness) — used only as a text-based category reference; no screenshots or contact details are reproduced.
