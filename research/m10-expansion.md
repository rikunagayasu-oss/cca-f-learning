# M10 マルチエージェント — 拡充コンテンツ（HTMLスニペット）

> 作成日: 2026-03-26
> タスク: GO04-A3（武将19・太史慈）
> 挿入先: index.html の M10セクション内（既存セクション4の後、用語集の前に挿入を想定）
> 注意: index.htmlには直接触れない（Phase Bで呂蒙が挿入）

---

## 挿入するセクションタブボタン（既存のsection-navに追加）

```html
<button class="section-tab" onclick="showSection('m10','s7',this)">高度なパターン</button>
<button class="section-tab" onclick="showSection('m10','s8',this)">情報追跡と障害対応</button>
```

---

## セクション7: 高度なマルチエージェントパターン

```html
<!-- Section 7: 高度なマルチエージェントパターン（拡充コンテンツ） -->
<div id="m10-s7" class="card" style="display:none">
  <h2>高度なマルチエージェントパターン</h2>
  <p>Module 10の前半では5つの基本パターンを学びました。ここでは、実際のプロダクション環境で使われるより高度なパターンを学びます。CCA-F試験のシナリオ問題（特にシナリオ3「マルチエージェントリサーチ」）で問われる重要テーマです。</p>

  <h3>Hub-and-Spoke（ハブ＆スポーク）パターン</h3>

  <div class="concept-box">
    <div class="label">重要概念</div>
    <p><strong>Hub-and-Spoke（ハブ＆スポーク）：</strong>中央のリードエージェント（ハブ）がタスクを分解し、複数のサブエージェント（スポーク）に並行して割り振り、結果を集約する設計パターン。自転車の車輪のように、中心（ハブ）から放射状にスポークが伸びる構造です。</p>
  </div>

  <p>たとえば「AIの医療活用について調査して」というリクエストを受けた場合：</p>
  <ol>
    <li><strong>リードエージェント（ハブ）</strong>がリクエストを分析し、調査戦略を立てる</li>
    <li>「診断AI」「創薬AI」「医療記録AI」など<strong>3〜5つのサブタスク</strong>に分解する</li>
    <li>各サブタスクを<strong>専門のサブエージェント（スポーク）に並行して</strong>割り振る</li>
    <li>各サブエージェントが独立して調査し、結果をリードエージェントに返す</li>
    <li>リードエージェントが全結果を<strong>統合・整理</strong>して最終レポートを作成する</li>
  </ol>

  <p>空港に例えると分かりやすいでしょう。空港（ハブ）から各都市（スポーク）に飛行機が飛びます。各都市間の直行便は基本的になく、必ずハブを経由します。マルチエージェントでも同様に、<strong>サブエージェント同士が直接通信することはなく</strong>、必ずリードエージェントを経由します。</p>

  <div class="tip-box">
    <div class="label">試験のポイント</div>
    <p style="margin-top:.25rem;">Hub-and-Spokeは前半で学んだ<strong>Orchestrator-Workers</strong>パターンの具体的な実装形態です。試験では「サブエージェント同士は直接通信できない（兄弟サブエージェント間に直接チャネルはない）」という制約がポイントになります。英語では "sibling subagents cannot communicate directly; they report back to the parent" と表現されます。</p>
  </div>

  <h4>Hub-and-Spokeの設計ポイント</h4>
  <table class="compare-table">
    <tr><th>設計要素</th><th>推奨アプローチ</th><th>避けるべきこと</th></tr>
    <tr><td><strong>サブタスクの定義</strong></td><td>目的・出力形式・使用ツール・タスク境界を明確に指定</td><td>曖昧な指示（「適当に調べて」）</td></tr>
    <tr><td><strong>並行数</strong></td><td>3〜5サブエージェント程度（リードが管理できる範囲）</td><td>10以上の大量並行（統合品質が低下）</td></tr>
    <tr><td><strong>コンテキスト隔離</strong></td><td>各サブエージェントに独立したコンテキストウィンドウ</td><td>全サブエージェントでコンテキストを共有</td></tr>
    <tr><td><strong>結果統合</strong></td><td>リードエージェントが不足を判断し、追加調査を指示</td><td>サブエージェントの結果をそのまま結合</td></tr>
  </table>

  <div class="warn-box">
    <div class="label">注意</div>
    <p style="margin-top:.25rem;">Hub-and-Spokeではトークン消費が大幅に増加します。Anthropicの実測では、シングルエージェントの約4倍のトークンを消費するのに対し、マルチエージェントでは約15倍になることもあります。コストと品質のトレードオフを常に意識しましょう。</p>
  </div>

  <h3>Agent-as-Tool と Handoff の使い分け（詳細版）</h3>

  <p>前半で学んだ2つの連携方式を、より実践的な判断基準で深掘りします。</p>

  <div class="concept-box">
    <div class="label">判断フローチャート</div>
    <p><strong>Q1: サブタスクの結果を、呼び出し元が加工・統合する必要があるか？</strong></p>
    <p>→ Yes: <strong>Agent-as-Tool</strong>（結果を受け取って次の処理に使う）</p>
    <p>→ No: 次の質問へ</p>
    <p><strong>Q2: ユーザーとの対話を、別のエージェントに完全に引き継ぎたいか？</strong></p>
    <p>→ Yes: <strong>Handoff</strong>（制御権を完全移譲）</p>
    <p>→ No: <strong>Agent-as-Tool</strong>（呼び出し元が対話を継続）</p>
  </div>

  <h4>実践例：カスタマーサポートシステム</h4>
  <table class="compare-table">
    <tr><th>シーン</th><th>適切な方式</th><th>理由</th></tr>
    <tr><td>注文状況を確認してユーザーに伝える</td><td><strong>Agent-as-Tool</strong></td><td>注文確認エージェントの結果を、メインエージェントがユーザーに伝える</td></tr>
    <tr><td>技術的な問題で専門サポートに完全移管</td><td><strong>Handoff</strong></td><td>技術サポートエージェントがユーザーと直接対話する方が効率的</td></tr>
    <tr><td>返金処理の前に規約を確認する</td><td><strong>Agent-as-Tool</strong></td><td>規約確認の結果をメインエージェントが判断材料にする</td></tr>
    <tr><td>英語ユーザーを英語対応チームに移す</td><td><strong>Handoff</strong></td><td>言語の異なるエージェントに対話自体を引き継ぐ</td></tr>
  </table>

  <div class="tip-box">
    <div class="label">試験のポイント</div>
    <p style="margin-top:.25rem;">CCA-Fのシナリオ問題では「このケースでAgent-as-ToolとHandoffのどちらを使うべきか」が頻出です。<strong>「制御権」と「結果の利用方法」</strong>の2軸で判断しましょう。結果を統合する必要があればAgent-as-Tool、完全な担当交代ならHandoff。迷ったら「電話相談（Agent-as-Tool）か電話転送（Handoff）か」で考えると明快です。</p>
  </div>

  <h3>マルチエージェントのガードレール強化</h3>

  <p>前半で学んだ入力/出力ガードレールに加え、マルチエージェント特有の安全設計を学びます。</p>

  <h4>多層防御（Defense in Depth）</h4>
  <p>城の防御に例えると分かりやすいでしょう。外堀（入力バリデーション）、内堀（エージェント間の信頼境界）、天守閣（最終出力チェック）と、<strong>複数の防御層を重ねる</strong>ことで、1つの層が突破されても全体の安全性を保ちます。</p>

  <table class="compare-table">
    <tr><th>防御層</th><th>対象</th><th>具体例</th></tr>
    <tr><td><strong>入力バリデーション</strong></td><td>ユーザーからの入力</td><td>プロンプトインジェクション検出、入力長の制限</td></tr>
    <tr><td><strong>ツールレベル制限</strong></td><td>各エージェントのツールアクセス</td><td>最小権限の原則 — 各エージェントに必要なツールだけ付与</td></tr>
    <tr><td><strong>エージェント間の信頼境界</strong></td><td>エージェント間のデータ受け渡し</td><td>サブエージェントの出力を検証してからリードが使用</td></tr>
    <tr><td><strong>出力フィルタリング</strong></td><td>最終的なユーザーへの応答</td><td>個人情報（PII）の検出・マスキング、不適切コンテンツの除去</td></tr>
  </table>

  <h4>ソース品質ガードレール</h4>
  <p>マルチエージェントリサーチでは、エージェントが低品質な情報源（SEO対策だけのサイトなど）を使わないよう、<strong>情報源の品質基準</strong>をプロンプトに明記します。「公式ドキュメントや学術論文を優先し、コンテンツファーム（内容の薄い量産型サイト）は避けること」のような指示です。</p>

  <h4>検索戦略ガードレール</h4>
  <p>調査系のエージェントには「まず短くて広い検索語から始め、結果を評価してから徐々に焦点を絞る」という<strong>段階的な検索戦略</strong>を指示します。いきなり狭い検索をすると重要な情報を見逃すリスクがあるためです。</p>

  <div class="concept-box">
    <div class="label">重要概念</div>
    <p><strong>労力スケーリング（Effort Scaling）：</strong>リクエストの複雑さに応じてエージェントの投入量を調整するルール。たとえば「簡単な質問 → 1エージェント・3〜10回のツール呼び出し」「複雑な調査 → 10以上のサブエージェント」のように、事前に基準を設けます。過剰なリソース投入を防ぎ、コスト効率を高めます。</p>
  </div>
</div>
```

