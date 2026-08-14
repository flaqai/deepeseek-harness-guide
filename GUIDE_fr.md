# Guide technique DeepSeek Harness

[English](GUIDE.md) | [简体中文](GUIDE_zh.md) | [繁體中文](GUIDE_tw.md) | [日本語](GUIDE_ja.md) | [한국어](GUIDE_ko.md) | [Deutsch](GUIDE_de.md) | [Español](GUIDE_es.md) | [Français](GUIDE_fr.md) | [Italiano](GUIDE_it.md) | [Português](GUIDE_pt.md) | [Русский](GUIDE_ru.md) | [العربية](GUIDE_ar.md) | [Bahasa Indonesia](GUIDE_id.md) | [ไทย](GUIDE_th.md) | [Tiếng Việt](GUIDE_vi.md)

Ce guide s'appuie sur une [analyse technique en chinois](https://mp.weixin.qq.com/s/Kf87hcNdSmY4ODWI4UZ8cg), vérifiée avec le [code officiel](https://github.com/deepseek-ai/deepseek-harness) et la [documentation d'architecture](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md).

> DeepSeek Harness est en Developer Preview. L'article analyse des Commits fixes ; packages, Presets et API internes peuvent évoluer.

## Modèle central

DSH maintient deux systèmes coordonnés :

- **Graphe de plugins à l'exécution** : capacités présentes, Scope de visibilité et cycle de vie possédé par les Fibers.
- **Flux append-only d'événements de Session** : faits durables de l'Agent, projetés vers l'historique du modèle, l'UI, Resume et Fork.

L'Agent Loop obtient modèles, Prompts, outils et politiques depuis le graphe, puis écrit les résultats dans le flux d'événements.

## Composition

`Profile → Bundles → Profile Patch → Home Patch → --patch`

Les couches tardives remplacent une ligne complète par ID ou en insèrent une nouvelle. Premier diagnostic :

```bash
dsh --profile web --dump-config
```

## Runtime Cordis

| Élément | Rôle |
| --- | --- |
| Context | Visibilité, héritage et Realms isolés des Services. |
| Service | Contrat stable entre Definition, Provider et Consumer. |
| Fiber | Instance réelle du Plugin avec configuration, dépendances et Disposers. |
| Effect | Associe acquisition et nettoyage des ressources à un Fiber. |
| Event | Étend le flux par notification, décision ou Waterfall Middleware. |
| Loader | Transforme la configuration en arbre actualisable et démontable. |

`inject` est un contrat de dépendance du Context, pas une permission système. `ctx.effect()` structure le nettoyage sans annuler les transactions externes.

## Agent et Session

Un Turn contient zéro ou plusieurs Steps ; un Step couvre généralement une requête modèle et les outils associés. Les Session Events enregistrent limites, messages, Chunks, Tool Calls et résultats. `deriveMessages()` projette l'historique visible par le modèle.

Enregistrement complet ne signifie pas renvoi complet. Compaction peut masquer une ancienne surface tout en conservant les événements. Un journal rejouable ne rend pas les effets externes répétables sans risque.

## Cache et sécurité

Un graphe dynamique n'invalide pas automatiquement le cache de préfixe. Il est invalidé lorsque changent outils, Prompt, modèle ou historique visibles. Gardez un ordre stable et isolez les données volatiles.

Un Plugin tiers est du code privilégié dans le processus hôte. Vérifiez scripts d'installation, API Node, réseau, secrets, fichiers, sous-processus, télémétrie et Cleanup ; épinglez un Commit.

## Vérifications de développement

- Utiliser un Service ou Event Seam avant de modifier la Loop.
- Déclarer les dépendances avec `inject` et valider la configuration par Schema.
- Donner propriété et Cleanup aux listeners, timers, Services et handles.
- Choisir si l'état appartient au Host, à l'Agent Scope ou au Session Log.
- Tester Provider swap, mise à jour, Unload, Resume, Fork et Compaction.
- Publier en Bundle et vérifier avec `--dump-config`.

Consultez les versions [anglaise](GUIDE.md) ou [chinoise](GUIDE_zh.md) pour le détail.

