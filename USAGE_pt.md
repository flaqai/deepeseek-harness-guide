# Manual de uso do DeepSeek Harness

[English](USAGE.md) · [简体中文](USAGE_zh.md)

Esta é a introdução rápida em português. O DeepSeek Harness está em prévia para desenvolvedores; valide os comandos com o commit implantado e a documentação oficial.

## Início rápido

```bash
npx @deepseek-ai/dsh web
dsh --profile web --dump-config
```

Abra `http://127.0.0.1:3080`, configure o serviço de modelo e comece em um workspace descartável. O segundo comando mostra a árvore de plugins resolvida por Profile, Bundles e Patches.

## Categorias de módulos

- Composição do runtime: Context, Service, Fiber, Effect, Event e Loader.
- Execução do agente: adaptador, Prompt, Agent Loop, ferramentas, política, aprovação e sandbox.
- Estado: eventos de Session, memória, compactação e replay.
- Interface: Host, API remota, Web Client, desktop, TUI e móvel.
- Ecossistema: workflows, navegador, visão, integrações, temas e ferramentas de desenvolvimento.

## Instalação segura

```bash
dsh plugin --profile demo add <package-or-git-spec>
dsh --profile demo --dump-config
```

Fixe o commit Git e revise licença, scripts de instalação, rede, arquivos, subprocessos, credenciais e retenção. Teste início, negação, timeout, descarregamento, reinício e rollback em um Profile descartável.

## Skills práticos

[`skills/`](skills/) contém quatro Agent Skills para explorar o repositório, criar plugins, desenvolver ferramentas e revisar plugins. Um Skill orienta o desenvolvimento; não é um plugin do runtime DSH.

Consulte o [manual completo em inglês](USAGE.md) para procedimentos, diagnóstico e checklist de publicação.