---

## セクション8: 情報追跡と障害対応

```html
<!-- Section 8: 情報追跡と障害対応（拡充コンテンツ） -->
<div id="m10-s8" class="card" style="display:none">
  <h2>情報追跡（Provenance）と障害対応</h2>
  <p>マルチエージェントシステムでは、「この情報はどのエージェントが、どの情報源から取得したか」を記録する<strong>情報出所（Provenance）追跡</strong>と、エージェントが失敗した場合の<strong>フォールバック（代替手段への切り替え）</strong>が欠かせません。</p>

  <h3>情報出所（Provenance）追跡</h3>

  <div class="concept-box">
    <div class="label">重要概念</div>
    <p><strong>情報出所 / プロベナンス（Provenance）：</strong>情報の「出どころ」と「経路」を追跡する仕組み。ワインの産地証明のように、「この情報はどこから来て、誰が加工したか」を記録します。マルチエージェントでは複数のエージェントが情報を収集・加工するため、最終レポートのどの部分がどの情報源に基づくかを明らかにすることが品質保証の鍵です。</p>
  </div>

  <p>新聞社の取材チームに例えましょう。複数の記者（サブエージェント）が現場で取材し、デスク（リードエージェント）が記事にまとめます。このとき、記事中の各事実に<strong>「誰が、どこで取材した情報か」</strong>が分からなければ、事実確認ができません。</p>

  <h4>Provenance追跡の3ステップ</h4>
  <table class="compare-table">
    <tr><th>ステップ</th><th>内容</th><th>比喩</th></tr>
    <tr><td><strong>1. 情報収集時の記録</strong></td><td>各サブエージェントが「どの情報源から、いつ取得したか」をメタデータとして付与</td><td>記者が取材メモに日時と取材先を記録</td></tr>
    <tr><td><strong>2. 統合時の引用付与</strong></td><td>リードエージェントが最終レポートを作成する際、各主張に情報源を紐づけ</td><td>デスクが記事の各段落に出典を付ける</td></tr>
    <tr><td><strong>3. 引用検証</strong></td><td>専用の引用エージェント（Citation Agent）が、主張と引用元の整合性を検証</td><td>校閲者が出典と記事の内容が一致するか確認</td></tr>
  </table>

  <div class="tip-box">
    <div class="label">試験のポイント</div>
    <p style="margin-top:.25rem;">Anthropicの公式マルチエージェントリサーチシステムでは、専用の<strong>Citation Agent（引用エージェント）</strong>がレポート内の全主張に対して情報源との照合を行います。CCA-F試験のシナリオ3（マルチエージェントリサーチ）では「引用の正確性（citation accuracy）」が評価基準として問われる可能性が高いです。英語では "citation accuracy: do the cited sources match the claims?" と表現されます。</p>
  </div>

  <h4>なぜProvenance追跡が重要か？</h4>
  <ul>
    <li><strong>ハルシネーション（幻覚）の検出</strong> — 情報源がない主張を特定できる</li>
    <li><strong>品質評価</strong> — 情報源の信頼度に基づいてレポートの品質を評価できる</li>
    <li><strong>デバッグ</strong> — 誤った情報がどのエージェントから来たか特定し、改善できる</li>
    <li><strong>法令遵守</strong> — 企業利用では情報の出所を示す義務がある場合がある</li>
  </ul>

  <h3>フォールバックループの設計</h3>

  <div class="concept-box">
    <div class="label">重要概念</div>
    <p><strong>フォールバック（Fallback）：</strong>主要な処理が失敗した場合に、代替手段に切り替える仕組み。飛行機のエンジンが1つ止まっても残りのエンジンで飛べるように、システムが完全に停止しないための「保険」です。</p>
  </div>

  <h4>マルチエージェントにおけるフォールバック戦略</h4>

  <p>Anthropicは「ツールが失敗していることをエージェントに伝え、エージェント自身に適応させる」アプローチが驚くほどうまく機能すると報告しています。エージェントにエラー情報を構造化して渡すことで、エージェント自身が最適な代替手段を選べるのです。</p>

  <table class="compare-table">
    <tr><th>フォールバック戦略</th><th>内容</th><th>比喩</th></tr>
    <tr><td><strong>リトライ（Retry）</strong></td><td>同じ処理を再試行する。一時的なエラー（ネットワーク障害など）に有効</td><td>電話がつながらなかったので、もう一度かけ直す</td></tr>
    <tr><td><strong>代替エージェント切替</strong></td><td>失敗したエージェントの代わりに、別の専門エージェントを起動する</td><td>担当医が不在なので、別の医師に診てもらう</td></tr>
    <tr><td><strong>段階的品質低下（Graceful Degradation）</strong></td><td>最高品質の処理が失敗した場合、品質を落としてでも結果を返す</td><td>特急列車が運休なので、各駅停車で目的地に向かう</td></tr>
    <tr><td><strong>状態保存付き再開（Stateful Resumption）</strong></td><td>エラー発生時点の状態を保存し、最初からではなくその地点から再開する</td><td>ゲームのセーブポイントから再開する</td></tr>
  </table>

  <h4>構造化エラーレスポンス</h4>
  <p>フォールバックを効果的に機能させるには、ツールがエラーを返す際に<strong>構造化された情報</strong>を含めることが重要です。</p>

  <div class="concept-box">
    <div class="label">エラーレスポンスの3要素</div>
    <p><strong>1. is_error</strong> — エラーであることを示すフラグ（true/false）</p>
    <p><strong>2. エラーの種類</strong> — 一時的なエラー（リトライ可能）か、恒久的なエラー（リトライ不可）か</p>
    <p><strong>3. 推奨アクション</strong> — 「別のツールを試してください」「パラメータを変えて再試行してください」などの具体的な指示</p>
    <p>これにより、エージェントは「ただ失敗した」ではなく「何がどう失敗し、次に何をすべきか」を判断できます。</p>
  </div>

  <h4>フォールバックの優先順位</h4>
  <p>複数のフォールバック手段がある場合、能力の高い順に試みます：</p>
  <ol>
    <li><strong>フル機能で再試行</strong>（一時的エラーの場合）</li>
    <li><strong>代替エージェントに切替</strong>（特定エージェントの問題の場合）</li>
    <li><strong>機能を制限して実行</strong>（リソース不足の場合）</li>
    <li><strong>最低限の応答を返す</strong>（すべて失敗した場合 — 「現在この処理は利用できません」）</li>
  </ol>

  <div class="warn-box">
    <div class="label">注意</div>
    <p style="margin-top:.25rem;">フォールバックでは<strong>無限ループに注意</strong>が必要です。リトライ回数の上限を必ず設定し、上限に達したら次の戦略に移行するか、人間にエスカレーションしましょう。Agent SDKでは <code>max_turns</code> でループ回数を制限できます。</p>
  </div>

  <h3>観測可能性（Observability）</h3>
  <p>マルチエージェントシステムでは、各エージェントが何をしているかを<strong>把握できる仕組み</strong>が不可欠です。</p>

  <div class="concept-box">
    <div class="label">重要概念</div>
    <p><strong>観測可能性（Observability）：</strong>システム内部の動作を外部から理解できる度合い。マルチエージェントでは「どのエージェントが、いつ、何のツールを使い、どんな結果を得たか」を追跡できることを指します。Anthropicは「エージェントの判断パターンとやり取りの構造を監視する — ただし個々の会話の内容は監視しない」というプライバシーに配慮した観測アプローチを推奨しています。</p>
  </div>

  <table class="compare-table">
    <tr><th>観測対象</th><th>内容</th><th>目的</th></tr>
    <tr><td><strong>メッセージストリーム</strong></td><td>Agent SDKが出力する各メッセージの型（SystemMessage, AssistantMessage等）</td><td>エージェントループのどの段階にいるか把握</td></tr>
    <tr><td><strong>ツール呼び出し履歴</strong></td><td>どのツールが何回呼ばれ、成功/失敗したか</td><td>ボトルネック特定、エラーパターン分析</td></tr>
    <tr><td><strong>トークン消費量</strong></td><td>各エージェントのトークン使用量</td><td>コスト管理、効率改善</td></tr>
    <tr><td><strong>判断パターン</strong></td><td>エージェントがどのような状況でどのツールを選んだか</td><td>プロンプト改善、品質向上</td></tr>
  </table>

  <div class="tip-box">
    <div class="label">試験のポイント</div>
    <p style="margin-top:.25rem;">CCA-F試験では「マルチエージェントシステムの信頼性をどう確保するか」が問われます。<strong>Provenance追跡 + フォールバック設計 + 観測可能性</strong>の3本柱を押さえましょう。特にAnthropicが強調する2つの原則を覚えてください：(1) "Let the agent know when a tool is failing and let it adapt"（エージェントに障害を伝え、適応させる）(2) "Monitor decision patterns, not conversation contents"（判断パターンを監視し、会話内容は監視しない）</p>
  </div>
</div>
```

