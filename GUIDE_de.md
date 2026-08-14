# Technischer Leitfaden zu DeepSeek Harness

[English](GUIDE.md) | [简体中文](GUIDE_zh.md) | [繁體中文](GUIDE_tw.md) | [日本語](GUIDE_ja.md) | [한국어](GUIDE_ko.md) | [Deutsch](GUIDE_de.md) | [Español](GUIDE_es.md) | [Français](GUIDE_fr.md) | [Italiano](GUIDE_it.md) | [Português](GUIDE_pt.md) | [Русский](GUIDE_ru.md) | [العربية](GUIDE_ar.md) | [Bahasa Indonesia](GUIDE_id.md) | [ไทย](GUIDE_th.md) | [Tiếng Việt](GUIDE_vi.md)

Dieser Leitfaden basiert auf einer [chinesischen Architekturanalyse](https://mp.weixin.qq.com/s/Kf87hcNdSmY4ODWI4UZ8cg) und wurde mit dem [offiziellen Quellcode](https://github.com/deepseek-ai/deepseek-harness) sowie der [Architekturdokumentation](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md) abgeglichen.

> DeepSeek Harness befindet sich in der Developer Preview. Der Artikel untersucht feste Commits; Paketnamen, Presets und interne APIs können sich ändern.

## Zentrales Modell

DSH verwaltet zwei gekoppelte Systeme:

- **Runtime-Plugin-Graph:** Welche Fähigkeiten existieren, in welchem Scope sie sichtbar sind und welcher Fiber ihren Lebenszyklus besitzt.
- **Append-only Session Event Stream:** Dauerhafte Fakten über die Agent-Ausführung, projiziert in Modellverlauf, UI, Resume und Forks.

Der Agent Loop bezieht Modelle, Prompts, Tools und Richtlinien aus dem Graphen und schreibt Ergebnisse in den Event Stream zurück.

## Kompositionspipeline

`Profile → Bundles → Profile Patch → Home Patch → --patch`

Spätere Ebenen ersetzen vollständige Konfigurationszeilen per ID oder fügen neue ein. Die erste Diagnose sollte sein:

```bash
dsh --profile web --dump-config
```

## Cordis-Runtime

| Element | Aufgabe |
| --- | --- |
| Context | Sichtbarkeit, Vererbung und isolierte Realms von Services. |
| Service | Stabiler Vertrag zwischen Definition, Provider und Consumer. |
| Fiber | Konkrete Plugin-Instanz mit Konfiguration, Abhängigkeiten und Disposern. |
| Effect | Ordnet Ressourcenerwerb und Bereinigung einem Fiber zu. |
| Event | Erweitert Abläufe durch Benachrichtigung, Entscheidung oder Waterfall-Middleware. |
| Loader | Überführt Konfiguration in einen aktualisierbaren Plugin-Baum. |

`inject` ist ein Context-Abhängigkeitsvertrag, keine Betriebssystemberechtigung. `ctx.effect()` bietet strukturierte Bereinigung, aber keinen automatischen Rollback externer Transaktionen.

## Agent und Session

Ein Turn enthält null oder mehr Steps; ein Step umfasst meist eine Modellanfrage und zugehörige Tool-Aufrufe. Session Events protokollieren Grenzen, Nachrichten, Chunks, Tool Calls und Ergebnisse. `deriveMessages()` projiziert daraus den modellseitigen Verlauf.

Vollständige Aufzeichnung bedeutet nicht vollständiges erneutes Senden. Compaction kann alte Oberflächen verdecken und Original-Events erhalten. Ein abspielbares Protokoll macht externe Nebenwirkungen nicht automatisch wiederholbar.

## Cache und Sicherheit

Ein dynamischer Graph zerstört Prefix-Caching nicht von selbst. Erst Änderungen an sichtbaren Tools, Prompts, Modellen oder Verlauf prägen einen neuen Präfix. Reihenfolgen sollten stabil und volatile Daten isoliert sein.

Drittanbieter-Plugins sind privilegierter Code im Host-Prozess. Prüfe Installationsskripte, Node-APIs, Netzwerk, Zugangsdaten, Dateizugriff, Subprozesse, Telemetrie und Cleanup; pinne einen Commit.

## Entwicklungscheckliste

- Vor Änderungen am Loop vorhandene Service- oder Event-Seams nutzen.
- Abhängigkeiten mit `inject`, Konfiguration mit Schemas deklarieren.
- Listener, Timer, Services und Handles einem Lebenszyklus zuordnen.
- Zustand bewusst Host, Agent Scope oder Session Log zuweisen.
- Provider-Tausch, Update, Unload, Resume, Fork und Compaction testen.
- Als Bundle paketieren und mit `--dump-config` prüfen.

Weitere Details stehen in der [englischen](GUIDE.md) oder [chinesischen Fassung](GUIDE_zh.md).

