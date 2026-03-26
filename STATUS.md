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

- [x] Module 9「エージェントアーキテクチャ」のコンテンツ作成
  - 拡張LLM（Augmented LLM）の3要素（検索・ツール・メモリ）
  - 5つのワークフローパターン（Prompt Chaining/Routing/Parallelization/Orchestrator-Workers/Evaluator-Optimizer）
  - エージェントループとstop_reasonによるループ制御
  - Coordinator-Subagentパターンとセッション管理
  - プログラム的エンフォースメント vs プロンプトベースのガイダンス
  - ACI設計・ポカヨケ設計
  - 用語集（11語）・確認クイズ（5問）
- [x] Module 10「マルチエージェント」のコンテンツ作成
  - マルチエージェントシステムの判断フロー（シンプルさ優先）
  - 5つのワークフローパターン復習と比較表
  - エージェント間通信3方式（メッセージパッシング/共有メモリ/ハンドオフ）
  - Agent-as-Tool vs Handoff（制御権の違い）
  - タスク分解（機能分割 vs データ分割）
  - Guardrails/HITL/エスカレーションによる安全性設計
  - 用語集（12語）・確認クイズ（5問）
- [x] Module 11「コンテキスト管理と信頼性」のコンテンツ作成
  - コンテキストウィンドウ・トークン・チャンキング戦略
  - RAG（Contextual Retrieval含む）・プロンプトキャッシング
  - ハルシネーション対策・Citations・検証パターン
  - エラー処理（指数バックオフ・べき等性）・グレースフルデグラデーション
  - 用語集（10語）・確認クイズ（5問）
- [x] Module 12「総復習と模擬試験」のコンテンツ作成
  - 5ドメイン要点総まとめ
  - 試験対策ガイド（60問/120分/720点合格）
  - シナリオ問題の読み方テクニック
  - 模擬試験（15問・全5ドメイン横断）

- [x] Module 5「ツール設計の基礎」拡充（5セクション追加+クイズ5問追加）
  - ツール説明文（description）のLLM最適化: 5要素チェックリスト、良い/悪い比較
  - 構造化エラーハンドリング（isError/isRetryable）: エラータイプ分類、リトライ可否判定
  - ツール分配（Tool Distribution）: 3戦略（カテゴリ別/段階的開示/ルーティング型）
  - 推論オーバーロード防止: 5つの対策、ツール数目安、統合例
  - tool_choiceの高度な使い方: 強制実行/ステップ制御/バリデーション
  - 用語集5語追加、確認クイズ5問追加（計10問）
- [x] Module 6「MCP統合」拡充（4セクション追加+クイズ5問追加）
  - MCP設定ファイルの階層構造: .mcp.json vs ~/.claude.json の優先順位
  - MCPサーバー設計パターン: 認証3方式、Roots、ベストプラクティス
  - MCPの高度な機能: サンプリング、通知（Progress/Log）、初期化ハンドシェイク
  - トランスポート選定ガイド: stdio/Streamable HTTP/ステートレスHTTPの比較
  - 用語集7語追加、確認クイズ5問追加（計10問）
- [x] Module 9「エージェントアーキテクチャ」拡充（Phase B1で統合済み）
  - Agent SDK実装、Secure Deployment（隔離・最小権限・多層防御）
  - エラー伝搬設計（isError/isRetryable）、コンテキスト隔離
  - 確認クイズ5問追加（計10問）
- [x] Module 10「マルチエージェント」拡充（Phase B1で統合済み）
  - Hub-and-Spoke詳細、Agent-as-Tool vs Handoff深掘り
  - Guardrails/HITL/エスカレーション、観測可能性
  - 確認クイズ5問追加（計10問）

## 次にやること

- M5・M6・M9・M10の拡充完了。残りのモジュール拡充（M11: D5強化等）へ

## 作業の進め方

新しいセッションでは、まず以下を読んでもらう：
1. `PROJECT.md` — 全体方針とルールの把握
2. `STATUS.md`（このファイル）— 現在の状況の把握
3. `index.html` の該当部分 — 既存の構造の確認

その上で「Module X のコンテンツを作成して」と依頼する。

## 備考

- 公式リソース（Anthropic Academy、GitHubコース）の内容を参考にしつつ、非エンジニア向けに噛み砕いて日本語化する方針
- 社内横展開も想定しているため、分かりやすさを最優先