---

## 用語集への追加項目（既存のm10-s5に追加）

```html
<div class="glossary-item">
  <div class="glossary-term">ハブ＆スポーク</div>
  <div class="glossary-en">Hub-and-Spoke</div>
  <div class="glossary-def">中央のリードエージェント（ハブ）がタスクを分解し、複数のサブエージェント（スポーク）に並行して割り振り、結果を集約するパターン。空港のハブ空港のように、全ての通信がハブを経由する。</div>
</div>
<div class="glossary-item">
  <div class="glossary-term">情報出所</div>
  <div class="glossary-en">Provenance</div>
  <div class="glossary-def">情報の「出どころ」と「経路」を追跡する仕組み。どのエージェントが、どの情報源から取得し、どう加工したかを記録し、最終成果物の各主張と情報源を紐づける。</div>
</div>
<div class="glossary-item">
  <div class="glossary-term">フォールバック</div>
  <div class="glossary-en">Fallback</div>
  <div class="glossary-def">主要な処理が失敗した場合に代替手段に切り替える仕組み。リトライ、代替エージェント切替、段階的品質低下、状態保存付き再開の4つの主要戦略がある。</div>
</div>
<div class="glossary-item">
  <div class="glossary-term">引用エージェント</div>
  <div class="glossary-en">Citation Agent</div>
  <div class="glossary-def">マルチエージェントリサーチで最終レポート内の全主張に対し、引用元との整合性を検証する専用エージェント。情報出所追跡の最終ステップを担う。</div>
</div>
<div class="glossary-item">
  <div class="glossary-term">段階的品質低下</div>
  <div class="glossary-en">Graceful Degradation</div>
  <div class="glossary-def">システムの一部が故障しても、品質を落としつつ動作を継続する設計。完全な停止を避け、可能な範囲で価値を提供し続ける。</div>
</div>
<div class="glossary-item">
  <div class="glossary-term">観測可能性</div>
  <div class="glossary-en">Observability</div>
  <div class="glossary-def">システム内部の動作を外部から理解できる度合い。マルチエージェントでは各エージェントの行動・ツール利用・判断パターンを追跡する仕組みを指す。</div>
</div>
<div class="glossary-item">
  <div class="glossary-term">労力スケーリング</div>
  <div class="glossary-en">Effort Scaling</div>
  <div class="glossary-def">リクエストの複雑さに応じてエージェントの投入量（数、ツール呼び出し回数等）を調整するルール。過剰なリソース投入を防ぎコスト効率を高める。</div>
</div>
```

