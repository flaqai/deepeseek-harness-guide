# Contributing

[English](CONTRIBUTING.md) | [简体中文](CONTRIBUTING_zh.md)

Thank you for helping make the DeepSeek Harness Guide accurate, practical, and accessible.

## Contribution areas

- Correct technical claims against current upstream source.
- Add small, reproducible plugin examples and tests.
- Improve architecture diagrams, glossaries, and troubleshooting guides.
- Review or improve translations.
- Document a community plugin with its version, security assumptions, and verification steps.
- Develop one of the planned Agent Skills.

## Source policy

Prefer primary, versioned sources:

1. DeepSeek Harness source and official documentation.
2. Cordis source and paper.
3. Reproducible experiments pinned to a Commit.
4. Secondary articles for interpretation and discovery.

When behavior is internal or version-sensitive, include an upstream file link and Commit SHA. Clearly label inference, observed behavior, preview API, and roadmap claims. Do not turn an article's interpretation into an unqualified project guarantee.

## Documentation structure

| File | Purpose |
| --- | --- |
| `README*.md` | Project introduction, quick start, and navigation. |
| `GUIDE*.md` | Technical architecture, lifecycle, Session, security, and author guidance. |
| `ROADMAP*.md` | Planned project evolution. |
| Future `examples/` | Reproducible plugins and test fixtures. |
| Future `skills/` | Reusable Agent Skills for DSH development. |

## Translation workflow

English is the structural source, while Simplified Chinese is the primary Chinese technical edition. Other languages should preserve the same concepts and safety warnings even when they use a concise form.

For a conceptual or navigation change:

1. Update `README.md` or `GUIDE.md`.
2. Update the paired Simplified Chinese file.
3. Update every affected locale or open a clearly labeled translation follow-up.
4. Keep all language switchers and local technical-guide links valid.
5. Do not translate code identifiers, commands, package names, event names, or configuration keys.

Use these terms consistently: Plugin, Bundle, Profile, Patch, Preset, Context, Service, Provider, Consumer, Fiber, Effect, Event, Loader, Turn, Step, Session, Scope, Realm, and Seam.

Machine-assisted translation is welcome as a draft. Mark translations that still need native-speaker review in the pull request description.

## Technical contribution checklist

- Pin the DeepSeek Harness revision used for research or testing.
- Include prerequisites and exact commands.
- Use placeholder credentials only.
- Explain the expected output and failure mode.
- Test load, dependency absence, configuration update, Provider replacement, unload, and restart where relevant.
- Test Resume, Fork, and Compaction for Session-related changes.
- Document permissions, network access, filesystem scope, subprocesses, install scripts, telemetry, and retained data.
- Verify the effective configuration with `dsh --dump-config`.

## Style

- Lead with the outcome or concept, then explain mechanics.
- Distinguish fact, inference, recommendation, and planned work.
- Prefer small runnable examples over long speculative snippets.
- Link to authoritative sources close to the claim they support.
- Avoid copying long passages from articles or official documentation; summarize and attribute instead.
- Keep preview warnings visible.

## Before submitting

Check that:

- Markdown code fences are balanced.
- Relative links resolve.
- Language navigation includes all supported locales.
- No secrets, tokens, personal paths, or private endpoints are present.
- New files have a clear owner and maintenance purpose.
- The change is focused enough to review.

By contributing, you agree that your work is provided under this repository's [MIT License](LICENSE).

