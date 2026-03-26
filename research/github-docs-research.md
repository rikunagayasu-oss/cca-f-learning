# GitHub Courses & 公式Docs 調査レポート

> 調査日: 2026-03-26
> 担当: GO03-A2

---

## 1. GitHub Courses（全5コース）詳細

リポジトリ: https://github.com/anthropics/courses

### 1-1. Anthropic API Fundamentals

**難易度: 基礎（入門）** | **管理レベル内容: なし**

| # | Notebook | 内容概要 |
|---|----------|----------|
| 01 | 01_getting_started.ipynb | APIキー取得、SDK導入、初回リクエスト |
| 02 | 02_messages_format.ipynb | Messages API形式、レスポンス解析、マルチターン会話 |
| 03 | 03_models.ipynb | Claudeモデルファミリー比較（速度・機能・性能） |
| 04 | 04_parameters.ipynb | max_tokens/temperature/stop_sequence解説、トークンとコストの関係 |
| 05 | 05_Streaming.ipynb | ストリーミングレスポンス実装 |
| 06 | 06_vision.ipynb | 画像入力（Vision）プロンプティング |

完全に開発者入門向け。パラメータ編でトークン課金への言及がある程度。

### 1-2. Prompt Engineering Interactive Tutorial

**難易度: 基礎〜中級** | **管理レベル内容: なし**

Anthropic 1P版とAmazon Bedrock版の2バージョンあり。

| # | Notebook | 内容概要 |
|---|----------|----------|
| 00 | 00_Tutorial_How-To.ipynb | チュートリアルの使い方ガイド |
| 01 | 01_Basic_Prompt_Structure.ipynb | 基本プロンプト構造 |
| 02 | 02_Being_Clear_and_Direct.ipynb | 明確で直接的なプロンプト |
| 03 | 03_Assigning_Roles_Role_Prompting.ipynb | ロール付与 |
| 04 | 04_Separating_Data_and_Instructions.ipynb | データと指示の分離 |
| 05 | 05_Formatting_Output_and_Speaking_for_Claude.ipynb | 出力フォーマット指定 |
| 06 | 06_Precognition_Thinking_Step_by_Step.ipynb | Chain of Thought、XMLタグ構造化 |
| 07 | 07_Using_Examples_Few-Shot_Prompting.ipynb | Few-Shotプロンプティング |
| 08 | 08_Avoiding_Hallucinations.ipynb | ハルシネーション回避 |
| 09 | 09_Complex_Prompts_from_Scratch.ipynb | 複雑プロンプト構築（法務・金融・SW開発の業界別実例、10要素テンプレート） |
| 10.1 | 10.1_Appendix_Chaining_Prompts.ipynb | プロンプトチェーニング |
| 10.2 | 10.2_Appendix_Tool_Use.ipynb | ツール利用の付録 |
| 10.3 | 10.3_Appendix_Search_Retrieval.ipynb | 検索・情報取得の付録 |

開発者・プロンプト設計者向け。Chapter 9の業界別例は実務的だが、運用管理視点はない。

### 1-3. Real World Prompting

**難易度: 中級** | **管理レベル内容: わずか**

前提: Prompt Engineering Tutorial完了を推奨。

| # | Notebook | 内容概要 |
|---|----------|----------|
| 01 | 01_prompting_recap.ipynb | プロンプティング技術の復習 |
| 02 | 02_medical_prompt.ipynb | 医療分野プロンプト設計 |
| 03 | 03_prompt_engineering.ipynb | プロンプトエンジニアリングのプロセス定義（反復改善・スケーラビリティ） |
| 04 | 04_call_summarizer.ipynb | 通話要約プロンプト設計 |
| 05 | 05_customer_support_ai.ipynb | カスタマーサポートAI構築（ハルシネーション防止、スコープ制限、RAG言及） |

Lesson 5でハルシネーション防止・RAG・本番テストに言及するが、ガバナンス・エンタープライズ運用の体系的内容はない。

### 1-4. Prompt Evaluations

**難易度: 中級〜上級** | **管理レベル内容: 部分的にあり**

| # | Notebook | 内容概要 |
|---|----------|----------|
| 01 | 01_intro_to_evals.ipynb | 評価の概念導入（「測定能力の欠如が本番LLM利用の最大障害」） |
| 02 | 02_workbench_evals.ipynb | Anthropic Workbenchでの人間評価 |
| 03 | 03_code_graded.ipynb | コードベース自動評価 |
| 04 | 04_code_graded_classification_evals.ipynb | 分類タスクのコードベース評価 |
| 05 | lesson.ipynb (promptfoo) | Promptfoo導入、コードグレード評価 |
| 06 | lesson.ipynb (promptfoo分類) | Promptfoo分類評価 |
| 07 | lesson.ipynb (カスタムグレーダー) | カスタムグレーダー実装 |
| 08 | lesson.ipynb (モデルグレード) | LLM-as-a-judge |
| 09 | lesson.ipynb (カスタムモデルグレード) | 3指標自動採点システム（簡潔性・正確性・トーン） |

