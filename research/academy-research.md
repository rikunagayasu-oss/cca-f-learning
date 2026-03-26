# Anthropic Academy 全13コース 内容精査レポート

> 調査日: 2026-03-26
> 調査元: https://anthropic.skilljar.com/

## 調査目的

Anthropic Academy全13コースの内容を精査し、以下を明らかにする:
1. 各コースの内容概要（レッスン数・トピック一覧）
2. 難易度レベル（基礎/中級/上級/管理レベル）の判定
3. 管理・ガバナンス・運用管理に関するコンテンツの有無と詳細
4. CCA-F試験範囲との対応関係

---

## 1. コース一覧サマリー

| # | コース名 | レッスン数 | 難易度 | 管理レベル内容 | CCA-F関連度 |
|---|---------|-----------|--------|-------------|-----------|
| 1 | Building with the Claude API | 87-98 | 中級 | なし | ★★★ 最重要 |
| 2 | Claude Code in Action | 10 | 中級 | なし | ★★★ 最重要 |
| 3 | Introduction to Model Context Protocol | 16 | 中級 | なし | ★★★ 高 |
| 4 | Model Context Protocol: Advanced Topics | 15 | 上級 | 微量（本番運用） | ★★☆ 中-高 |
| 5 | Introduction to Agent Skills | 約10-11 | 中級 | 微量（企業配布） | ★★☆ 中 |
| 6 | Claude 101 | 14 | 基礎 | なし | ★☆☆ 低 |
| 7 | AI Fluency: Framework & Foundations | 14 | 基礎-中級 | 一部（個人倫理） | ☆☆☆ 無関係 |
| 8 | AI Fluency for Educators | 4 | 中級（教育者） | 微量（カリキュラム） | ☆☆☆ 無関係 |
| 9 | AI Fluency for Students | 5 | 基礎（学生） | なし | ☆☆☆ 無関係 |
| 10 | AI Fluency for Nonprofits | 未公開 | 基礎（NPO） | あり（組織導入） | ☆☆☆ 無関係 |
| 11 | Teaching AI Fluency | 7 | 中級（教育者） | 一部（カリキュラム） | ☆☆☆ 無関係 |
| 12 | Claude with Amazon Bedrock | 16 | 中級-上級 | なし | ★★★ 高 |
| 13 | Claude with Google Cloud's Vertex AI | 16 | 中級-上級 | なし | ★★★ 高 |

---

## 2. 各コース詳細

### コース1: Building with the Claude API

- **URL**: https://anthropic.skilljar.com/claude-with-the-anthropic-api
- **レッスン数**: 87-98（7-8セクション構成。Agents and workflowsセクション含む版は98）
- **所要時間**: 8時間超
- **対象レベル**: 中級（Python開発者向け）
- **前提知識**: Pythonプログラミング、基本的なJSON知識

**セクション構成**:

| セクション | レッスン数 | 主要トピック |
|-----------|-----------|------------|
| Getting started with Claude | 16 | モデル概要、APIアクセス、マルチターン会話、システムプロンプト、温度パラメータ、ストリーミング、構造化データ |
| Prompt engineering & evaluation | 16 | プロンプト技法（明確性、XMLタグ、例示）、評価ワークフロー、テストデータ生成、モデルベース/コードベース採点 |
| Tool use with Claude | 14 | Tool Use導入、ツール関数・スキーマ、メッセージブロック処理、マルチターンツール会話、バッチツール、テキスト編集ツール、Web検索ツール |
| Retrieval augmented generation | 10 | RAG導入、テキストチャンキング、テキスト埋め込み、BM25検索、マルチインデックスパイプライン、リランキング、Contextual Retrieval、Extended Thinking |
| Model Context Protocol (MCP) | 12 | MCP導入、クライアント/サーバー実装、ツール・リソース・プロンプト定義、Server Inspector |
| Claude Code & Computer Use | 8 | Claude Codeセットアップ・実践、MCPサーバー連携、並列化、自動デバッグ、Computer Use |
| Agents and workflows | 11 | 並列化・チェーニング・ルーティングワークフロー、エージェントとツール、環境検査、画像/PDF/Citations/キャッシング |

