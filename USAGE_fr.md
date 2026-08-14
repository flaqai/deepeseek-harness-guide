# Manuel d'utilisation de DeepSeek Harness

[English](USAGE.md) · [简体中文](USAGE_zh.md)

Cette page est le guide rapide en français. DeepSeek Harness est encore en préversion développeur ; vérifiez les commandes avec le commit déployé et la documentation officielle.

## Démarrage rapide

```bash
npx @deepseek-ai/dsh web
dsh --profile web --dump-config
```

Ouvrez `http://127.0.0.1:3080`, configurez le service de modèle et commencez dans un espace jetable. La seconde commande affiche l'arbre de plugins résolu depuis le Profile, les Bundles et les Patches.

## Catégories de modules

- Composition du runtime : Context, Service, Fiber, Effect, Event et Loader.
- Exécution de l'agent : adaptateur, Prompt, Agent Loop, outils, politique, approbation et bac à sable.
- État : événements de Session, mémoire, compactage et rejeu.
- Interface : Host, API distante, Web Client, bureau, TUI et mobile.
- Écosystème : workflows, navigateur, vision, intégrations, thèmes et outils de développement.

## Installation sûre

```bash
dsh plugin --profile demo add <package-or-git-spec>
dsh --profile demo --dump-config
```

Épinglez le commit Git et examinez licence, scripts d'installation, réseau, fichiers, sous-processus, secrets et conservation. Testez démarrage, refus, délai, déchargement, redémarrage et retour arrière dans un Profile jetable.

## Skills pratiques

[`skills/`](skills/) contient quatre Agent Skills pour explorer le dépôt, créer un plugin, développer un outil et auditer un plugin. Un Skill guide le développement ; ce n'est pas un plugin du runtime DSH.

Le [manuel anglais complet](USAGE.md) contient les procédures, le dépannage et la checklist de publication.