5コース中で管理レベルに最も近い。品質管理・測定の体系化が本番運用でのプロンプト品質保証に直結。ただし「開発者が品質を担保する手法」であり、組織レベルのAIガバナンスではない。

### 1-5. Tool Use

**難易度: 中級** | **管理レベル内容: わずか**

| # | Notebook | 内容概要 |
|---|----------|----------|
| 01 | 01_tool_use_overview.ipynb | ツール利用の概念・ユースケース・全体像 |
| 02 | 02_your_first_simple_tool.ipynb | 初ツール実装 |
| 03 | 03_structured_outputs.ipynb | ツールによるJSON構造化出力強制 |
| 04 | 04_complete_workflow.ipynb | 完全ワークフロー実装 |
| 05 | 05_tool_choice.ipynb | tool_choiceパラメータ制御 |
| 06 | 06_chatbot_with_multiple_tools.ipynb | 複数ツール統合チャットボット（カスタマーサポート例） |

Lesson 6で権限管理・トランザクション整合性の本番対応に言及するが実装は含まない。

### GitHub Courses 総合判定

| コース | 難易度 | 管理/アーキテクトレベル | CCA-F対応ドメイン |
|--------|--------|----------------------|-----------------|
| API Fundamentals | 基礎 | **なし** | D4,D5 |
| Prompt Engineering Tutorial | 基礎〜中級 | **なし** | D4 |
| Real World Prompting | 中級 | **わずか** | D4 |
| Prompt Evaluations | 中級〜上級 | **部分的** | D4,D5 |
| Tool Use | 中級 | **わずか** | D2 |

**結論**: 5コースすべてが「開発者・プロンプト設計者」向けの実装教材。管理レベル・アーキテクトレベル（運用管理、ガバナンス、セキュリティポリシー、コスト管理戦略、監査・コンプライアンス等）の体系的内容は含まれていない。

---

## 2. 公式ドキュメント（docs.anthropic.com）運用管理・ガバナンス系セクション

> 注: docs.anthropic.com は現在 platform.claude.com/docs にリダイレクト（301）

### 2-1. 運用管理・ガバナンス系セクション一覧

| セクション | 概要 |
|---|---|
| **Admin API overview** | 組織リソース（メンバー、Workspace、APIキー）のプログラム管理 |
| **Usage and Cost API** | 利用量・コストデータのAPI取得（トークン消費、コスト内訳） |
| **Claude Code Analytics API** | 開発者生産性・Claude Code導入状況モニタリング |
| **Rate Limits** | RPM/ITPM/OTPM制限、Tier別制限値 |
| **Service Tiers** | Standard/Priority/Batch 3段階のサービスレベル |
| **Security and Compliance** | Trust Centerへの案内（SOC2、ISO等） |
| **Privacy Policy** | プライバシーポリシー本文へのリンク |
| **Strengthen Guardrails** | ハルシネーション削減、ジェイルブレイク対策、ストリーミング拒否処理 |
| **Content Moderation** | コンテンツモデレーションガイド |
| **Secure Deployment** | エージェントの安全なデプロイ手法 |
| **Claude Code Security** | Claude Codeのセキュリティアーキテクチャ |
| **Data Usage (Claude Code)** | Claude Codeのデータ取扱い |

### 2-2. Enterprise向け機能

| 機能 | 詳細 |
|---|---|
| **SSO（SAML）** | Team/Enterpriseプランで利用可能 |
| **SCIM** | ユーザー自動プロビジョニング・アクセス制御自動化 |
| **監査ログ** | ユーザーアクション、システムイベント、データアクセスの追跡 |
| **ドメインキャプチャ** | SSO設定時に組織ドメインユーザーを自動キャプチャ |
| **カスタムデータ保持** | 保持期間カスタマイズ（ゼロデータ保持も可能） |
| **ロールベースパーミッション** | 細粒度のユーザー権限管理 |
| **HIPAA対応** | Enterprise Regulatedティア |
| **MCP管理者制御** | 管理者がユーザーに許可するMCPコネクターを制御 |

### 2-3. Admin API 詳細

Admin APIキー（`sk-ant-admin...`プレフィックス）が必要。adminロールのメンバーのみ発行可能。

