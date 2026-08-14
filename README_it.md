# Guida a DeepSeek Harness

[English](README.md) | [简体中文](README_zh.md) | [繁體中文](README_tw.md) | [日本語](README_ja.md) | [한국어](README_ko.md) | [Deutsch](README_de.md) | [Español](README_es.md) | [Français](README_fr.md) | [Italiano](README_it.md) | [Português](README_pt.md) | [Русский](README_ru.md) | [العربية](README_ar.md) | [Bahasa Indonesia](README_id.md) | [ไทย](README_th.md) | [Tiếng Việt](README_vi.md)

> Una guida multilingue della comunità per comprendere, estendere e creare plugin per [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness).

DeepSeek Harness (`dsh`) è un agent harness open source sviluppato da DeepSeek AI. L'idea centrale è: **tutto è un plugin**. Adattatori dei modelli, strumenti, ciclo dell'agente, sessioni, autorizzazioni, sandbox, telemetria e interfaccia possono essere composti o sostituiti tramite configurazione.

> [!IMPORTANT]
> Questo è un progetto indipendente della comunità, non un repository ufficiale DeepSeek. DeepSeek Harness è in anteprima per sviluppatori e può introdurre modifiche incompatibili. Verifica sempre i dettagli nel [repository ufficiale](https://github.com/deepseek-ai/deepseek-harness) e nella [documentazione ufficiale](https://deepseek-harness.github.io/deepseek-harness/).

## A cosa serve un harness

Un modello da solo non legge repository, esegue comandi, chiama strumenti, richiede approvazioni, conserva sessioni o gestisce il recupero dagli errori. L'harness fornisce questo ambiente operativo e coordina utente, modello, strumenti e stato dell'applicazione.

DeepSeek Harness usa [Cordis](https://github.com/cordiverse/cordis). I plugin aggiungono servizi, eventi tipizzati ed effetti reversibili a un Context condiviso. È quindi possibile sostituire modelli, strumenti, sandbox, archiviazione o sotto-agenti senza mantenere un fork completo.

## Concetti chiave

| Concetto | Significato |
| --- | --- |
| Plugin | Modulo TypeScript, oggetto o classe di servizio montato in un Context Cordis. |
| Bundle | Pacchetto npm che distribuisce un livello di configurazione tramite `dsh.bundle`. |
| Profile | Composizione eseguibile di Bundle e dipendenze locali. |
| Patch | Overlay YAML che inserisce o sostituisce righe di configurazione. |
| Service / Event | Capacità sostituibile e punto di estensione del flusso dell'agente. |

Anche il ciclo dell'agente è sostituibile. Il ciclo predefinito prepara prompt e schemi degli strumenti, riceve in streaming il modello, esegue strumenti e registra eventi persistenti di sessione.

## Avvio rapido

```bash
npx @deepseek-ai/dsh web
```

La Web UI è disponibile per impostazione predefinita su `http://127.0.0.1:3080`. Aggiungi le credenziali in **Settings → Models**, quindi scegli un workspace.

## Contenuti della guida

- Cordis, ciclo di vita dei plugin, dependency injection ed effetti reversibili.
- Plugin per strumenti, modelli, sandbox, archiviazione, sotto-agenti e Web UI.
- Bundle, Profile, `cordis.patch.yml`, test, pubblicazione e sicurezza.
- Agent Skills pianificate: `dsh-repository-explorer`, `dsh-plugin-scaffold`, `dsh-tool-builder` e `dsh-plugin-review`.

Qui una **Skill** è un workflow riutilizzabile per agenti di programmazione, non un **Plugin** runtime di DeepSeek Harness. Queste Skills non sono ancora pubblicate.

## Risorse ufficiali

- [Codice sorgente](https://github.com/deepseek-ai/deepseek-harness)
- [Architettura](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)
- [Primo plugin](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.md)
- [Packaging e installazione](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.md)

## Licenza

[MIT](LICENSE)