**管理レベル内容**: なし。純粋に技術的な開発者向けコンテンツ。
**CCA-F対応**: 全5ドメインをカバー。CCA-F試験ガイドPDFがこのコースページからリンクされている。**最重要の準備コース**。

---

### コース2: Claude Code in Action

- **URL**: https://anthropic.skilljar.com/claude-code-in-action
- **レッスン数**: 10（1セクション）
- **対象レベル**: 中級（CLI・Git経験のある開発者向け）
- **前提知識**: コマンドライン操作、基本的なGit知識

**レッスン一覧**:
1. What is a Coding Assistant?
2. Claude Code in Action
3. Adding Context
4. Making Changes
5. Controlling Context
6. Custom Commands
7. Extending Claude Code with MCP Servers
8. Github Integration
9. Introducing Hooks / Defining Hooks / Implementing a Hook / Useful Hooks!
10. The Claude Code SDK

**管理レベル内容**: なし。個人の開発ワークフローに特化。
**CCA-F対応**: ドメイン3（Claude Code設定とワークフロー、20%）を直接カバー。シナリオ2（コード生成）・シナリオ5（CI/CD）に直結。

---

### コース3: Introduction to Model Context Protocol

- **URL**: https://anthropic.skilljar.com/introduction-to-model-context-protocol
- **レッスン数**: 16（2セクション）
- **対象レベル**: 中級（Python・JSON・HTTPの知識がある開発者向け）

**セクション構成**:

| セクション | レッスン数 | 内容 |
|-----------|-----------|------|
| MCP fundamentals & server development | 8 | MCPアーキテクチャ理解、Python SDKでのサーバー構築、Inspector |
| MCP client implementation & advanced features | 8 | クライアント実装、リソース・プロンプト定義、3プリミティブ（ツール/リソース/プロンプト） |

**レッスン一覧**:
1. Introducing MCP
2. MCP Clients
3. Project Setup
4. Defining Tools with MCP
5. The Server Inspector
6. Implementing a Client
7. Defining Resources
8. Accessing Resources
9. Defining Prompts
10. Prompts in the Client
11. MCP Review
12-16. 3プリミティブ詳細（ツール[モデル制御]、リソース[アプリ制御]、プロンプト[ユーザー制御]）、オートコンプリート、コンテキスト注入

**管理レベル内容**: なし。純粋にプロトコル実装の技術コース。
**CCA-F対応**: ドメイン2（ツール設計とMCP統合、18%）を直接カバー。

---

### コース4: Model Context Protocol: Advanced Topics

- **URL**: https://anthropic.skilljar.com/model-context-protocol-advanced-topics
- **レッスン数**: 15（2セクション）
- **対象レベル**: 上級（非同期Python、JSON、HTTP、SSE経験が必要）

**セクション構成**:

| セクション | レッスン数 | 内容 |
|-----------|-----------|------|
| Core MCP features | 8 | サンプリング、進捗通知、Roots、権限システム |
| Transports and communication | 7 | JSONメッセージプロトコル、Stdioトランスポート、StreamableHTTP、ステートレスHTTP、本番スケーリング |

**レッスン一覧**:
1. Sampling（AIコストのクライアント委譲）
2. Log and Progress Notifications
3. Roots（ファイルアクセス安全管理）
4. JSON Message Types
5. The Stdio Transport
6. The StreamableHTTP Transport
7. StreamableHTTP in Depth
8. Stateless HTTP
9-15. 権限システム、初期化ハンドシェイク、SSE通信、HTTPトランスポート制限、ロードバランサ構成、トランスポート選定基準

