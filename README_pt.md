# Guia do DeepSeek Harness

[English](README.md) | [简体中文](README_zh.md) | [繁體中文](README_tw.md) | [日本語](README_ja.md) | [한국어](README_ko.md) | [Deutsch](README_de.md) | [Español](README_es.md) | [Français](README_fr.md) | [Italiano](README_it.md) | [Português](README_pt.md) | [Русский](README_ru.md) | [العربية](README_ar.md) | [Bahasa Indonesia](README_id.md) | [ไทย](README_th.md) | [Tiếng Việt](README_vi.md)

📘 [Ler o guia técnico de arquitetura →](GUIDE_pt.md)

> Um guia comunitário e multilíngue para entender, estender e criar plugins para o [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness).

DeepSeek Harness (`dsh`) é um harness de agentes open source desenvolvido pela DeepSeek AI. Sua ideia central é: **tudo é um plugin**. Adaptadores de modelos, ferramentas, loop do agente, sessões, permissões, sandbox, telemetria e interface podem ser compostos ou substituídos por configuração.

> [!IMPORTANT]
> Este é um guia comunitário independente, não um repositório oficial da DeepSeek. O DeepSeek Harness está em prévia para desenvolvedores e pode introduzir mudanças incompatíveis. Confirme os detalhes no [repositório oficial](https://github.com/deepseek-ai/deepseek-harness) e na [documentação oficial](https://deepseek-harness.github.io/deepseek-harness/).

## Por que um harness?

Um modelo sozinho não lê repositórios, executa comandos, chama ferramentas, solicita aprovações, preserva sessões nem se recupera de falhas. O harness fornece esse ambiente operacional e coordena usuário, modelo, ferramentas e estado da aplicação.

O DeepSeek Harness é baseado no [Cordis](https://github.com/cordiverse/cordis). Plugins adicionam serviços, eventos tipados e efeitos reversíveis a um Context compartilhado. Assim, modelos, ferramentas, sandboxes, armazenamento e subagentes podem ser trocados sem manter um fork completo.

## Conceitos principais

| Conceito | Significado |
| --- | --- |
| Plugin | Módulo TypeScript, objeto ou classe de serviço montado em um Context Cordis. |
| Bundle | Pacote npm que entrega uma camada de configuração via `dsh.bundle`. |
| Profile | Composição executável de Bundles e dependências locais. |
| Patch | Camada YAML que insere ou substitui linhas de configuração. |
| Service / Event | Capacidade substituível e ponto de extensão do fluxo do agente. |

O próprio loop do agente também é substituível. O padrão monta prompts e esquemas de ferramentas, transmite a resposta do modelo, executa ferramentas e registra eventos persistentes de sessão.

## Início rápido

```bash
npx @deepseek-ai/dsh web
```

A interface Web é servida por padrão em `http://127.0.0.1:3080`. Adicione credenciais em **Settings → Models** e escolha um workspace.

## Conteúdo deste guia

- Cordis, ciclo de vida de plugins, injeção de dependências e efeitos reversíveis.
- Plugins de ferramentas, modelos, sandbox, armazenamento, subagentes e Web UI.
- Bundles, Profiles, `cordis.patch.yml`, testes, publicação e segurança.
- Agent Skills planejadas: `dsh-repository-explorer`, `dsh-plugin-scaffold`, `dsh-tool-builder` e `dsh-plugin-review`.

Aqui, uma **Skill** é um fluxo reutilizável de instruções para agentes de programação; não é o mesmo que um **Plugin** de runtime do DeepSeek Harness. Essas Skills ainda não foram publicadas.

## Recursos oficiais

- [Código-fonte](https://github.com/deepseek-ai/deepseek-harness)
- [Arquitetura](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)
- [Primeiro plugin](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.md)
- [Empacotamento e instalação](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.md)

## Licença

[MIT](LICENSE)
