# Guide DeepSeek Harness

[English](README.md) | [简体中文](README_zh.md) | [繁體中文](README_tw.md) | [日本語](README_ja.md) | [한국어](README_ko.md) | [Deutsch](README_de.md) | [Español](README_es.md) | [Français](README_fr.md) | [Italiano](README_it.md) | [Português](README_pt.md) | [Русский](README_ru.md) | [العربية](README_ar.md) | [Bahasa Indonesia](README_id.md) | [ไทย](README_th.md) | [Tiếng Việt](README_vi.md)

📘 [Architecture technique →](GUIDE_fr.md) · [Manuel d'utilisation →](USAGE_fr.md) · [Skills pratiques →](skills/)

> Un guide communautaire multilingue pour comprendre, étendre et créer des plugins pour [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness).

DeepSeek Harness (`dsh`) est un harness d'agents open source développé par DeepSeek AI. Son idée centrale est simple et puissante : **tout est un plugin**. Adaptateurs de modèles, outils, boucle de l'agent, sessions, permissions, bac à sable, télémétrie et interface peuvent être composés ou remplacés par configuration.

> [!IMPORTANT]
> Ce dépôt est un guide communautaire indépendant, et non un dépôt officiel de DeepSeek. DeepSeek Harness est en préversion développeur et peut introduire des changements incompatibles. Vérifiez les détails dans le [dépôt officiel](https://github.com/deepseek-ai/deepseek-harness) et la [documentation officielle](https://deepseek-harness.github.io/deepseek-harness/).

## Pourquoi un harness ?

Un modèle seul ne lit pas un dépôt, n'exécute pas de commandes, n'appelle pas d'outils, ne demande pas d'autorisation et ne conserve pas une session. Le harness fournit cet environnement d'exécution et coordonne l'utilisateur, le modèle, les outils et l'état de l'application.

DeepSeek Harness repose sur [Cordis](https://github.com/cordiverse/cordis). Les plugins ajoutent des services, des événements typés et des effets réversibles à un Context partagé. On peut ainsi remplacer modèle, outils, sandbox, stockage ou sous-agents sans maintenir un fork complet.

## Concepts essentiels

| Concept | Signification |
| --- | --- |
| Plugin | Module TypeScript, objet ou classe de service monté dans un Context Cordis. |
| Bundle | Paquet npm distribuant une couche de configuration via `dsh.bundle`. |
| Profile | Composition exécutable de Bundles et de dépendances locales. |
| Patch | Surcouche YAML qui insère ou remplace des lignes de configuration. |
| Service / Event | Capacité remplaçable et point d'extension du flux de l'agent. |

La boucle de l'agent est elle aussi remplaçable. La boucle par défaut assemble prompts et schémas d'outils, diffuse la réponse du modèle, exécute les outils et enregistre les événements persistants de session.

## Démarrage rapide

```bash
npx @deepseek-ai/dsh web
```

L'interface Web est servie par défaut sur `http://127.0.0.1:3080`. Ajoutez les identifiants dans **Settings → Models**, puis choisissez un espace de travail.

## Contenu prévu

- Cordis, cycle de vie des plugins, injection de dépendances et effets réversibles.
- Plugins d'outils, modèles, sandbox, stockage, sous-agents et interface Web.
- Bundles, Profiles, `cordis.patch.yml`, tests, publication et sécurité.
- Agent Skills disponibles : `dsh-repository-explorer`, `dsh-plugin-scaffold`, `dsh-tool-builder` et `dsh-plugin-review`.

Ici, un **Skill** est un workflow d'instructions réutilisable pour un agent de programmation ; ce n'est pas un **Plugin** d'exécution DeepSeek Harness. Ces Skills se trouvent dans [`skills/`](skills/).

## Ressources officielles

- [Code source](https://github.com/deepseek-ai/deepseek-harness)
- [Architecture](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)
- [Premier plugin](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.md)
- [Empaquetage et installation](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.md)

## Licence

[MIT](LICENSE)
