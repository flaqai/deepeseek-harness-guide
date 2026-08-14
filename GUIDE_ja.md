# DeepSeek Harness 技術ガイド

[English](GUIDE.md) | [简体中文](GUIDE_zh.md) | [繁體中文](GUIDE_tw.md) | [日本語](GUIDE_ja.md) | [한국어](GUIDE_ko.md) | [Deutsch](GUIDE_de.md) | [Español](GUIDE_es.md) | [Français](GUIDE_fr.md) | [Italiano](GUIDE_it.md) | [Português](GUIDE_pt.md) | [Русский](GUIDE_ru.md) | [العربية](GUIDE_ar.md) | [Bahasa Indonesia](GUIDE_id.md) | [ไทย](GUIDE_th.md) | [Tiếng Việt](GUIDE_vi.md)

本ガイドは[中国語のアーキテクチャ解説](https://mp.weixin.qq.com/s/Kf87hcNdSmY4ODWI4UZ8cg)を参考にし、[公式ソース](https://github.com/deepseek-ai/deepseek-harness)と[公式アーキテクチャ文書](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)で内容を照合しています。

> DeepSeek Harness は Developer Preview 中です。記事は固定 Commit を分析しており、パッケージ名、Preset、内部 API は変更される可能性があります。

## 中心となるモデル

DSH は二つのシステムを同時に維持します。

- **実行時プラグイングラフ**：現在存在する能力、その Scope、Fiber によるライフサイクル所有権を表します。
- **追記専用 Session Event Stream**：Agent が実行した永続的な事実を記録し、モデル履歴、UI、再開、Fork に投影します。

Agent Loop はグラフからモデル、Prompt、ツール、ポリシーを取得し、結果をイベントストリームへ書き戻します。

## 構成パイプライン

`Profile → Bundles → Profile Patch → Home Patch → --patch`

後のレイヤーは ID 単位で設定行全体を置換するか、新しい行を追加します。最初の診断には次を使います。

```bash
dsh --profile web --dump-config
```

## Cordis ランタイム

| 要素 | 役割 |
| --- | --- |
| Context | Service の可視性、継承、分離 Realm を決定。 |
| Service | Definition、Provider、Consumer を安定した契約で接続。 |
| Fiber | 設定、依存、状態、Disposer を持つ一回の Plugin マウント。 |
| Effect | リソース取得とクリーンアップを Fiber に帰属。 |
| Event | 通知、判断、Waterfall Middleware で処理を拡張。 |
| Loader | 設定 Entry を更新・アンロード可能なプラグインツリーへ変換。 |

`inject` は Context 内の依存契約であり、OS の権限制御ではありません。`ctx.effect()` は構造化クリーンアップを提供しますが、外部トランザクションは自動で取り消せません。

## Agent と Session

Turn は 0 個以上の Step を含み、Step は通常、一回のモデル要求と関連するツール実行を含みます。Session Event は境界、メッセージ、Chunk、Tool Call、Result を記録します。`deriveMessages()` がログからモデル向けの履歴を投影します。

完全な記録は、毎回すべてを再送することとは異なります。Compaction は元イベントを残したまま古い Surface を隠せます。また、再生可能なログでも外部副作用が安全に再実行できるとは限りません。

## キャッシュと安全性

動的なグラフ自体は Prefix Cache を壊しません。ツール、Prompt、モデル、履歴などモデル可視 Surface が変わると無効化されます。順序を決定的に保ち、変動データを分離してください。

第三者 Plugin は高権限の同一プロセスコードです。インストールスクリプト、Node API、ネットワーク、認証情報、ファイル、サブプロセス、テレメトリー、終了処理を確認し、Commit を固定してください。

## 開発チェック

- Loop を変える前に既存 Service / Event Seam を探す。
- `inject` で依存を宣言し、Schema で設定を検証する。
- Listener、Timer、Service、Handle に所有者と Cleanup を持たせる。
- 状態を Host、Agent Scope、Session Log のどこに置くか決める。
- Provider 交換、更新、Unload、Resume、Fork、Compaction をテストする。
- Bundle 化し、`--dump-config` で最終ツリーを確認する。

詳細は[英語版](GUIDE.md)または[簡体中国語版](GUIDE_zh.md)を参照してください。

