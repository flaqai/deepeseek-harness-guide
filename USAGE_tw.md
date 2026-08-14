# DeepSeek Harness 使用手冊

[完整簡體中文版](USAGE_zh.md) · [English](USAGE.md)

本頁提供繁體中文快速操作路徑。DeepSeek Harness 仍在開發者預覽階段，請以實際使用的 Commit 與官方文件為準。

## 快速開始

```bash
npx @deepseek-ai/dsh web
dsh --profile web --dump-config
```

開啟 `http://127.0.0.1:3080`，設定模型服務，先在一次性工作區執行無破壞性的任務。第二個命令用來檢查 Profile、Bundle 與 Patch 最終組合出的外掛樹。

## 模組分類

- Runtime 組合：Context、Service、Fiber、Effect、Event、Loader。
- Agent 執行：模型介面卡、Prompt、Agent Loop、工具、策略、審批與沙箱。
- 狀態：Session Event、記憶、壓縮與重放。
- 介面：Host、Remote API、Web Client、桌面端、TUI 與行動端。
- 生態擴充：工作流、瀏覽器、視覺、外部整合、主題與開發工具。

## 安裝與審查

```bash
dsh plugin --profile demo add <package-or-git-spec>
dsh --profile demo --dump-config
```

先固定 Git Commit 並審查授權、安裝腳本、網路、檔案、子行程、憑證與資料保留。使用一次性 Profile 測試啟動、拒絕、逾時、卸載、重啟和回滾。生態清單收錄不等於安全背書。

## 實用 Skills

[`skills/`](skills/) 提供原始碼探索、外掛腳手架、工具開發與外掛審查四個 Agent Skill。Skill 指導開發流程，不是透過 `dsh plugin` 安裝的 Runtime 外掛。

完整步驟、排障順序與發布檢查表請閱讀[簡體中文手冊](USAGE_zh.md)。
