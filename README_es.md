# Guía de DeepSeek Harness

[English](README.md) | [简体中文](README_zh.md) | [繁體中文](README_tw.md) | [日本語](README_ja.md) | [한국어](README_ko.md) | [Deutsch](README_de.md) | [Español](README_es.md) | [Français](README_fr.md) | [Italiano](README_it.md) | [Português](README_pt.md) | [Русский](README_ru.md) | [العربية](README_ar.md) | [Bahasa Indonesia](README_id.md) | [ไทย](README_th.md) | [Tiếng Việt](README_vi.md)

> Una guía comunitaria y multilingüe para comprender, ampliar y crear plugins para [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness).

DeepSeek Harness (`dsh`) es un harness de agentes de código abierto desarrollado por DeepSeek AI. Su idea central es: **todo es un plugin**. Los adaptadores de modelos, las herramientas, el bucle del agente, las sesiones, los permisos, el sandbox, la telemetría y la interfaz se pueden componer o sustituir mediante configuración.

> [!IMPORTANT]
> Este es un proyecto comunitario independiente, no un repositorio oficial de DeepSeek. DeepSeek Harness está en vista previa para desarrolladores y puede introducir cambios incompatibles. Consulta siempre el [repositorio oficial](https://github.com/deepseek-ai/deepseek-harness) y la [documentación oficial](https://deepseek-harness.github.io/deepseek-harness/).

## Por qué importa el harness

Un modelo por sí solo no lee repositorios, ejecuta comandos, llama herramientas, solicita permisos, conserva sesiones ni se recupera de errores. El harness proporciona ese entorno de ejecución y coordina al usuario, el modelo, las herramientas y el estado de la aplicación.

DeepSeek Harness utiliza [Cordis](https://github.com/cordiverse/cordis). Los plugins aportan servicios, eventos tipados y efectos reversibles a un Context compartido. Esto permite cambiar modelos, herramientas, sandboxes, almacenamiento o subagentes sin mantener un fork completo.

## Conceptos principales

| Concepto | Significado |
| --- | --- |
| Plugin | Módulo TypeScript, objeto o clase de servicio montado en un Context de Cordis. |
| Bundle | Paquete npm que distribuye una capa de configuración mediante `dsh.bundle`. |
| Profile | Composición ejecutable de Bundles y dependencias locales. |
| Patch | Capa YAML que inserta o reemplaza filas de configuración. |
| Service / Event | Capacidad sustituible y punto de extensión del flujo del agente. |

El propio bucle del agente también se puede reemplazar. El bucle predeterminado prepara prompts y esquemas de herramientas, transmite la respuesta del modelo, ejecuta herramientas y registra eventos persistentes de sesión.

## Inicio rápido

```bash
npx @deepseek-ai/dsh web
```

La interfaz web se sirve por defecto en `http://127.0.0.1:3080`. Añade las credenciales en **Settings → Models** y selecciona un espacio de trabajo.

## Contenido de esta guía

- Cordis, ciclo de vida de plugins, inyección de dependencias y efectos reversibles.
- Plugins de herramientas, modelos, sandbox, almacenamiento, subagentes y Web UI.
- Bundles, Profiles, `cordis.patch.yml`, pruebas, publicación y seguridad.
- Agent Skills previstos: `dsh-repository-explorer`, `dsh-plugin-scaffold`, `dsh-tool-builder` y `dsh-plugin-review`.

Aquí, un **Skill** es un flujo de instrucciones reutilizable para agentes de programación; no es lo mismo que un **Plugin** en tiempo de ejecución. Estos Skills aún no están publicados.

## Recursos oficiales

- [Código fuente](https://github.com/deepseek-ai/deepseek-harness)
- [Arquitectura](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)
- [Primer plugin](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.md)
- [Empaquetado e instalación](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.md)

## Licencia

[MIT](LICENSE)