| リソース | 操作 | エンドポイント |
|---|---|---|
| Organization Info | 取得 | `GET /v1/organizations/me` |
| Organization Members | 一覧/更新/削除 | `/v1/organizations/users` |
| Organization Invites | 作成/一覧/削除 | `/v1/organizations/invites` |
| Workspaces | 作成/一覧/取得/更新 | `/v1/organizations/workspaces` |
| Workspace Members | 追加/一覧/更新/削除 | `/v1/organizations/workspaces/{id}/members` |
| API Keys | 一覧/更新 | `/v1/organizations/api_keys` |
| Usage Report | 取得 | `/v1/organizations/usage_report/messages` |
| Cost Report | 取得 | `/v1/organizations/cost_report` |

#### 組織ロール（5段階）

| ロール | 権限 |
|---|---|
| user | Workbenchのみ |
| claude_code_user | Workbench + Claude Code |
| developer | Workbench + APIキー管理 |
| billing | Workbench + 請求管理 |
| admin | 全権限 + ユーザー管理 |

#### ベストプラクティス
- Workspace・APIキーには意味のある名前と説明を付与
- メンバーのロール・権限を定期的に監査
- 未使用Workspace・期限切れ招待をクリーンアップ
- APIキーの利用状況モニタリング、定期ローテーション

### 2-4. Rate Limits & Service Tiers

#### 使用量ティア

| ティア | 必要クレジット購入額 | 月間上限 |
|---|---|---|
| Tier 1 | $5 | $100 |
| Tier 2 | $40 | $500 |
| Tier 3 | $200 | $1,000 |
| Tier 4 | $400 | $200,000 |
| Monthly Invoicing | N/A | 無制限 |

#### Tier 4 レート制限（主要モデル）

| モデル | RPM | ITPM | OTPM |
|---|---|---|---|
| Claude Opus 4.x | 4,000 | 2,000,000 | 400,000 |
| Claude Sonnet 4.x | 4,000 | 2,000,000 | 400,000 |
| Claude Haiku 4.5 | 4,000 | 4,000,000 | 800,000 |

重要仕様:
- `cache_read_input_tokens` はITPM制限にカウントされない（キャッシュ利用で実効スループット向上）
- Token Bucketアルゴリズム使用（連続補充）
- Workspace単位でカスタム制限設定可能

#### Service Tiers
- **Standard**: デフォルト、ベストエフォート
- **Priority**: 優先処理、99.5%稼働時間目標、1/3/6/12ヶ月コミットメント
- **Batch**: 非同期、50%割引

### 2-5. セキュリティ・コンプライアンス

- **Trust Center** (trust.anthropic.com): SOC 2 Type 2、ISO 27001等のコンプライアンスアーティファクト提供
- Claude Codeは「read-only権限をデフォルト」、追加アクションはユーザー明示許可を要求
- エージェント安全デプロイ: 隔離（Isolation）、最小権限（Least Privilege）、多層防御（Defense in Depth）

### 2-6. データプライバシー・保持ポリシー

| 区分 | データ保持期間 | モデル訓練への使用 |
|---|---|---|
| Consumer（Free/Pro/Max） | 改善許可時: 5年 / 不許可時: 30日 | 許可時のみ |
| Commercial（API/Work/Enterprise） | 30日以内に自動削除 | 訓練に使用しない |
| ゼロデータ保持 | Enterpriseで利用可能 | 訓練に使用しない |

- データは転送中・保存中の両方で自動暗号化
- ユーザーデータを第三者に販売しない

### 2-7. ガードレール強化（Strengthen Guardrails）

| トピック | 内容 |
|---|---|
| ハルシネーション削減 | 「I don't know」許可、直接引用グラウンディング、CoT検証、Best-of-N検証、反復的精緻化 |
| ジェイルブレイク対策 | プロンプトインジェクション防御 |
| ストリーミング拒否 | ストリーミング時の拒否ハンドリング |
| コンテンツモデレーション | モデレーションガイド |

---

## 3. 既存ROADMAP.md 1-2/1-3セクションとの差分

### 3-1. GitHub Courses（ROADMAP 1-2）との差分

| 項目 | ROADMAP.md 1-2 の記載 | 今回調査で判明した差分 |
|---|---|---|
| コース数 | 5コース | 一致 |
| API Fundamentals | 「APIキー取得・モデルパラメータ・マルチモーダル・ストリーミング」 | **Notebook 6本の詳細判明**。マルチモーダル=Vision(06)。概要は正確 |
| Prompt Engineering | 「プロンプト技法ステップバイステップ」 | **Notebook 14本（付録含む）**。Bedrock版も存在。業界別実例(09)・付録3本の存在が未記載 |
| Real World Prompting | 「複雑な実務プロンプトへの技法適用」 | **Notebook 5本**。医療・通話要約・カスタマーサポートの具体ドメインが未記載 |
| Prompt Evaluations | 「プロダクション向け評価パイプライン構築」 | **Notebook 9本**。Promptfoo統合・LLM-as-a-judge・3指標自動採点の詳細が未記載 |
| Tool Use | 「Tool Useの設計・実装」 | **Notebook 6本**。構造化出力強制・tool_choice・複数ツール統合チャットボットの詳細が未記載 |
| 対応ドメイン | D2,D4,D5 | **D4が圧倒的に多い**（4/5コースがD4対応）。D2はTool Useのみ。D5はAPI FundamentalsとEvals |
| 管理レベル内容 | 記載なし | **5コースとも管理/アーキテクトレベルの体系的内容なし**。Evalsコースのみ品質管理に部分的対応 |

