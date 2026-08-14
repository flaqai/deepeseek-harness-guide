# Roadmap

[English](ROADMAP.md) | [简体中文](ROADMAP_zh.md)

This roadmap favors small, source-backed, reviewable additions over a large speculative handbook.

## Phase 1 — Foundation

- [x] Establish a multilingual project introduction.
- [x] Support 15 language entry points.
- [x] Add an English and Chinese technical architecture guide.
- [x] Add concise localized technical guides for every supported language.
- [x] Define source, contribution, and translation policies.

## Phase 2 — Reproducible plugin development

- [ ] Add a minimal lifecycle-safe plugin example.
- [ ] Add a typed tool plugin with configuration validation.
- [ ] Add Bundle, Profile, and Patch packaging examples.
- [ ] Add tests for missing dependencies, Provider replacement, config updates, and unload.
- [ ] Add a `--dump-config` troubleshooting walkthrough.

## Phase 3 — Runtime and Session depth

- [ ] Document Context inheritance and isolated Service Realms with runnable examples.
- [ ] Demonstrate Effect cleanup, ordering, and failure behavior.
- [ ] Trace a complete Turn/Step lifecycle from input to tool result.
- [ ] Add Session Resume, Fork, Compaction, and projection examples.
- [ ] Add prompt-surface stability and provider cache experiments.

## Phase 4 — Safety and quality

- [ ] Publish a third-party plugin security review template.
- [ ] Add sandbox and approval-boundary examples.
- [ ] Define compatibility metadata for upstream Commits and DSH releases.
- [ ] Add automated Markdown, link, translation-navigation, and terminology checks.
- [ ] Establish native-speaker translation review labels.

## Phase 5 — Reusable Agent Skills

- [x] `dsh-repository-explorer`
- [x] `dsh-plugin-scaffold`
- [x] `dsh-tool-builder`
- [x] `dsh-plugin-review`

Each Skill must contain trigger guidance, prerequisites, an ordered workflow, safety boundaries, verification steps, and links to the matching official contract.

## Phase 6 — Ecosystem

- [ ] Curate community plugins with source, license, version, and security notes.
- [ ] Add complete plugin case studies pinned to upstream revisions.
- [ ] Document Host/Client and remote API extension patterns.
- [ ] Compare local, remote, and sandboxed capability providers with measured evidence.

The roadmap is directional, not a compatibility promise. Priorities may change as DeepSeek Harness evolves.
