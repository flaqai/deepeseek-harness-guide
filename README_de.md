# DeepSeek Harness Guide

[English](README.md) | [简体中文](README_zh.md) | [繁體中文](README_tw.md) | [日本語](README_ja.md) | [한국어](README_ko.md) | [Deutsch](README_de.md) | [Español](README_es.md) | [Français](README_fr.md) | [Italiano](README_it.md) | [Português](README_pt.md) | [Русский](README_ru.md) | [العربية](README_ar.md) | [Bahasa Indonesia](README_id.md) | [ไทย](README_th.md) | [Tiếng Việt](README_vi.md)

> Ein mehrsprachiger Community-Leitfaden zum Verstehen und Erweitern von [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) sowie zur Entwicklung eigener Plugins.

DeepSeek Harness (`dsh`) ist ein von DeepSeek AI entwickeltes Open-Source-Agent-Harness. Die zentrale Idee lautet: **Alles ist ein Plugin**. Modelladapter, Werkzeuge, Agentenschleife, Sitzungsspeicher, Berechtigungen, Sandbox, Telemetrie und Benutzeroberfläche können per Konfiguration zusammengesetzt oder ersetzt werden.

> [!IMPORTANT]
> Dieses Projekt ist ein unabhängiger Community-Leitfaden und kein offizielles DeepSeek-Repository. DeepSeek Harness befindet sich in der Developer Preview und kann inkompatible Änderungen enthalten. Maßgeblich sind das [offizielle Repository](https://github.com/deepseek-ai/deepseek-harness) und die [offizielle Dokumentation](https://deepseek-harness.github.io/deepseek-harness/).

## Warum ein Harness?

Ein Modell allein liest kein Repository, führt keine Befehle aus, ruft keine Werkzeuge auf, fordert keine Freigaben an und speichert keine Sitzungen. Das Harness stellt diese Laufzeit bereit und koordiniert Benutzer, Modell, Werkzeuge und Anwendungszustand.

DeepSeek Harness basiert auf [Cordis](https://github.com/cordiverse/cordis). Plugins tragen Services, typisierte Events und reversible Effekte zu einem gemeinsamen Context bei. So lassen sich Modelle, Werkzeuge, Sandboxes, Speicher oder Subagenten austauschen, ohne die gesamte Anwendung zu forken.

## Kernbegriffe

| Begriff | Bedeutung |
| --- | --- |
| Plugin | TypeScript-Modul, Objekt oder Serviceklasse, die in einen Cordis Context eingebunden wird. |
| Bundle | npm-Paket, das über `dsh.bundle` eine Konfigurationsschicht ausliefert. |
| Profile | Startfähige Komposition aus Bundles und lokalen Plugin-Abhängigkeiten. |
| Patch | YAML-Overlay zum Einfügen oder Ersetzen von Konfigurationszeilen. |
| Service / Event | Austauschbare Fähigkeit bzw. Erweiterungspunkt im Agentenablauf. |

Auch die Agentenschleife ist austauschbar. Der Standardablauf erstellt Prompts und Tool-Schemas, streamt Modellantworten, führt Werkzeuge aus und protokolliert dauerhafte Sitzungsereignisse.

## Schnellstart

```bash
npx @deepseek-ai/dsh web
```

Die Weboberfläche läuft standardmäßig unter `http://127.0.0.1:3080`. Hinterlege unter **Settings → Models** Zugangsdaten und wähle anschließend einen Workspace.

## Inhalt dieses Leitfadens

- Cordis, Plugin-Lebenszyklus, Dependency Injection und reversible Effekte.
- Plugins für Tools, Modelle, Sandboxes, Speicher, Subagenten und Web UI.
- Bundles, Profiles, `cordis.patch.yml`, Tests, Veröffentlichung und Sicherheit.
- Geplante Agent Skills: `dsh-repository-explorer`, `dsh-plugin-scaffold`, `dsh-tool-builder` und `dsh-plugin-review`.

Ein **Skill** ist hier ein wiederverwendbarer Arbeitsablauf für KI-Coding-Agenten und nicht dasselbe wie ein DeepSeek-Harness-**Plugin**. Die genannten Skills sind noch nicht veröffentlicht.

## Offizielle Ressourcen

- [Quellcode](https://github.com/deepseek-ai/deepseek-harness)
- [Architektur](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)
- [Erstes Plugin](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.md)
- [Plugin paketieren und installieren](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.md)

## Lizenz

[MIT](LICENSE)