---

## 確認クイズ5問（JSON形式）

以下を QUIZ_DATA の m10 配列に追加（既存5問の後ろに追加 = インデックス5〜9）：

```json
{
  "m10_expansion": [
    {
      "question": "マルチエージェントリサーチシステムで、リードエージェントが3つのサブエージェントに調査タスクを並行して割り振り、結果を統合してレポートを作成する設計パターンはどれですか？",
      "options": [
        "Evaluator-Optimizer（評価者・最適化者）パターン",
        "Hub-and-Spoke（ハブ＆スポーク）パターン",
        "Prompt Chaining（プロンプトチェーン）パターン",
        "Routing（ルーティング）パターン"
      ],
      "correct": 1,
      "explanation": "Hub-and-Spoke（ハブ＆スポーク）パターンは、中央のリードエージェント（ハブ）がタスクを分解し、複数のサブエージェント（スポーク）に並行して割り振り、結果を集約する設計です。Orchestrator-Workersの具体的な実装形態であり、サブエージェント同士は直接通信せず、必ずリードエージェントを経由します。"
    },
    {
      "question": "マルチエージェントシステムの情報出所（Provenance）追跡において、Anthropicの公式マルチエージェントリサーチシステムが採用しているアプローチはどれですか？",
      "options": [
        "リードエージェントが全ての情報源URLを記録するだけで十分とする",
        "サブエージェントの出力を暗号化して改ざんを防止する",
        "専用のCitation Agent（引用エージェント）がレポート内の全主張と引用元の整合性を検証する",
        "ユーザーが手動で各情報の出典を確認する設計にする"
      ],
      "correct": 2,
      "explanation": "Anthropicの公式マルチエージェントリサーチシステムでは、専用のCitation Agent（引用エージェント）がレポートと情報源を照合し、各主張に適切な引用が付けられているかを検証します。評価基準として「引用の正確性（citation accuracy）— 引用元と主張が一致しているか」が使われます。"
    },
    {
      "question": "マルチエージェントシステムでツールが失敗した場合のフォールバック設計として、Anthropicが推奨するアプローチはどれですか？",
      "options": [
        "エラーが発生したら即座にシステム全体を停止し、人間に通知する",
        "エラーを無視して次の処理に進む",
        "ツールの失敗をエージェントに構造化して伝え、エージェント自身に適応させる",
        "全てのツール呼び出しを事前にシミュレーションし、失敗する可能性のあるものは実行しない"
      ],
      "correct": 2,
      "explanation": "Anthropicは「ツールが失敗していることをエージェントに伝え、エージェント自身に適応させる（Let the agent know when a tool is failing and let it adapt）」アプローチが驚くほどうまく機能すると報告しています。構造化されたエラー情報（is_error、エラー種類、推奨アクション）を渡すことで、エージェントが最適な代替手段を自ら選択できます。"
    },
    {
      "question": "Hub-and-Spokeパターンにおいて、サブエージェント（スポーク）同士の通信について正しい記述はどれですか？",
      "options": [
        "サブエージェント同士は自由にメッセージを交換でき、効率的に情報共有できる",
        "サブエージェント同士は直接通信できず、必ずリードエージェント（ハブ）を経由する",
        "サブエージェントは共有メモリを通じてのみ通信できる",
        "サブエージェント同士の通信は暗号化チャネルで自動的に保護される"
      ],
      "correct": 1,
      "explanation": "Hub-and-Spokeパターンでは、サブエージェント（兄弟サブエージェント）同士は直接通信できません（sibling subagents cannot communicate directly）。全ての通信はリードエージェント（ハブ）を経由します。空港のハブ空港のように、各都市間の直行便はなく、必ずハブを経由する構造です。"
    },
    {
      "question": "マルチエージェントシステムの観測可能性（Observability）について、Anthropicが推奨するアプローチはどれですか？",
      "options": [
        "全エージェントの会話内容を記録し、管理者が全文を確認する",
        "エージェントの判断パターンとやり取りの構造を監視し、個々の会話内容は監視しない",
        "最終出力のみを検査し、途中経過は一切監視しない",
        "外部の監視AIが全エージェントをリアルタイムで評価する"
      ],
      "correct": 1,
      "explanation": "Anthropicは「エージェントの判断パターンとやり取りの構造を監視する — ただし個々の会話の内容は監視しない（Monitor agent decision patterns and interaction structures — without monitoring the contents of individual conversations）」というプライバシーに配慮した観測アプローチを推奨しています。これにより、システムの健全性を保ちつつ、ユーザープライバシーを尊重できます。"
    }
  ]
}
```

---

## DOMAIN_MAP への追加

```json
{
  "m10_expansion": "d1"
}
```

全5問ともドメイン1（エージェント型アーキテクチャとオーケストレーション）に対応。

---

## 挿入手順メモ（Phase B 呂蒙向け）

1. **section-nav** に2つのタブボタン（s7, s8）を追加
2. **m10-s4の後** にセクション7（m10-s7）とセクション8（m10-s8）のHTMLを挿入
3. **m10-s5（用語集）** に7つの用語項目を追加
4. **QUIZ_DATA.m10** 配列に5問を追加（既存5問の後ろ）
5. **DOMAIN_MAP** にm10追加分のマッピングを追加
