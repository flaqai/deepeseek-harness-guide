# DeepSeek Harness – Bedienungsanleitung

[English](USAGE.md) · [简体中文](USAGE_zh.md)

Diese Seite ist der deutsche Schnelleinstieg. DeepSeek Harness befindet sich in der Developer Preview; prüfen Sie alle Befehle gegen den eingesetzten Commit und die offizielle Dokumentation.

## Schnellstart

```bash
npx @deepseek-ai/dsh web
dsh --profile web --dump-config
```

Öffnen Sie `http://127.0.0.1:3080`, konfigurieren Sie einen Modelldienst und testen Sie zuerst in einem temporären Workspace. Der zweite Befehl zeigt den aus Profile, Bundles und Patches aufgelösten Plugin-Baum.

## Modulgruppen

- Runtime-Komposition: Context, Service, Fiber, Effect, Event und Loader.
- Agent-Ausführung: Modelladapter, Prompt, Agent Loop, Tools, Richtlinie, Freigabe und Sandbox.
- Zustand: Session Events, Speicher, Komprimierung und Replay.
- Oberfläche: Host, Remote API, Web Client, Desktop, TUI und Mobilgerät.
- Ökosystem: Workflows, Browser, Vision, externe Integrationen, Themes und Entwicklungswerkzeuge.

## Sichere Installation

```bash
dsh plugin --profile demo add <package-or-git-spec>
dsh --profile demo --dump-config
```

Fixieren Sie den Git-Commit und prüfen Sie Lizenz, Installationsskripte, Netzwerk, Dateien, Unterprozesse, Zugangsdaten und Aufbewahrung. Testen Sie Start, Ablehnung, Timeout, Entladen, Neustart und Rollback in einem temporären Profile.

## Praktische Skills

Unter [`skills/`](skills/) liegen vier Agent Skills für Repository-Analyse, Plugin-Gerüst, Tool-Entwicklung und Plugin-Prüfung. Ein Skill steuert Entwicklungsarbeit und ist kein DSH-Runtime-Plugin.

Die vollständigen Abläufe, Fehlersuche und Release-Checkliste stehen im [englischen Handbuch](USAGE.md).
