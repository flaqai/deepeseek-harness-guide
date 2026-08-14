# DeepSeek Harness ガイド

[English](README.md) | [简体中文](README_zh.md) | [繁體中文](README_tw.md) | [日本語](README_ja.md) | [한국어](README_ko.md) | [Deutsch](README_de.md) | [Español](README_es.md) | [Français](README_fr.md) | [Italiano](README_it.md) | [Português](README_pt.md) | [Русский](README_ru.md) | [العربية](README_ar.md) | [Bahasa Indonesia](README_id.md) | [ไทย](README_th.md) | [Tiếng Việt](README_vi.md)

📘 [技術アーキテクチャガイドを読む →](GUIDE_ja.md)

> [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) の理解、拡張、プラグイン開発を支援するコミュニティ運営の多言語ガイドです。

DeepSeek Harness（`dsh`）は、DeepSeek AI が公開したオープンソースのエージェントハーネスです。中心となる考え方は **「すべてがプラグイン」**。モデルアダプター、ツール、エージェントループ、セッション保存、権限、サンドボックス、テレメトリー、UI を設定によって構成・交換できます。

> [!IMPORTANT]
> 本リポジトリは独立したコミュニティガイドであり、DeepSeek の公式リポジトリではありません。DeepSeek Harness は開発者プレビュー中で、破壊的変更が入る可能性があります。実装の詳細は[公式リポジトリ](https://github.com/deepseek-ai/deepseek-harness)と[公式ドキュメント](https://deepseek-harness.github.io/deepseek-harness/)で確認してください。

## Harness が担うもの

モデル単体では、リポジトリの読み取り、コマンド実行、ツール呼び出し、承認要求、セッション保存、障害復旧は行えません。Harness はこの実行環境を提供し、ユーザー、モデル、ツール、アプリケーション状態を結ぶループを制御します。

DeepSeek Harness は [Cordis](https://github.com/cordiverse/cordis) を基盤にしています。プラグインは共有 Context に Service、型付き Event、可逆 Effect を追加します。これにより、アプリ全体をフォークせずにモデル、ツール、サンドボックス、ストレージ、サブエージェントを差し替えられます。

## 主要な概念

| 概念 | 説明 |
| --- | --- |
| Plugin | Cordis Context にマウントされる TypeScript モジュール、オブジェクト、サービスクラス。 |
| Bundle | `dsh.bundle` を通して設定レイヤーを配布する npm パッケージ。 |
| Profile | Bundle とローカル依存をまとめた実行可能な構成。 |
| Patch | 設定行を追加・置換する YAML オーバーレイ。 |
| Service / Event | 交換可能な能力と、エージェント処理を観測・介入する拡張点。 |

エージェントループ自体も交換可能です。既定のループは、プロンプトとツールスキーマを組み立て、モデル応答をストリーミングし、ツールを実行し、セッションイベントを記録します。

## クイックスタート

```bash
npx @deepseek-ai/dsh web
```

Web UI は既定で `http://127.0.0.1:3080` に起動します。**Settings → Models** でモデル認証情報を追加し、ワークスペースを選択してください。

## このガイドで扱う内容

- Cordis、プラグインのライフサイクル、依存性注入、可逆 Effect。
- ツール、モデル、サンドボックス、ストレージ、サブエージェント、Web UI の拡張。
- Bundle、Profile、`cordis.patch.yml`、テスト、公開、安全性。
- 予定している Agent Skills：`dsh-repository-explorer`、`dsh-plugin-scaffold`、`dsh-tool-builder`、`dsh-plugin-review`。

ここでの **Skill** は AI コーディングエージェント向けの再利用可能な手順で、DeepSeek Harness の実行時 **Plugin** とは別物です。上記 Skills はまだ公開されていません。

## 公式リソース

- [ソースコード](https://github.com/deepseek-ai/deepseek-harness)
- [アーキテクチャ](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)
- [最初のプラグインチュートリアル](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.md)
- [パッケージ化とインストール](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.md)

## ライセンス

[MIT](LICENSE)
