# Manuale d'uso di DeepSeek Harness

[English](USAGE.md) · [简体中文](USAGE_zh.md)

Questa è la guida rapida in italiano. DeepSeek Harness è ancora in anteprima per sviluppatori: verifica i comandi rispetto al commit distribuito e alla documentazione ufficiale.

## Avvio rapido

```bash
npx @deepseek-ai/dsh web
dsh --profile web --dump-config
```

Apri `http://127.0.0.1:3080`, configura il servizio del modello e prova prima in un workspace temporaneo. Il secondo comando mostra l'albero dei plugin risolto da Profile, Bundle e Patch.

## Categorie di moduli

- Composizione runtime: Context, Service, Fiber, Effect, Event e Loader.
- Esecuzione agent: adattatore, Prompt, Agent Loop, strumenti, policy, approvazione e sandbox.
- Stato: eventi Session, memoria, compattazione e replay.
- Interfaccia: Host, API remota, Web Client, desktop, TUI e mobile.
- Ecosistema: workflow, browser, visione, integrazioni, temi e strumenti di sviluppo.

## Installazione sicura

```bash
dsh plugin --profile demo add <package-or-git-spec>
dsh --profile demo --dump-config
```

Fissa il commit Git e controlla licenza, script di installazione, rete, file, processi secondari, credenziali e conservazione. Prova avvio, rifiuto, timeout, scaricamento, riavvio e rollback in un Profile temporaneo.

## Skill pratici

[`skills/`](skills/) include quattro Agent Skill per esplorare il repository, creare plugin, sviluppare strumenti e revisionare plugin. Uno Skill guida lo sviluppo e non è un plugin del runtime DSH.

Consulta il [manuale completo in inglese](USAGE.md) per procedure, diagnosi e checklist di rilascio.
