# CCA-F 学習サイト — 進捗状況

> 最終更新: 2026-03-19

## 現在のステータス

**フェーズ**: 初期構築完了、コンテンツ作成フェーズへ移行

## 完了済み

- [x] CCA-F試験の調査（試験構成、5ドメイン、6シナリオ、公式リソース一覧）
- [x] 8週間学習カリキュラムの設計
- [x] 学習サイトのフレームワーク構築（進捗管理、クイズ機能、ダッシュボード）
- [x] Module 0（導入編）の完全なコンテンツ作成
  - Claudeとは何か、モデル一覧
  - Anthropic製品群の整理
  - 試験構成と5ドメインの解説
  - 6つの試験シナリオの概要
  - 基本用語集（10語）
  - 確認クイズ（5問）
- [x] PROJECT.md（方針書）作成
- [x] STATUS.md（このファイル）作成
- [x] GitHub Pages セットアップ（rikunagayasu-oss/cca-f-learning）
- [x] Module 1「Claude製品群を理解する」のコンテンツ作成
  - 製品ラインナップ（一般向け・開発者向け・企業向け）
  - APIとSDK（Messages API、Batches API、公式SDK 7言語）
  - Claude Code（CLI・CLAUDE.md・フック・MCP・Agent SDK）
  - 製品の使い分けガイド（ユーザー別推奨・モデル選択・コスト最適化）
  - 確認クイズ（5問）
- [x] Module 2「プロンプトの基本」のコンテンツ作成
  - 基本原則（明確性・コンテキスト・肯定形指示）
  - 構造化テクニック（XMLタグ・Few-shot・長文配置）
  - 高度なテクニック（CoT・Extended Thinking・チェーニング・構造化出力）
  - 用語集（11語）・確認クイズ（5問）
- [x] Module 3「システムプロンプト設計」のコンテンツ作成
  - システムプロンプトの役割と設計パターン
  - XMLタグ構造化・Few-shot・Extended Thinking
  - 運用と安全性（キャッシング・インジェクション対策・Multi-pass Review）
  - 用語集（9語）・確認クイズ（5問）
- [x] Module 4「構造化出力とJSON」のコンテンツ作成
  - JSON Outputs vs Strict Tool Useの使い分け
  - JSON Schemaスキーマ設計（サポート/非サポート制約）
  - 実践パターン（データ抽出・分類・エンティティ抽出）
  - 用語集（9語）・確認クイズ（5問）

- [x] Module 5「ツール設計の基礎」のコンテンツ作成
  - Tool Use（Function Calling）の4ステップフロー
  - ツール定義（name, description, input_schema）の設計ベストプラクティス
  - Tool Choice（auto/any/tool/none）とStrict Mode
  - エラーハンドリング（is_error）とアンチパターン
  - 用語集（8語）・確認クイズ（5問）
- [x] Module 6「MCP統合」のコンテンツ作成
  - MCPの概念とM×N問題の解消
  - Host/Client/Serverアーキテクチャ（1:1関係）
  - 3つのプリミティブ（Resources/Tools/Prompts）と制御主体
  - MCP vs Tool Useの補完関係
  - トランスポート（stdio/Streamable HTTP）
  - 用語集（11語）・確認クイズ（5問）
- [x] Module 7「Claude Code入門」のコンテンツ作成
  - エージェント型ツールの位置づけ
  - 内蔵ツールと権限（読み取り専用=不要、変更系=必要）
  - 5つの権限モードと設定ファイル優先順位
  - CLIコマンド・フラグ（-p, --max-turns等）
  - サブエージェント（Explore/Plan/General-purpose）とHooks
  - 用語集（8語）・確認クイズ（5問）
- [x] Module 8「CLAUDE.mdとワークフロー」のコンテンツ作成
  - CLAUDE.mdの3層階層構造と読み込み優先順位
  - settings.json vs CLAUDE.mdの使い分け
  - Hooks/カスタムコマンド/スキルによるワークフロー設計
  - チーム開発・CI/CDでの運用パターン
  - 用語集（8語）・確認クイズ（5問）

## 次にやること

- [ ] Module 9「エージェントアーキテクチャ」のコンテンツ作成
- [ ] Module 10「マルチエージェント」のコンテンツ作成
- [ ] 以降、M11〜M12を順次作成

## 作業の進め方

新しいセッションでは、まず以下を読んでもらう：
1. `PROJECT.md` — 全体方針とルールの把握
2. `STATUS.md`（このファイル）— 現在の状況の把握
3. `index.html` の該当部分 — 既存の構造の確認

その上で「Module X のコンテンツを作成して」と依頼する。

## 備考

- 公式リソース（Anthropic Academy、GitHubコース）の内容を参考にしつつ、非エンジニア向けに噛み砕いて日本語化する方針
- 社内横展開も想定しているため、分かりやすさを最優先
