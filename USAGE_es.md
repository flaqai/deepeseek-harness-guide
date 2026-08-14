# Manual de uso de DeepSeek Harness

[English](USAGE.md) · [简体中文](USAGE_zh.md)

Esta página ofrece una guía rápida en español. DeepSeek Harness sigue en vista previa para desarrolladores; verifica los comandos con el commit desplegado y la documentación oficial.

## Inicio rápido

```bash
npx @deepseek-ai/dsh web
dsh --profile web --dump-config
```

Abre `http://127.0.0.1:3080`, configura el servicio de modelos y prueba primero en un espacio desechable. El segundo comando muestra el árbol de plugins resuelto a partir de Profile, Bundles y Patches.

## Categorías de módulos

- Composición del runtime: Context, Service, Fiber, Effect, Event y Loader.
- Ejecución del agente: adaptador, Prompt, Agent Loop, herramientas, política, aprobación y sandbox.
- Estado: eventos de Session, memoria, compactación y reproducción.
- Interfaz: Host, API remota, Web Client, escritorio, TUI y móvil.
- Ecosistema: flujos, navegador, visión, integraciones, temas y herramientas de desarrollo.

## Instalación segura

```bash
dsh plugin --profile demo add <package-or-git-spec>
dsh --profile demo --dump-config
```

Fija el commit de Git y revisa licencia, scripts de instalación, red, archivos, subprocesos, credenciales y retención de datos. Prueba arranque, denegación, tiempo límite, descarga, reinicio y reversión en un Profile desechable.

## Skills prácticos

[`skills/`](skills/) contiene cuatro Agent Skills para explorar el repositorio, crear plugins, desarrollar herramientas y revisar plugins. Un Skill guía al agente de desarrollo; no es un plugin del runtime DSH.

Consulta el [manual completo en inglés](USAGE.md) para los procedimientos, diagnóstico y lista de publicación.
