# Guía técnica de DeepSeek Harness

[English](GUIDE.md) | [简体中文](GUIDE_zh.md) | [繁體中文](GUIDE_tw.md) | [日本語](GUIDE_ja.md) | [한국어](GUIDE_ko.md) | [Deutsch](GUIDE_de.md) | [Español](GUIDE_es.md) | [Français](GUIDE_fr.md) | [Italiano](GUIDE_it.md) | [Português](GUIDE_pt.md) | [Русский](GUIDE_ru.md) | [العربية](GUIDE_ar.md) | [Bahasa Indonesia](GUIDE_id.md) | [ไทย](GUIDE_th.md) | [Tiếng Việt](GUIDE_vi.md)

Esta guía parte de un [análisis técnico en chino](https://mp.weixin.qq.com/s/Kf87hcNdSmY4ODWI4UZ8cg) y contrasta sus ideas con el [código oficial](https://github.com/deepseek-ai/deepseek-harness) y la [documentación de arquitectura](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md).

> DeepSeek Harness está en Developer Preview. El artículo analiza Commits fijos; los paquetes, Presets y APIs internas pueden cambiar.

## Modelo central

DSH mantiene dos sistemas coordinados:

- **Grafo de plugins en tiempo de ejecución:** capacidades actuales, Scope de visibilidad y propiedad del ciclo de vida mediante Fibers.
- **Flujo append-only de eventos de sesión:** hechos duraderos del Agent, proyectados al historial del modelo, UI, Resume y Fork.

El Agent Loop obtiene modelos, Prompts, herramientas y políticas del grafo, y escribe los resultados en el flujo de eventos.

## Composición

`Profile → Bundles → Profile Patch → Home Patch → --patch`

Las capas posteriores reemplazan una fila completa por ID o insertan otra. La primera herramienta de diagnóstico es:

```bash
dsh --profile web --dump-config
```

## Runtime de Cordis

| Elemento | Responsabilidad |
| --- | --- |
| Context | Visibilidad, herencia y Realms aislados de Services. |
| Service | Contrato estable entre Definition, Provider y Consumer. |
| Fiber | Instancia real del Plugin con configuración, dependencias y Disposers. |
| Effect | Asocia recursos y limpieza con un Fiber. |
| Event | Extiende el flujo con notificaciones, decisiones o Waterfall Middleware. |
| Loader | Convierte configuración en un árbol actualizable y descargable. |

`inject` es un contrato de dependencias de Context, no un permiso del sistema operativo. `ctx.effect()` organiza la limpieza, pero no revierte transacciones externas.

## Agent y Session

Un Turn contiene cero o más Steps; un Step suele incluir una petición al modelo y sus herramientas. Los Session Events registran límites, mensajes, Chunks, Tool Calls y resultados. `deriveMessages()` proyecta el historial visible para el modelo.

Registro completo no significa reenvío completo. Compaction puede ocultar contenido antiguo conservando los eventos originales. Un log reproducible tampoco hace seguros los efectos externos repetidos.

## Caché y seguridad

Un grafo dinámico no invalida por sí mismo la caché de prefijo. La invalidación ocurre cuando cambian herramientas, Prompt, modelo o historial visibles. Mantén orden estable y separa datos volátiles.

Los plugins de terceros son código privilegiado en el proceso host. Revisa scripts de instalación, APIs de Node, red, credenciales, archivos, subprocesos, telemetría y limpieza; fija un Commit.

## Lista de desarrollo

- Usar un Service o Event Seam antes de modificar el Loop.
- Declarar dependencias con `inject` y validar configuración con Schema.
- Dar propiedad y Cleanup a listeners, timers, Services y handles.
- Elegir si el estado pertenece al Host, Agent Scope o Session Log.
- Probar cambio de Provider, actualización, Unload, Resume, Fork y Compaction.
- Empaquetar como Bundle y validar con `--dump-config`.

Consulta la versión [inglesa](GUIDE.md) o [china](GUIDE_zh.md) para más detalles.

