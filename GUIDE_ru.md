# Техническое руководство по DeepSeek Harness

[English](GUIDE.md) | [简体中文](GUIDE_zh.md) | [繁體中文](GUIDE_tw.md) | [日本語](GUIDE_ja.md) | [한국어](GUIDE_ko.md) | [Deutsch](GUIDE_de.md) | [Español](GUIDE_es.md) | [Français](GUIDE_fr.md) | [Italiano](GUIDE_it.md) | [Português](GUIDE_pt.md) | [Русский](GUIDE_ru.md) | [العربية](GUIDE_ar.md) | [Bahasa Indonesia](GUIDE_id.md) | [ไทย](GUIDE_th.md) | [Tiếng Việt](GUIDE_vi.md)

Руководство основано на [китайском техническом разборе](https://mp.weixin.qq.com/s/Kf87hcNdSmY4ODWI4UZ8cg) и сверено с [официальным кодом](https://github.com/deepseek-ai/deepseek-harness) и [документацией архитектуры](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md).

> DeepSeek Harness находится в Developer Preview. Статья исследует фиксированные Commits; пакеты, Presets и внутренние API могут измениться.

## Центральная модель

DSH поддерживает две связанные системы:

- **Граф runtime-плагинов:** доступные способности, Scope их видимости и жизненный цикл, принадлежащий Fibers.
- **Append-only поток событий Session:** постоянные факты работы Agent, проецируемые в историю модели, UI, Resume и Fork.

Agent Loop получает модели, Prompts, инструменты и политики из графа и записывает результаты в поток событий.

## Композиция

`Profile → Bundles → Profile Patch → Home Patch → --patch`

Поздние слои заменяют строку конфигурации целиком по ID или вставляют новую. Первая диагностика:

```bash
dsh --profile web --dump-config
```

## Runtime Cordis

| Элемент | Роль |
| --- | --- |
| Context | Видимость, наследование и изолированные Realms сервисов. |
| Service | Стабильный контракт между Definition, Provider и Consumer. |
| Fiber | Реальный экземпляр Plugin с конфигурацией, зависимостями и Disposers. |
| Effect | Закрепляет ресурсы и Cleanup за Fiber. |
| Event | Расширяет поток уведомлением, решением или Waterfall Middleware. |
| Loader | Превращает конфигурацию в обновляемое и выгружаемое дерево. |

`inject` — контракт зависимостей Context, а не разрешение ОС. `ctx.effect()` организует Cleanup, но не откатывает внешние транзакции.

## Agent и Session

Turn содержит ноль или больше Steps; Step обычно включает запрос модели и вызовы инструментов. Session Events записывают границы, сообщения, Chunks, Tool Calls и результаты. `deriveMessages()` проецирует видимую модели историю.

Полная запись не означает полную повторную отправку. Compaction может скрыть старую поверхность, сохранив события. Воспроизводимый журнал не делает внешние эффекты безопасными для повтора.

## Кэш и безопасность

Динамический граф сам по себе не сбрасывает Prefix Cache. Это происходит при изменении видимых инструментов, Prompt, модели или истории. Сохраняйте стабильный порядок и отделяйте изменчивые данные.

Сторонние Plugins — привилегированный код в процессе хоста. Проверяйте установочные скрипты, Node API, сеть, секреты, файлы, подпроцессы, телеметрию и Cleanup; фиксируйте Commit.

## Проверка разработки

- Использовать Service или Event Seam до изменения Loop.
- Объявлять зависимости через `inject`, проверять конфигурацию Schema.
- Назначать владельца и Cleanup для listeners, timers, Services и handles.
- Решить, относится ли состояние к Host, Agent Scope или Session Log.
- Тестировать замену Provider, update, Unload, Resume, Fork и Compaction.
- Упаковать как Bundle и проверить через `--dump-config`.

Подробнее см. [английскую](GUIDE.md) или [китайскую версию](GUIDE_zh.md).