**管理レベル内容**: 微量。本番デプロイメント（スケーリング、トランスポート選定）の考慮事項を含むが、組織ガバナンスの内容はなし。
**CCA-F対応**: ドメイン2の高度な側面をカバー。MCPトランスポート機構や本番運用パターンに対応。

---

### コース5: Introduction to Agent Skills

- **URL**: https://anthropic.skilljar.com/introduction-to-agent-skills
- **レッスン数**: 約10-11トピック
- **対象レベル**: 中級（Claude Code利用者向け）

**トピック一覧**:
1. Skill fundamentals（CLAUDE.md・Hooks・サブエージェントとの違い）
2. Creating your first Skill from scratch
3. SKILL.md frontmatter and configuration
4. Writing effective descriptions for reliable matching
5. Directory organization and progressive disclosure
6. Advanced options（allowed-tools、コンテキスト効率的スクリプト）
7. Sharing Skills through repositories
8. Plugin distribution
9. Enterprise / organization-wide deployment
10. Wiring Skills into custom subagents
11. Troubleshooting common issues

**管理レベル内容**: 微量。エンタープライズ配布・組織全体デプロイメントに触れるが、技術実装視点であり、ガバナンス方針ではない。
**CCA-F対応**: ドメイン3（Claude Code設定とワークフロー、20%）に関連。試験ガイドにAgent Skills・MCPサーバー統合・Plan modeが明記。

---

### コース6: Claude 101

- **URL**: https://anthropic.skilljar.com/claude-101
- **レッスン数**: 14（4セクション + 修了証）
- **対象レベル**: 基礎（Claude初心者、一般ビジネスユーザー向け）

**セクション構成**:

| セクション | レッスン |
|-----------|---------|
| Meet Claude | What is Claude? / Your first conversation / Getting better results / Chat, Cowork, Code |
| Organizing your work and knowledge | Introduction to projects / Creating with artifacts / Working with skills |
| Expanding Claude's reach | Connecting your tools / Enterprise search / Research mode for deep dives |
| Putting it all together | Claude in action: use-cases by role / Other ways to work with Claude / What's next? |

**管理レベル内容**: なし。製品操作のウォークスルーコース。
**CCA-F対応**: 低。CCA-F準備コースとしてリストされているが（基礎概念のカバー）、試験の主要ドメインの内容は薄い。

---

### コース7: AI Fluency: Framework & Foundations

- **URL**: https://anthropic.skilljar.com/ai-fluency-framework-foundations
- **レッスン数**: 14（10セクション）
- **所要時間**: 3-4時間（約1.1時間のビデオ）
- **対象レベル**: 基礎-中級（全レベル対象）
- **開発**: Prof. Joseph Feller（University College Cork）、Prof. Rick Dakan（Ringling College）と共同開発

**4Dフレームワーク**:

| D | 名称 | 内容 |
|---|------|------|
| Delegation | 委任 | AIに任せる/人間が保持する判断 |
| Description | 記述 | 構造化プロンプティングによる意図伝達（6原則） |
| Discernment | 識別 | AI出力の体系的評価 |
| Diligence | 勤勉 | 透明性・倫理的責任（作成/透明性/展開の3層） |

**レッスン一覧**:
1. Introduction to AI Fluency
2. Why do we need AI Fluency?
3. The 4D Framework
4. Generative AI fundamentals
5. Capabilities & limitations
6. A closer look at Delegation / Project planning and Delegation
7. A closer look at Description
8. Effective prompting techniques
9. A closer look at Discernment
10. The Description-Discernment loop
11. A closer look at Diligence
12. Conclusion / Certificate

**管理レベル内容**: 一部。Diligenceセクションで責任あるAI利用・透明性・倫理的展開を扱うが、個人レベルの実践であり組織ガバナンスではない。
**CCA-F対応**: 無関係。CCA-F準備コースとしてはリストされていない。

---

### コース8: AI Fluency for Educators

