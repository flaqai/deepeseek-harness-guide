# Guía de DeepSeek Harness

[English](README.md) | [简体中文](README_zh.md) | [繁體中文](README_tw.md) | [日本語](README_ja.md) | [한국어](README_ko.md) | [Deutsch](README_de.md) | [Español](README_es.md) | [Français](README_fr.md) | [Italiano](README_it.md) | [Português](README_pt.md) | [Русский](README_ru.md) | [العربية](README_ar.md) | [Bahasa Indonesia](README_id.md) | [ไทย](README_th.md) | [Tiếng Việt](README_vi.md)

![DeepSeek Harness Guide — Del primer inicio al desarrollo de Agents](assets/deepseek-harness-guide-hero.png)

> Guía multilingüe para desarrolladores que quieren comprender, ejecutar y ampliar [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness), y crear sus propios agentes sobre él.

DeepSeek Harness (`dsh`) es un **runtime y framework de composición de agentes** de código abierto desarrollado por DeepSeek AI. Une modelos, prompts, herramientas, permisos, sandbox, sesiones, subagentes, telemetría e interfaces, y permite sustituir cada módulo mediante una arquitectura común de plugins.

> [!IMPORTANT]
> DSH está en vista previa para desarrolladores y puede introducir cambios incompatibles. Fija el commit utilizado y consulta el [repositorio oficial](https://github.com/deepseek-ai/deepseek-harness). Esta guía es un proyecto comunitario independiente.

## Por dónde empezar

| Objetivo | Documento |
|---|---|
| Entender la arquitectura | [Guía técnica](GUIDE_es.md) |
| Instalar, usar y diagnosticar | [Manual de uso](USAGE_es.md) |
| Desarrollar un agente sobre DSH | [Ruta de desarrollo](#desarrollar-un-agente-con-dsh) |
| Trabajar con un agente de programación | [Skills prácticos](skills/) |

## Qué es DeepSeek Harness

Un modelo por sí solo no administra un workspace, no ejecuta herramientas de forma segura, no conserva sesiones, no solicita aprobación y no ofrece una UI. Un Agent Harness proporciona esa capa operativa. DSH es a la vez una aplicación Web Agent lista para usar y un framework para ensamblar agentes de programación, investigación, operaciones o dominios específicos.

Su principio es **Everything is a Plugin**. Proveedores de modelos, herramientas, Agent Loop, Session, políticas, sandbox, almacenamiento e interfaz utilizan el mismo modelo de composición Cordis.

## Arquitectura

```mermaid
flowchart LR
    C["Profile + Bundle + Patch"] --> G["Cordis plugin graph"]
    G --> A["Agent Loop"]
    A --> M["Model"]
    A --> T["Tools + policy + sandbox"]
    A --> S["Session events"]
    S --> A
    S --> U["Host API + Client UI"]
```

- Context, Service, Fiber, Effect, Event y Loader controlan visibilidad, dependencias y ciclo de vida.
- Bundle distribuye configuración, Profile compone el runtime y Patch mantiene diferencias del entorno.
- Agent Loop prepara el contexto, llama al modelo y las herramientas, y decide cuándo terminar.
- Los Session Events son la fuente durable y reproducible; la interfaz es una proyección.
- Host contiene las capacidades privilegiadas y Client presenta la interfaz.

## Inicio rápido

```bash
npx @deepseek-ai/dsh web
```

Abre `http://127.0.0.1:3080`, configura el modelo en **Settings → Models** y elige un workspace. Antes de diagnosticar plugins, inspecciona la composición efectiva:

```bash
dsh --profile web --dump-config
```

## Instalar y probar el Plugin OpenPencil

Este es el ejemplo con versiones fijadas de la página de referencia. Detén la Web UI, comprueba `op --version` y usa la misma versión de DSH para instalar, inspeccionar, iniciar y eliminar.

```bash
npx --yes -p @deepseek-ai/dsh@0.1.0-rc.6 dsh plugin --profile web add @zseven-w/dsh-openpencil@latest
npx --yes -p @deepseek-ai/dsh@0.1.0-rc.6 dsh --profile web --dump-config
npx --yes -p @deepseek-ai/dsh@0.1.0-rc.6 dsh web
```

En una Session nueva, pide crear e inspeccionar un documento `.op`. Antes de producción, revisa publicador, permisos y compatibilidad, y sustituye `@latest` por una versión exacta probada. Para quitarlo, detén la UI y ejecuta `dsh plugin --profile web remove @zseven-w/dsh-openpencil`. Consulta la guía completa de desarrollo, diagnóstico y seguridad en [inglés](README.md#install-and-use-a-dsh-plugin-openpencil-example) o [chino](README_zh.md#安装与使用-dsh-pluginopenpencil-示例).

## Desarrollar un agente con DSH

1. Define tarea, efectos permitidos, condición de finalización, presupuesto, cancelación y aprobaciones.
2. Elige un Profile, añade capacidades mediante Bundles y conserva diferencias en Patches.
3. Diseña modelo, Prompt, memoria, compactación y visibilidad de herramientas.
4. Divide Tools, Services, Providers, políticas y workflows en plugins pequeños.
5. Reutiliza el Agent Loop existente; sustitúyelo solo si cambia la planificación o finalización.
6. Guarda como Session Events los resultados que el modelo o la UI deberán reconstruir.
7. Coloca el runtime en Host, la presentación Web en Client y conéctalos mediante una API tipada.
8. Prueba montaje, denegación, timeout, descarga, reinicio y rollback en un Profile desechable.

Una Tool es una capacidad del runtime invocada por el modelo. Un Agent Skill guía al agente de programación y no es un plugin del runtime DSH.

## Documentación del proyecto

- [Guía técnica](GUIDE_es.md): Cordis, ciclo de vida, Session, caché y seguridad.
- [Manual de uso](USAGE_es.md): instalación, módulos, desarrollo de plugins, diagnóstico y publicación.
- [Skills prácticos](skills/): exploración, scaffolding, herramientas y revisión de seguridad.
- Versiones completas: [English](README.md) y [简体中文](README_zh.md).

## Seguridad y compatibilidad

Fija commits de DSH y plugins. Revisa scripts de instalación, archivos, red, subprocesos y retención. Inyección de dependencias, política, aprobación y sandbox del sistema son límites distintos. No incluyas credenciales reales, sesiones privadas, capturas, códigos QR ni contactos en la documentación.

## APIs de flaq.ai y programa de afiliados

[flaq.ai](https://flaq.ai/) es una plataforma externa de agregación de modelos y APIs de IA. Para un Agent basado en DSH se pueden evaluar [DeepSeek V4 Pro Text-to-Text](https://flaq.ai/models/deepseek/deepseek-v4-pro-text-to-text/) para razonamiento, escritura, código y análisis, y [DeepSeek V4 Flash Text-to-Text](https://flaq.ai/models/deepseek/deepseek-v4-flash-text-to-text/) para generación, resúmenes y automatización rápidos y atentos al coste. Antes de integrar, verifica identificador, streaming, llamadas de herramientas, precios, tratamiento de datos y errores actuales. No supone garantía de disponibilidad o compatibilidad.

Los desarrolladores y creadores también pueden solicitar acceso al [programa de afiliados de flaq.ai](https://flaq.ai/affiliate-agreement/). Deben cumplir el acuerdo vigente, la legislación y las obligaciones de divulgación; no se garantizan tráfico, comisiones, pagos ni ingresos.

[MIT License](LICENSE)
