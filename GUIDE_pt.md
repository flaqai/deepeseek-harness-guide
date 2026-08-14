# Guia técnico do DeepSeek Harness

[English](GUIDE.md) | [简体中文](GUIDE_zh.md) | [繁體中文](GUIDE_tw.md) | [日本語](GUIDE_ja.md) | [한국어](GUIDE_ko.md) | [Deutsch](GUIDE_de.md) | [Español](GUIDE_es.md) | [Français](GUIDE_fr.md) | [Italiano](GUIDE_it.md) | [Português](GUIDE_pt.md) | [Русский](GUIDE_ru.md) | [العربية](GUIDE_ar.md) | [Bahasa Indonesia](GUIDE_id.md) | [ไทย](GUIDE_th.md) | [Tiếng Việt](GUIDE_vi.md)

Este guia parte de uma [análise técnica em chinês](https://mp.weixin.qq.com/s/Kf87hcNdSmY4ODWI4UZ8cg), verificada com o [código oficial](https://github.com/deepseek-ai/deepseek-harness) e a [documentação de arquitetura](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md).

> DeepSeek Harness está em Developer Preview. O artigo analisa Commits fixos; pacotes, Presets e APIs internas podem mudar.

## Modelo central

DSH mantém dois sistemas coordenados:

- **Grafo de plugins em runtime:** capacidades atuais, Scope de visibilidade e ciclo de vida controlado por Fibers.
- **Fluxo append-only de eventos de Session:** fatos persistentes do Agent, projetados no histórico do modelo, UI, Resume e Fork.

O Agent Loop obtém modelos, Prompts, ferramentas e políticas do grafo e grava resultados no fluxo de eventos.

## Composição

`Profile → Bundles → Profile Patch → Home Patch → --patch`

Camadas posteriores substituem uma linha completa por ID ou inserem outra. Primeiro diagnóstico:

```bash
dsh --profile web --dump-config
```

## Runtime Cordis

| Elemento | Papel |
| --- | --- |
| Context | Visibilidade, herança e Realms isolados de Services. |
| Service | Contrato estável entre Definition, Provider e Consumer. |
| Fiber | Instância real do Plugin com configuração, dependências e Disposers. |
| Effect | Associa aquisição e Cleanup de recursos a um Fiber. |
| Event | Estende o fluxo com notificação, decisão ou Waterfall Middleware. |
| Loader | Transforma configuração em uma árvore atualizável e descarregável. |

`inject` é um contrato de dependência do Context, não uma permissão do sistema operacional. `ctx.effect()` estrutura o Cleanup, mas não desfaz transações externas.

## Agent e Session

Um Turn contém zero ou mais Steps; um Step costuma incluir uma solicitação ao modelo e suas ferramentas. Session Events registram limites, mensagens, Chunks, Tool Calls e resultados. `deriveMessages()` projeta o histórico visível ao modelo.

Registro completo não significa reenvio completo. Compaction pode ocultar uma superfície antiga mantendo os eventos. Um log reproduzível não torna efeitos externos seguros para repetição.

## Cache e segurança

Um grafo dinâmico não invalida sozinho a Prefix Cache. A invalidação ocorre quando mudam ferramentas, Prompt, modelo ou histórico visíveis. Mantenha ordenação estável e separe dados voláteis.

Plugins de terceiros são código privilegiado no processo host. Revise scripts de instalação, APIs Node, rede, credenciais, arquivos, subprocessos, telemetria e Cleanup; fixe um Commit.

## Checklist de desenvolvimento

- Usar um Service ou Event Seam antes de alterar o Loop.
- Declarar dependências com `inject` e validar configuração com Schema.
- Dar propriedade e Cleanup a listeners, timers, Services e handles.
- Decidir se o estado pertence ao Host, Agent Scope ou Session Log.
- Testar troca de Provider, update, Unload, Resume, Fork e Compaction.
- Empacotar como Bundle e validar com `--dump-config`.

Consulte as versões [inglesa](GUIDE.md) ou [chinesa](GUIDE_zh.md) para mais detalhes.