- **URL**: https://anthropic.skilljar.com/ai-fluency-for-educators
- **レッスン数**: 4（3セクション + 修了証）
- **所要時間**: 約3時間（35分のビデオ）
- **対象レベル**: 中級（教育者—教員、ID、教育リーダー向け）
- **前提**: 4Dフレームワークの理解

**レッスン一覧**:
1. Introduction to AI Fluency for Educators
2. AI Fluency Framework review
3. Applying AI Fluency to course design and learning outcomes
4. Applying AI Fluency to learning materials and assignments

**管理レベル内容**: 微量。カリキュラム設計・制度戦略に触れるが教育文脈に限定。
**CCA-F対応**: 無関係。

---

### コース9: AI Fluency for Students

- **URL**: https://anthropic.skilljar.com/ai-fluency-for-students
- **レッスン数**: 5（3セクション + 修了証）
- **所要時間**: 約3時間（30分のビデオ）
- **対象レベル**: 基礎（大学生・初期キャリア向け）
- **前提**: 4Dフレームワークの理解

**レッスン一覧**:
1. Welcome to AI Fluency for students
2. AI Fluency Framework
3. AI as a learning partner
4. AI in career planning
5. Being the human in the loop

**管理レベル内容**: なし。個人の学習・キャリア活用に特化。
**CCA-F対応**: 無関係。

---

### コース10: AI Fluency for Nonprofits

- **URL**: https://anthropic.skilljar.com/ai-fluency-for-nonprofits
- **レッスン数**: 未公開（4Dフレームワーク準拠の短期コース）
- **対象レベル**: 基礎（NPOスタッフ全般—資金調達、広報、事業運営、リーダーシップ）
- **共同開発**: GivingTuesday

**トピック（確認済み）**:
- 4Dフレームワーク（Delegation/Description/Discernment/Diligence）のNPO文脈適用
- 助成金執筆、プログラム評価、ドナーエンゲージメント、組織効率化
- 限られたリソースでの組織的AI導入
- ミッション整合型展開、ステークホルダーへの説明責任

**管理レベル内容**: **あり**。限られたリソースでの組織AI導入、ミッション整合型展開、ステークホルダー説明責任、組織ワークフローへのAI統合を扱う。**ただし非技術的な文脈**。
**CCA-F対応**: 無関係。

---

### コース11: Teaching AI Fluency

- **URL**: https://anthropic.skilljar.com/teaching-ai-fluency
- **レッスン数**: 7（約5-6時間、ビデオ約70分）
- **対象レベル**: 中級（教員、ID、トレーナー、ファシリテーター向け）
- **前提**: AI Fluency: Framework & Foundations修了

**レッスン一覧**:
1. 4Dフレームワークの教授アプローチ
2. AI Fluencyフレームワークの教育文脈への導入ポイント
3. AI Fluency評価・課題設計
4. カリキュラム統合戦略
5. AIを思考パートナーとしたコース設計
6. AI活用の学習教材作成
7. 責任あるAI協働のモデリングと真正性のある評価

**管理レベル内容**: 一部。カリキュラム計画・組織的学習設計を扱うが教育文脈に限定。
**CCA-F対応**: 無関係。

---

### コース12: Claude with Amazon Bedrock

- **URL**: https://anthropic.skilljar.com/claude-in-amazon-bedrock
- **レッスン数**: 16（Building with the Claude APIと約70%重複）
- **対象レベル**: 中級-上級（バックエンド開発者、MLエンジニア、DevOpsエンジニア）
- **前提**: Python、基本的なAWSサービス・Bedrock知識

