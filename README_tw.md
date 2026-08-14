# DeepSeek Harness 指南

[English](README.md) | [简体中文](README_zh.md) | [繁體中文](README_tw.md) | [日本語](README_ja.md) | [한국어](README_ko.md) | [Deutsch](README_de.md) | [Español](README_es.md) | [Français](README_fr.md) | [Italiano](README_it.md) | [Português](README_pt.md) | [Русский](README_ru.md) | [العربية](README_ar.md) | [Bahasa Indonesia](README_id.md) | [ไทย](README_th.md) | [Tiếng Việt](README_vi.md)

📘 [閱讀技術架構指南 →](GUIDE_tw.md)

> 一份協助你理解、擴充並為 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) 開發外掛的社群多語言指南。

DeepSeek Harness（`dsh`）是 DeepSeek AI 開源的 Agent Harness。它的核心理念是：**一切皆外掛**。模型介面卡、工具、Agent Loop、工作階段儲存、權限、沙箱、遙測與使用者介面，都能透過設定進行組合或替換。

> [!IMPORTANT]
> 本專案是獨立社群指南，並非 DeepSeek 官方倉庫。DeepSeek Harness 目前處於開發者預覽階段，可能出現不相容變更；請以[官方倉庫](https://github.com/deepseek-ai/deepseek-harness)與[官方文件](https://deepseek-harness.github.io/deepseek-harness/)為準。

## 為什麼重要

模型本身不會讀取程式庫、執行命令、呼叫工具、要求授權或保存工作階段。Harness 提供這個執行環境，並協調使用者、模型、工具與應用程式狀態。

DeepSeek Harness 由 [Cordis](https://github.com/cordiverse/cordis) 驅動。外掛向共享 Context 提供 Service、型別化 Event 與可逆 Effect，因此團隊可以替換模型、工具、沙箱、儲存或子 Agent，而不必分叉整個應用程式。

## 核心概念

| 概念 | 說明 |
| --- | --- |
| Plugin | 掛載至 Cordis Context 的 TypeScript 模組、物件或服務類別。 |
| Bundle | 透過 `dsh.bundle` 發布設定層的 npm 套件。 |
| Profile | 列出 Bundle 與本機外掛依賴的可執行組合。 |
| Patch | 插入或取代設定列的 YAML 覆寫層。 |
| Service / Event | 可替換能力，以及觀察或攔截 Agent 流程的擴充點。 |

Agent Loop 本身也是可替換的外掛。預設流程會組裝提示與工具 Schema、串流模型回應、執行工具、記錄工作階段事件，並持續到沒有待處理工作。

## 快速開始

```bash
npx @deepseek-ai/dsh web
```

Web UI 預設位於 `http://127.0.0.1:3080`。在 **Settings → Models** 新增模型憑證，選擇工作區後即可建立工作階段。

## 本指南的內容

- Cordis、外掛生命週期、依賴注入與可逆 Effect。
- 工具、模型、沙箱、儲存、子 Agent 與 Web UI 外掛。
- Bundle、Profile、`cordis.patch.yml`、測試、發布與安全性。
- 規劃中的 Agent Skills：`dsh-repository-explorer`、`dsh-plugin-scaffold`、`dsh-tool-builder` 與 `dsh-plugin-review`。

這裡的 **Skill** 是給 AI 編碼助手使用的可重複工作流程，與 DeepSeek Harness 執行階段的 **Plugin** 不同。上述 Skills 尚未發布。

## 官方資源

- [原始碼](https://github.com/deepseek-ai/deepseek-harness)
- [架構文件](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.zh.md)
- [第一個外掛教學](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.zh.md)
- [外掛打包與安裝](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.zh.md)

## 授權

[MIT](LICENSE)
