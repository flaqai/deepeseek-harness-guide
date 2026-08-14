# Guida tecnica a DeepSeek Harness

[English](GUIDE.md) | [简体中文](GUIDE_zh.md) | [繁體中文](GUIDE_tw.md) | [日本語](GUIDE_ja.md) | [한국어](GUIDE_ko.md) | [Deutsch](GUIDE_de.md) | [Español](GUIDE_es.md) | [Français](GUIDE_fr.md) | [Italiano](GUIDE_it.md) | [Português](GUIDE_pt.md) | [Русский](GUIDE_ru.md) | [العربية](GUIDE_ar.md) | [Bahasa Indonesia](GUIDE_id.md) | [ไทย](GUIDE_th.md) | [Tiếng Việt](GUIDE_vi.md)

Questa guida prende spunto da un'[analisi tecnica in cinese](https://mp.weixin.qq.com/s/Kf87hcNdSmY4ODWI4UZ8cg), verificata con il [codice ufficiale](https://github.com/deepseek-ai/deepseek-harness) e la [documentazione dell'architettura](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md).

> DeepSeek Harness è in Developer Preview. L'articolo analizza Commit fissi; pacchetti, Preset e API interne possono cambiare.

## Modello centrale

DSH mantiene due sistemi coordinati:

- **Grafo runtime dei plugin**: capacità presenti, Scope di visibilità e ciclo di vita posseduto dai Fiber.
- **Flusso append-only degli eventi di Session**: fatti persistenti dell'Agent, proiettati nella cronologia del modello, UI, Resume e Fork.

L'Agent Loop ottiene modelli, Prompt, strumenti e policy dal grafo e scrive i risultati nel flusso di eventi.

## Composizione

`Profile → Bundles → Profile Patch → Home Patch → --patch`

Gli strati successivi sostituiscono un'intera riga per ID o ne inseriscono una nuova. Prima diagnosi:

```bash
dsh --profile web --dump-config
```

## Runtime Cordis

| Elemento | Ruolo |
| --- | --- |
| Context | Visibilità, ereditarietà e Realm isolati dei Service. |
| Service | Contratto stabile fra Definition, Provider e Consumer. |
| Fiber | Istanza reale del Plugin con configurazione, dipendenze e Disposer. |
| Effect | Associa risorse e Cleanup a un Fiber. |
| Event | Estende il flusso con notifiche, decisioni o Waterfall Middleware. |
| Loader | Converte la configurazione in un albero aggiornabile e scaricabile. |

`inject` è un contratto di dipendenza del Context, non un permesso del sistema operativo. `ctx.effect()` struttura il Cleanup ma non annulla transazioni esterne.

## Agent e Session

Un Turn contiene zero o più Step; uno Step include in genere una richiesta al modello e i relativi strumenti. I Session Event registrano confini, messaggi, Chunk, Tool Call e risultati. `deriveMessages()` proietta la cronologia visibile al modello.

Registrazione completa non significa reinvio completo. Compaction può nascondere una vecchia superficie conservando gli eventi. Un log riproducibile non rende sicuri gli effetti esterni ripetuti.

## Cache e sicurezza

Un grafo dinamico non invalida da solo la Prefix Cache. L'invalidazione avviene quando cambiano strumenti, Prompt, modello o cronologia visibili. Mantieni l'ordine stabile e separa i dati volatili.

I Plugin di terze parti sono codice privilegiato nel processo host. Controlla script di installazione, API Node, rete, credenziali, file, sottoprocessi, telemetria e Cleanup; blocca un Commit.

## Checklist di sviluppo

- Usare un Service o Event Seam prima di modificare il Loop.
- Dichiarare dipendenze con `inject` e validare la configurazione con Schema.
- Assegnare proprietà e Cleanup a listener, timer, Service e handle.
- Decidere se lo stato appartiene a Host, Agent Scope o Session Log.
- Testare Provider swap, update, Unload, Resume, Fork e Compaction.
- Distribuire come Bundle e verificare con `--dump-config`.

Per i dettagli consulta la versione [inglese](GUIDE.md) o [cinese](GUIDE_zh.md).