**主要トピック**:
1. Amazon Bedrock経由のClaudeモデル設定
2. プロンプトエンジニアリング・評価
3. Tool Use（JSON Schemaツール定義、関数呼び出し）
4. RAG（テキストチャンキング、ベクトル埋め込み、ハイブリッド検索）
5. 高度な機能（Extended Thinking、Vision、PDF処理、キャッシング）
6. Claude Code（自動デバッグ、タスク実行）
7. Computer Use
8. MCP（ツール・リソース・プロンプト実装）
9. 推論最適化（ストリーミング、温度制御）
10. エージェントワークフロー（並列化、チェーニング、ルーティング）

**管理レベル内容**: なし。純粋に技術実装コース（AWS SDK/boto3使用）。
**CCA-F対応**: 高。Building with the Claude APIと同等のドメインカバレッジだがAWS SDK経由。

---

### コース13: Claude with Google Cloud's Vertex AI

- **URL**: https://anthropic.skilljar.com/claude-with-google-vertex
- **レッスン数**: 16（Building with the Claude APIと約70%重複）
- **対象レベル**: 中級-上級（バックエンド開発者、MLエンジニア、DevOps）
- **前提**: Python、Google Cloud Platform経験

**主要トピック**: コース12（Bedrock版）と並列構成。全例がGCP SDK経由。

**管理レベル内容**: なし。
**CCA-F対応**: 高。Bedrock版と同等。

---

## 3. CCA-F試験との対応関係

### CCA-F 5ドメインとコースのマッピング

| ドメイン | 配点 | 主要対応コース | 補助コース |
|---------|------|-------------|-----------|
| D1: エージェント型アーキテクチャとオーケストレーション | 27% | Building with the Claude API | Introduction to Agent Skills |
| D2: ツール設計とMCP統合 | 18% | Intro to MCP, MCP: Advanced Topics | Building with the Claude API |
| D3: Claude Code設定とワークフロー | 20% | Claude Code in Action | Introduction to Agent Skills |
| D4: プロンプトエンジニアリングと構造化出力 | 20% | Building with the Claude API | Claude 101 |
| D5: コンテキスト管理と信頼性 | 15% | Building with the Claude API | （各コースに分散） |

### コース別CCA-F有効度

| 有効度 | コース |
|--------|-------|
| ★★★ 最重要 | 1. Building with the Claude API（全ドメインカバー） |
| ★★★ 高 | 2. Claude Code in Action（D3直結） |
| ★★★ 高 | 3. Intro to MCP（D2直結） |
| ★★☆ 中-高 | 4. MCP: Advanced Topics（D2深堀り） |
| ★★☆ 中 | 5. Introduction to Agent Skills（D3補完） |
| ★★★ 高 | 12. Claude with Amazon Bedrock（全ドメイン、AWS版） |
| ★★★ 高 | 13. Claude with Vertex AI（全ドメイン、GCP版） |
| ★☆☆ 低 | 6. Claude 101（D4前提知識のみ） |
| ☆☆☆ 無関係 | 7-11. AI Fluencyシリーズ全5コース |

---

## 4. 管理・ガバナンス・運用管理コンテンツの分析

### 結論: **現行Academy 13コースに管理・アーキテクトレベルの体系的コンテンツは存在しない**

| カテゴリ | 該当コース | 内容 | 深度 |
|---------|-----------|------|------|
| 組織導入戦略 | AI Fluency for Nonprofits | 限られたリソースでの組織AI導入 | 浅い（非技術的） |
| 企業配布・展開 | Introduction to Agent Skills | エンタープライズ配布、組織全体デプロイメント | 微量（技術実装視点） |
| 本番運用 | MCP: Advanced Topics | スケーリング、トランスポート選定、ロードバランサ構成 | 中程度（インフラ限定） |
| 倫理・責任 | AI Fluency: Framework & Foundations | Diligence（透明性・倫理的責任） | 浅い（個人レベル） |
| **ガバナンス** | **なし** | — | — |
| **セキュリティポリシー** | **なし** | — | — |
| **ROI評価** | **なし** | — | — |
| **変更管理** | **なし** | — | — |
| **コンプライアンス** | **なし** | — | — |

