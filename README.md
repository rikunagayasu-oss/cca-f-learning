# CCA-F 学習サイト

**[サイトを開く（GitHub Pages）](https://rikunagayasu-oss.github.io/cca-f-learning/)**

## 概要

Anthropic公式のCCA-F（Claude Certified Architect — Foundations）試験対策として構築した、**日本語の学習ポータルサイト**です。

非エンジニアの方でも理解しやすいよう、技術用語には平易な日本語の説明を添え、比喩やたとえ話を交えて解説しています。全13モジュール・8週間のカリキュラムで、CCA-F試験の5ドメインを体系的に学べます。

## 主な特徴

- **13モジュール構成** — 基礎から応用まで段階的に学習できるカリキュラム
- **各モジュールにクイズ付き** — 4択問題で理解度をその場で確認。日本語解説付き
- **進捗管理機能** — 学習ダッシュボードで各モジュールの進捗を一覧表示
- **完全オフライン対応** — 外部依存なし。HTMLファイル1つで完結
- **日本語ネイティブ向け** — 英語用語には日本語の説明を併記（試験は英語のため、英語名も習得可能）

## CCA-F 試験の5ドメイン

| ドメイン | 配点 | 内容 | 対応Week |
|---------|------|------|---------|
| 1. エージェント型アーキテクチャとオーケストレーション | 27% | AIエージェントの設計パターン、複数エージェントの連携 | Week 7 |
| 2. Claude Codeの設定とワークフロー | 20% | 設定ファイル、コマンド、CI/CD連携 | Week 6 |
| 3. プロンプトエンジニアリングと構造化出力 | 20% | 効果的なプロンプト設計、JSON出力の制御 | Week 3-4 |
| 4. ツール設計とMCP統合 | 18% | ツールの設計方法、MCPサーバーの構築 | Week 5 |
| 5. コンテキスト管理と信頼性 | 15% | 長文処理の戦略、エラー処理、人間によるレビュー | Week 8 |

## モジュール一覧

| Module | タイトル | 対応Week |
|--------|---------|---------|
| M0 | 導入 — 全体像と資格概要 | Week 1 |
| M1 | Claude製品群を理解する | Week 1 |
| M2 | プロンプトの基本 | Week 2 |
| M3 | システムプロンプト設計 | Week 3 |
| M4 | 構造化出力とJSON | Week 4 |
| M5 | ツール設計の基礎 | Week 5 |
| M6 | MCP統合 | Week 5 |
| M7 | Claude Code入門 | Week 6 |
| M8 | CLAUDE.mdとワークフロー | Week 6 |
| M9 | エージェントアーキテクチャ | Week 7 |
| M10 | マルチエージェント | Week 7 |
| M11 | コンテキスト管理 | Week 8 |
| M12 | 総復習と模擬試験 | Week 8 |

## 技術構成

- **HTML + CSS + vanilla JavaScript** — フレームワーク不使用
- **`index.html` 1ファイルで完結** — ダウンロードして開くだけで使えます
- **進捗データはlocalStorageに保存** — ブラウザを閉じても学習状況が保持されます
- **外部依存なし** — インターネット接続がなくても学習可能

## 使い方

### オンラインで使う

[GitHub Pages](https://rikunagayasu-oss.github.io/cca-f-learning/) にアクセスするだけで使えます。

### ローカルで使う

1. このリポジトリをクローンまたはダウンロード
2. `index.html` をブラウザで開く

```bash
git clone https://github.com/rikunagayasu-oss/cca-f-learning.git
open cca-f-learning/index.html
```

## 参考リソース

- [Anthropic Academy](https://anthropic.skilljar.com/) — 公式の無料学習コース（全13コース）
- [Anthropic公式ドキュメント](https://docs.anthropic.com) — API・ツール・エージェント設計のリファレンス
- [GitHub Courses](https://github.com/anthropics/courses) — Jupyter Notebook形式の実践教材

## ライセンス

このプロジェクトは学習目的で作成されています。
