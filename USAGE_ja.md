# DeepSeek Harness 利用ガイド

[English](USAGE.md) · [簡体中文・完全版](USAGE_zh.md)

このページは日本語のクイックガイドです。DeepSeek Harness は開発者プレビュー中のため、利用するコミットと公式ドキュメントを必ず確認してください。

## クイックスタート

```bash
npx @deepseek-ai/dsh web
dsh --profile web --dump-config
```

`http://127.0.0.1:3080` を開き、モデルサービスを設定し、使い捨てのワークスペースで安全なタスクを試します。2 番目のコマンドは Profile、Bundle、Patch から解決されたプラグインツリーを表示します。

## モジュール分類

- Runtime 構成：Context、Service、Fiber、Effect、Event、Loader。
- Agent 実行：モデルアダプター、Prompt、Agent Loop、ツール、ポリシー、承認、サンドボックス。
- 状態：Session Event、メモリ、圧縮、リプレイ。
- UI：Host、Remote API、Web Client、デスクトップ、TUI、モバイル。
- エコシステム：ワークフロー、ブラウザー、ビジョン、外部連携、テーマ、開発ツール。

## 安全なインストール

```bash
dsh plugin --profile demo add <package-or-git-spec>
dsh --profile demo --dump-config
```

Git コミットを固定し、ライセンス、インストールスクリプト、ネットワーク、ファイル、子プロセス、認証情報、データ保持を確認します。使い捨て Profile で起動、拒否、タイムアウト、アンロード、再起動、ロールバックをテストしてください。

## 実用 Skills

[`skills/`](skills/) には、リポジトリ探索、プラグイン雛形、ツール開発、プラグイン監査の 4 つがあります。Skill は開発 Agent 向けの手順であり、DSH Runtime プラグインではありません。

完全な操作、トラブルシューティング、リリースチェックリストは [English handbook](USAGE.md) を参照してください。