### 3-2. 公式Docs（ROADMAP 1-3）との差分

| 項目 | ROADMAP.md 1-3 の記載 | 今回調査で判明した差分 |
|---|---|---|
| Agent SDK overview | 記載あり | 一致。加えてSecure Deploymentガイドが存在 |
| Tool use overview | 記載あり | 一致 |
| MCP configuration | 記載あり | 一致。加えてEnterprise向けMCP管理者制御が存在 |
| CLAUDE.md・Hooks | 記載あり | 一致 |
| Structured outputs | 記載あり | 一致 |
| Context management | 記載あり | 一致 |
| **Admin API** | **未記載** | **新規発見**: 組織管理（メンバー・Workspace・APIキー）のプログラム管理API |
| **Usage and Cost API** | **未記載** | **新規発見**: 利用量・コストデータAPI取得 |
| **Claude Code Analytics API** | **未記載** | **新規発見**: 開発者生産性モニタリングAPI |
| **Rate Limits詳細** | **未記載** | **新規発見**: Tier構造、RPM/ITPM/OTPM制限値、Token Bucketアルゴリズム |
| **Service Tiers** | **未記載** | **新規発見**: Standard/Priority/Batch 3段階、Priority=99.5%SLA |
| **Strengthen Guardrails** | **未記載** | **新規発見**: ハルシネーション削減・ジェイルブレイク対策・モデレーションガイド |
| **Enterprise機能** | **未記載** | **新規発見**: SSO/SCIM/監査ログ/ドメインキャプチャ/カスタムデータ保持/HIPAA |
| **組織ロール** | **未記載** | **新規発見**: 5段階ロール（user/claude_code_user/developer/billing/admin） |
| **データプライバシー** | **未記載** | **新規発見**: Consumer/Commercial/ゼロ保持の3区分、訓練利用ポリシー |
| **Secure Deployment** | **未記載** | **新規発見**: エージェント安全デプロイ（隔離・最小権限・多層防御） |
| **Claude Code Security** | **未記載** | **新規発見**: セキュリティアーキテクチャ・データ取扱い |

### 3-3. 差分の重要度評価

ROADMAPが記載していない領域のうち、CCA-F試験で出題される可能性が高いもの:

| 未記載領域 | CCA-F関連度 | 対応ドメイン | 理由 |
|---|---|---|---|
| Rate Limits & Service Tiers | **高** | D5 | コンテキスト管理と信頼性に直結。Token Bucket・キャッシュ非カウントは実務知識 |
| Strengthen Guardrails | **高** | D5 | ハルシネーション対策・セキュリティは信頼性の中核 |
| Secure Deployment | **高** | D1 | エージェントアーキテクチャの安全設計 |
| Admin API & 組織管理 | **中** | - | 管理者向け知識。試験の直接出題は限定的だが、Enterprise文脈で問われる可能性 |
| データプライバシー | **中** | D5 | コンプライアンス観点で出題の可能性あり |
| Enterprise機能 | **低〜中** | - | SSO/SCIM等は試験の中心ではないが、シナリオ問題で文脈として登場の可能性 |

---

## 4. 主要Sources

- GitHub courses: https://github.com/anthropics/courses
- Admin API: https://docs.anthropic.com/en/docs/administration/administration-api
- Rate Limits: https://docs.anthropic.com/en/api/rate-limits
- Service Tiers: https://docs.anthropic.com/en/api/service-tiers
- Usage/Cost API: https://docs.anthropic.com/en/api/usage-cost-api
- Enterprise Plan: https://claude.com/pricing/enterprise
- Enterprise Features: https://support.anthropic.com/en/collections/10351014-enterprise-plan-features
- Trust Center: https://trust.anthropic.com/
- Privacy Center: https://privacy.anthropic.com/
- Reduce Hallucinations: https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations
- Secure Deployment: https://platform.claude.com/docs/en/agent-sdk/secure-deployment
- Claude Code Security: https://docs.anthropic.com/en/docs/claude-code/security