### 今後の展開

Anthropicは2026年後半に追加の資格レベルを予定:
- **Advanced Architect認定**: CCA-Fの上位。エンタープライズガバナンスを含む可能性あり
- **Developer認定**: 実装特化
- **Seller認定**: パートナー営業チーム向け

また、以下のエンタープライズ向けコースの存在が確認されている（13コースとは別枠の可能性）:
- "Driving enterprise adoption of Claude"
- "Enterprise train-the-trainer"

これらは組織ロールアウトに焦点を当てており、管理レベルのコンテンツに最も近い。

---

## 5. ROADMAP.md 1-1セクションとの差分

### ROADMAP.md記載と本調査の差分

| # | ROADMAP記載 | 本調査での修正・補足 |
|---|-----------|-------------------|
| 1 | Building with the Claude API — 8時間超 | レッスン数87-98と判明。7-8セクション構成。Agents and workflowsセクション含む版あり |
| 2 | Claude Code in Action — 21レッスン | **10レッスン**（1セクション）。Hooks部分にサブレッスンあり |
| 3 | Introduction to MCP — 3プリミティブ基礎 | 16レッスン・2セクション。クライアント実装まで含む |
| 4 | MCP: Advanced Topics — サンプリング等 | 15レッスン・2セクション。本番スケーリング・トランスポート選定まで含む |
| 5 | Introduction to Agent Skills — Skills構築等 | 約10-11トピック。エンタープライズ配布・サブエージェント連携も含む |
| 6 | Claude 101 — 基本操作 | 14レッスン・4セクション。Projects、Artifacts、Skills、Research mode含む |
| 7 | AI Fluency: Framework & Foundations — 4D | 14レッスン・10セクション。3-4時間。CC BY-NC-SA。正確 |
| 8 | AI Fluency for Educators | 4レッスン・3セクション。約3時間。前提: 4Dフレームワーク |
| 9 | AI Fluency for Students | 5レッスン・3セクション。約3時間。前提: 4Dフレームワーク |
| 10 | AI Fluency for Nonprofits | レッスン数未公開。GivingTuesdayと共同開発。**管理レベル内容あり** |
| 11 | Teaching AI Fluency | 7レッスン。5-6時間。ビデオ約70分。前提: 4Dフレームワーク |
| 12 | Claude with Amazon Bedrock | 16レッスン。APIコースと約70%重複。元はAWS社員向け |
| 13 | Claude with Vertex AI | 16レッスン。APIコースと約70%重複。GCP SDK版 |

**主な差分**:
- コース2のレッスン数: ROADMAP記載の「21レッスン」→ 実際は**10レッスン**
- コース12・13のCCA-F関連度: ROADMAP記載の「低」→ 実際は**高**（APIコースと同等のドメインカバレッジ）
- ROADMAP未記載: 各コースの前提条件・セクション構成・修了証の有無

---

## 6. 主公への提言（学習サイト拡充に向けて）

### 管理・アーキテクトレベルのコンテンツについて

現行Academyには管理・ガバナンスの体系的コンテンツが**存在しない**。学習サイトに管理レベルのコンテンツを含める場合、以下のアプローチが考えられる:

1. **公式ドキュメントからの抽出**: docs.anthropic.comのエンタープライズ/セキュリティセクション
2. **CCA-F上位資格の動向監視**: 2026年後半予定のAdvanced Architect認定の内容公開を待つ
3. **独自コンテンツ作成**: 組織導入・ガバナンス・ROI評価は公式Academy外から構成する必要あり

### CCA-F試験対策としての優先学習コース

1. **Building with the Claude API**（必修 — 全ドメインカバー）
2. **Claude Code in Action**（D3対策）
3. **Introduction to MCP** + **MCP: Advanced Topics**（D2対策）
4. **Introduction to Agent Skills**（D3補完）
5. **Claude 101**（前提知識確認）
