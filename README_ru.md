# Руководство по DeepSeek Harness

[English](README.md) | [简体中文](README_zh.md) | [繁體中文](README_tw.md) | [日本語](README_ja.md) | [한국어](README_ko.md) | [Deutsch](README_de.md) | [Español](README_es.md) | [Français](README_fr.md) | [Italiano](README_it.md) | [Português](README_pt.md) | [Русский](README_ru.md) | [العربية](README_ar.md) | [Bahasa Indonesia](README_id.md) | [ไทย](README_th.md) | [Tiếng Việt](README_vi.md)

📘 [Открыть техническое руководство →](GUIDE_ru.md)

> Многоязычное руководство сообщества по устройству, расширению и разработке плагинов для [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness).

DeepSeek Harness (`dsh`) — открытая среда выполнения агентов от DeepSeek AI. Ее главная идея: **всё является плагином**. Адаптеры моделей, инструменты, цикл агента, хранение сессий, разрешения, песочница, телеметрия и интерфейс могут компоноваться и заменяться через конфигурацию.

> [!IMPORTANT]
> Это независимое руководство сообщества, а не официальный репозиторий DeepSeek. DeepSeek Harness находится на стадии предварительной версии для разработчиков, поэтому возможны несовместимые изменения. Сверяйтесь с [официальным репозиторием](https://github.com/deepseek-ai/deepseek-harness) и [официальной документацией](https://deepseek-harness.github.io/deepseek-harness/).

## Зачем нужен Harness

Модель сама по себе не читает репозиторий, не запускает команды, не вызывает инструменты, не запрашивает разрешения и не сохраняет сессию. Harness предоставляет эту среду и координирует пользователя, модель, инструменты и состояние приложения.

DeepSeek Harness работает на базе [Cordis](https://github.com/cordiverse/cordis). Плагины добавляют в общий Context сервисы, типизированные события и обратимые эффекты. Это позволяет заменять модели, инструменты, песочницы, хранилища и субагентов без форка всего приложения.

## Основные понятия

| Понятие | Значение |
| --- | --- |
| Plugin | TypeScript-модуль, объект или класс сервиса, подключенный к Cordis Context. |
| Bundle | npm-пакет, поставляющий слой конфигурации через `dsh.bundle`. |
| Profile | Запускаемая композиция Bundles и локальных зависимостей. |
| Patch | YAML-слой, который вставляет или заменяет строки конфигурации. |
| Service / Event | Заменяемая возможность и точка расширения в работе агента. |

Цикл агента также заменяем. Стандартный цикл собирает промпты и схемы инструментов, получает потоковый ответ модели, запускает инструменты и записывает постоянные события сессии.

## Быстрый старт

```bash
npx @deepseek-ai/dsh web
```

По умолчанию Web UI доступен по адресу `http://127.0.0.1:3080`. Добавьте учетные данные в **Settings → Models**, затем выберите рабочую область.

## Что будет в руководстве

- Cordis, жизненный цикл плагинов, внедрение зависимостей и обратимые эффекты.
- Плагины инструментов, моделей, песочниц, хранения, субагентов и Web UI.
- Bundles, Profiles, `cordis.patch.yml`, тестирование, публикация и безопасность.
- Планируемые Agent Skills: `dsh-repository-explorer`, `dsh-plugin-scaffold`, `dsh-tool-builder` и `dsh-plugin-review`.

Здесь **Skill** — повторно используемый рабочий процесс для ИИ-агента разработки, а не runtime-**Plugin** DeepSeek Harness. Перечисленные Skills пока не опубликованы.

## Официальные ресурсы

- [Исходный код](https://github.com/deepseek-ai/deepseek-harness)
- [Архитектура](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)
- [Первый плагин](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.md)
- [Упаковка и установка](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.md)

## Лицензия

[MIT](LICENSE)
