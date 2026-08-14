# DeepSeek Harness 技術指南

[English](GUIDE.md) | [简体中文](GUIDE_zh.md) | [繁體中文](GUIDE_tw.md) | [日本語](GUIDE_ja.md) | [한국어](GUIDE_ko.md) | [Deutsch](GUIDE_de.md) | [Español](GUIDE_es.md) | [Français](GUIDE_fr.md) | [Italiano](GUIDE_it.md) | [Português](GUIDE_pt.md) | [Русский](GUIDE_ru.md) | [العربية](GUIDE_ar.md) | [Bahasa Indonesia](GUIDE_id.md) | [ไทย](GUIDE_th.md) | [Tiếng Việt](GUIDE_vi.md)

本指南參考[《DSH：DeepSeek Harness 架構解析》](https://mp.weixin.qq.com/s/Kf87hcNdSmY4ODWI4UZ8cg)，並以[官方原始碼](https://github.com/deepseek-ai/deepseek-harness)和[架構文件](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.zh.md)交叉核對。

> DeepSeek Harness 仍處於 Developer Preview。文章分析固定的 Commit；套件名稱、Preset 與內部 API 可能改變。

## 核心心智模型

DSH 同時維護兩套系統：

- **執行階段外掛圖**：描述目前有哪些能力、在哪個 Scope 可見，以及由哪個 Fiber 管理生命週期。
- **追加式 Session Event Stream**：記錄 Agent 已發生的持久事實，並投影成模型歷史、UI、恢復與 Fork。

Agent Loop 從外掛圖取得模型、Prompt、工具和策略，完成工作後把結果寫回事件流。

## 組合管線

`Profile → Bundles → Profile Patch → Home Patch → --patch`

後面的層會按 ID 取代完整設定列或插入新列。排障時先執行：

```bash
dsh --profile web --dump-config
```

## Cordis 執行階段

| 元件 | 職責 |
| --- | --- |
| Context | 決定 Service 可見範圍、繼承與隔離 Realm。 |
| Service | 用穩定介面連接 Definition、Provider 與 Consumer。 |
| Fiber | 一次真實的 Plugin 掛載，保存設定、依賴、狀態與清理函式。 |
| Effect | 將資源取得與 Disposer 歸屬於 Fiber。 |
| Event | 以通知、決策或 Waterfall Middleware 擴充流程。 |
| Loader | 將設定 Entry 調和成可更新與卸載的外掛樹。 |

`inject` 是 Context 依賴契約，不是作業系統權限。`ctx.effect()` 提供結構化清理，但不能自動回滾外部交易。

## Agent 與 Session

Turn 可包含零到多個 Step；Step 通常包含一次模型請求和相關工具執行。Session Event 記錄 Turn/Step 邊界、訊息、Chunk、Tool Call 與 Result。`deriveMessages()` 從日誌投影模型目前看到的內容。

「完整記錄」不等於「每次完整重送」。Compaction 可遮蔽舊 Surface 而保留原始事件；可回放日誌也不表示外部副作用可安全重做。

## 快取與安全

動態外掛圖不必然破壞前綴快取；只有工具、Prompt、模型或歷史等模型可見 Surface 改變時才會失效。保持順序穩定並隔離易變資料。

第三方外掛是高權限同進程程式碼。請審查安裝腳本、Node API、網路、憑證、檔案系統、子程序、遙測和清理；固定 Commit 後再授予 Build 權限。

## 開發檢查

- 優先使用既有 Service 或 Event Seam，不直接修改 Loop。
- 用 `inject` 宣告依賴，用 Schema 驗證設定。
- 讓 Listener、Timer、Service 與 Handle 都有生命週期歸屬。
- 明確狀態屬於 Host、Agent Scope 或 Session Log。
- 測試 Provider 替換、設定更新、Unload、Resume、Fork 與 Compaction。
- 打包為 Bundle，並用 `--dump-config` 驗證結果。

完整細節可閱讀[英文版](GUIDE.md)或[簡體中文版](GUIDE_zh.md)。

