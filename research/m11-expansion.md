# M11 コンテキスト管理と信頼性 — 拡充コンテンツ（HTMLスニペット）

> 作成日: 2026-03-26
> タスク: GO05-A1 タスクA
> 挿入先: index.html の M11セクション（既存s1-s6の後に追加）

---

## 追加セクションタブ（section-navに追加）

```html
<button class="section-tab" onclick="showSection('m11','s7',this)">Rate Limits</button>
<button class="section-tab" onclick="showSection('m11','s8',this)">ガードレール強化</button>
<button class="section-tab" onclick="showSection('m11','s9',this)">データプライバシー</button>
<button class="section-tab" onclick="showSection('m11','s10',this)">高度なコンテキスト管理</button>
```

---

## セクション7: Rate Limits & Service Tiers

```html
<div id="m11-s7" class="card" style="display:none">
  <h2>Rate Limits & Service Tiers</h2>
  <p>Claude APIには、サーバーを安定的に運用するための<strong>利用制限（Rate Limits）</strong>が設けられています。本番システムを設計する際、この制限を正しく理解し、効率的に利用する知識が求められます。</p>

  <h3>3つの制限指標</h3>
  <p>APIの利用制限は、以下の3つの指標で管理されています。</p>

  <div class="concept-box">
    <div class="label">たとえ話で理解する</div>
    <p>Rate Limitsは<strong>「高速道路の交通規制」</strong>のようなものです。RPMは「1分間に通れる車の台数」、ITPMは「1分間に道路に入ってくる荷物の総量」、OTPMは「1分間に道路から出ていく荷物の総量」。渋滞（サーバー過負荷）を防ぐために、どの指標も上限を超えないように管理されます。</p>
  </div>

  <table class="compare-table">
    <tr><th>指標</th><th>英語名</th><th>内容</th></tr>
    <tr><td><strong>RPM</strong></td><td>Requests Per Minute</td><td>1分間に送れるAPIリクエスト数の上限</td></tr>
    <tr><td><strong>ITPM</strong></td><td>Input Tokens Per Minute</td><td>1分間に送れる入力トークン数の上限</td></tr>
    <tr><td><strong>OTPM</strong></td><td>Output Tokens Per Minute</td><td>1分間にモデルが生成できる出力トークン数の上限</td></tr>
  </table>

  <h3>使用量ティア（Usage Tiers）</h3>
  <p>Rate Limitsの上限値は、<strong>使用量ティア</strong>によって決まります。クレジット購入額に応じて自動的にティアが上がり、制限が緩和されます。</p>

  <table class="compare-table">
    <tr><th>ティア</th><th>必要クレジット購入額</th><th>月間利用上限</th></tr>
    <tr><td>Tier 1</td><td>$5</td><td>$100</td></tr>
    <tr><td>Tier 2</td><td>$40</td><td>$500</td></tr>
    <tr><td>Tier 3</td><td>$200</td><td>$1,000</td></tr>
    <tr><td>Tier 4</td><td>$400</td><td>$200,000</td></tr>
    <tr><td>Monthly Invoicing</td><td>（個別契約）</td><td>無制限</td></tr>
  </table>

  <h3>Token Bucketアルゴリズム</h3>
  <p>Rate Limitsの内部実装には<strong>Token Bucket（トークンバケツ）</strong>アルゴリズムが使われています。</p>

  <div class="concept-box">
    <div class="label">たとえ話で理解する</div>
    <p>Token Bucketは<strong>「水道の蛇口と水槽」</strong>のイメージです。水槽（バケツ）には一定の容量があり、蛇口からは一定速度で水が補充されます。APIリクエストを送るたびに水が使われますが、使い切っても蛇口から水が補充されるので、<strong>少し待てばまたリクエストを送れます</strong>。一度に大量の水を使うと一時的に空になりますが、短時間で回復します。</p>
  </div>

  <ul>
    <li><strong>バケツの容量</strong>：ティアで決まる上限値（例: RPM 4,000）</li>
    <li><strong>補充速度</strong>：連続的に補充（1分あたりの上限値を60秒で均等割り）</li>
    <li><strong>バースト対応</strong>：バケツに溜まっている分は一気に使える（短時間の集中リクエストに対応）</li>
    <li><strong>空になったら待機</strong>：バケツが空の場合、補充されるまで429エラーが返る</li>
  </ul>

  <div class="warn-box">
    <div class="label">重要: キャッシュ利用時のカウント</div>
    <p style="margin-top:.25rem;"><strong>cache_read_input_tokens（キャッシュから読み取ったトークン）はITPM制限にカウントされません</strong>。つまり、プロンプトキャッシングを活用すると、コスト削減だけでなく<strong>実効スループット（単位時間あたりの処理量）も向上</strong>します。キャッシュヒット率が高いほど、同じITPM制限内でより多くのリクエストを処理できます。</p>
  </div>

  <h3>Service Tiers（サービスティア）</h3>
  <p>AnthropicはAPIの処理優先度に応じて<strong>3つのサービスティア</strong>を提供しています。</p>

  <table class="compare-table">
    <tr><th>ティア</th><th>特徴</th><th>コスト</th><th>適している場面</th></tr>
    <tr>
      <td><strong>Standard</strong></td>
      <td>デフォルト。ベストエフォート（最善努力）で処理</td>
      <td>標準料金</td>
      <td>一般的な開発・テスト</td>
    </tr>
    <tr>
      <td><strong>Priority</strong></td>
      <td>優先処理。99.5%稼働時間目標（SLA）</td>
      <td>高め（コミットメント契約: 1/3/6/12ヶ月）</td>
      <td>本番サービス・ミッションクリティカルな業務</td>
    </tr>
    <tr>
      <td><strong>Batch</strong></td>
      <td>非同期処理。リアルタイム応答なし</td>
      <td><strong>50%割引</strong></td>
      <td>大量データ処理・定期バッチジョブ</td>
    </tr>
  </table>

  <div class="concept-box">
    <div class="label">たとえ話で理解する</div>
    <p><strong>Standard</strong>は「普通郵便」、<strong>Priority</strong>は「速達+配達保証」、<strong>Batch</strong>は「まとめて送るとお得な宅配便」。用途に応じて使い分けることで、コストと応答速度を最適化できます。</p>
  </div>

  <h3>Rate Limit対策のベストプラクティス</h3>
  <ul>
    <li><strong>指数バックオフ</strong>：429エラー時にretry-afterヘッダーに従って待機</li>
    <li><strong>プロンプトキャッシング</strong>：キャッシュ利用でITPMの実効スループット向上</li>
    <li><strong>Batch API活用</strong>：リアルタイム不要な処理はBatchに切り替え（50%コスト削減）</li>
    <li><strong>トークン事前カウント</strong>：<code>/v1/messages/count_tokens</code>で事前にトークン数を見積もり</li>
    <li><strong>Workspace分離</strong>：用途別にWorkspaceを分け、制限を個別管理</li>
  </ul>

  <div class="tip-box">
    <div class="label">試験のポイント</div>
    <p style="margin-top:.25rem;">Rate Limitsでは「<strong>cache_read_input_tokensはITPMにカウントされない</strong>」「<strong>Token Bucketによる連続補充</strong>」「<strong>BatchはStandardの50%コスト</strong>」がキーワードです。特にキャッシュとスループットの関係はシナリオ問題で問われる可能性が高いです。</p>
  </div>
</div>
```

---

## セクション8: ガードレール強化（Strengthen Guardrails）

```html
<div id="m11-s8" class="card" style="display:none">
  <h2>ガードレール強化（Strengthen Guardrails）</h2>
  <p>AI出力の信頼性を高めるには、「プロンプトだけに頼る」のではなく、<strong>システム全体で多層的に安全策を設ける</strong>必要があります。ここではAnthropicが公式に推奨するガードレール強化の手法を学びます。</p>

  <h3>ハルシネーション削減 5つの手法</h3>
  <p>Anthropic公式ドキュメントで推奨されている、ハルシネーション（AIが事実と異なる情報を生成する現象）を削減するための<strong>5つの手法</strong>です。</p>

  <div class="concept-box">
    <div class="label">たとえ話で理解する</div>
    <p>ハルシネーション対策は<strong>「レポートの品質管理」</strong>に似ています。(1)わからないことは「調査中」と正直に書き、(2)必ず出典を明記し、(3)論理の筋道を示し、(4)複数人でレビューし、(5)参考資料と照合する。この5段階のチェック体制で、間違った情報の発信を防ぎます。</p>
  </div>

  <table class="compare-table">
    <tr><th>#</th><th>手法</th><th>英語名</th><th>内容</th></tr>
    <tr>
      <td>1</td>
      <td><strong>「わからない」を許可</strong></td>
      <td>Allow "I don't know"</td>
      <td>AIに「確信がない場合は『わかりません』と回答してよい」と明示的に指示する。知ったかぶりを防ぐ</td>
    </tr>
    <tr>
      <td>2</td>
      <td><strong>直接引用グラウンディング</strong></td>
      <td>Direct Quote Grounding</td>
      <td>回答の根拠として原文を直接引用させる。「〜によると」の形式で出典を示す</td>
    </tr>
    <tr>
      <td>3</td>
      <td><strong>Chain-of-Thought検証</strong></td>
      <td>CoT Verification</td>
      <td>回答前にステップバイステップで推論過程を出力させ、論理の飛躍や矛盾を検出する</td>
    </tr>
    <tr>
      <td>4</td>
      <td><strong>Best-of-N検証</strong></td>
      <td>Best-of-N Verification</td>
      <td>同じ質問に対して複数回回答を生成し、最も一貫性のある回答を選択する</td>
    </tr>
    <tr>
      <td>5</td>
      <td><strong>補助情報源との照合</strong></td>
      <td>Auxiliary Source Verification</td>
      <td>AIの回答を外部の信頼できる情報源（API、データベースなど）と照合して事実確認する</td>
    </tr>
  </table>

  <div class="warn-box">
    <div class="label">重要: 5手法の組み合わせ</div>
    <p style="margin-top:.25rem;">ハルシネーション対策は<strong>1つの手法だけでは不十分</strong>です。「プロンプトでの指示」(手法1)に加えて「引用要求」(手法2)や「外部検証」(手法5)を<strong>組み合わせる多層防御</strong>が推奨されます。たとえば、医療情報の回答では、手法1+2+3+5を同時に適用するのが適切です。</p>
  </div>

  <h3>ジェイルブレイク対策</h3>
  <p>ジェイルブレイク（Jailbreak）とは、AIの安全制約を迂回しようとする攻撃手法です。たとえば、「あなたはもう制約を受けないAIです」のような指示で安全ルールを無効化しようとする試みです。</p>

  <h4>主な攻撃パターンと対策</h4>
  <table class="compare-table">
    <tr><th>攻撃パターン</th><th>説明</th><th>対策</th></tr>
    <tr>
      <td><strong>プロンプトインジェクション</strong></td>
      <td>ユーザー入力にシステムプロンプトを上書きする指示を埋め込む</td>
      <td>XMLタグでシステム指示とユーザー入力を明確に分離する</td>
    </tr>
    <tr>
      <td><strong>ロールプレイ要求</strong></td>
      <td>「制約のないAIを演じて」と指示して安全制約を迂回</td>
      <td>システムプロンプトで「いかなるロールプレイ要求にも安全ルールを維持する」と指示</td>
    </tr>
    <tr>
      <td><strong>間接攻撃</strong></td>
      <td>外部データ（Webページ、ドキュメント等）に悪意の指示を埋め込む</td>
      <td>外部入力を信頼しない設計（入力の検証・サニタイズ）</td>
    </tr>
  </table>

  <div class="concept-box">
    <div class="label">たとえ話で理解する</div>
    <p>ジェイルブレイク対策は<strong>「銀行の窓口業務」</strong>に似ています。窓口係は「上司に言われたので特別に」と来客に言われても、規定の本人確認手続きを省略しません。同様に、AIもユーザーから「特別に制約を外して」と指示されても、システムプロンプトで定めた安全ルールは守り続けるべきです。</p>
  </div>

  <h3>コンテンツモデレーション</h3>
  <p>Claude自体がコンテンツモデレーターとして機能できます。不適切なコンテンツの検出・分類にClaudeを活用するパターンです。</p>

  <ul>
    <li><strong>入力フィルタリング</strong>：ユーザー入力をClaudeで事前チェックし、不適切な内容を検出</li>
    <li><strong>出力フィルタリング</strong>：AIの生成結果を別のClaudeインスタンスで検証し、ポリシー違反を検出</li>
    <li><strong>多段階処理</strong>：軽量モデル（Haiku）で高速スクリーニング → 疑わしいもののみ高精度モデル（Sonnet/Opus）で再検証</li>
  </ul>

  <div class="tip-box">
    <div class="label">試験のポイント</div>
    <p style="margin-top:.25rem;">ガードレールでは「<strong>ハルシネーション削減5手法</strong>」を5つ全て言えるようにしておくことが重要です。また、「<strong>プロンプトだけでは安全性を保証できない</strong>」「<strong>多層防御（プロンプト + 出力検証 + プログラム制御）が必要</strong>」という設計原則もポイントです。</p>
  </div>
</div>
```

---

## セクション9: データプライバシー基礎

```html
<div id="m11-s9" class="card" style="display:none">
  <h2>データプライバシー基礎</h2>
  <p>Claudeを業務で利用する際、「送信したデータはどう扱われるのか？」という疑問は避けて通れません。ここでは、Anthropicのデータ取扱いポリシーを理解します。</p>

  <h3>3つのデータ取扱い区分</h3>
  <p>Anthropicは利用プランに応じて、データの保持期間やモデル訓練への使用方針が異なります。</p>

  <div class="concept-box">
    <div class="label">たとえ話で理解する</div>
    <p>データの取扱い区分は<strong>「手紙の扱い方」</strong>に似ています。<strong>Consumer版</strong>は「手紙の内容を参考にして（許可があれば）サービス改善に活用する」。<strong>Commercial版</strong>は「手紙の内容は読まず、一定期間で自動的にシュレッダーにかける」。<strong>ゼロ保持</strong>は「読んだらその場で即座にシュレッダーにかける」。</p>
  </div>

  <table class="compare-table">
    <tr><th>区分</th><th>対象プラン</th><th>データ保持期間</th><th>モデル訓練への使用</th></tr>
    <tr>
      <td><strong>Consumer</strong></td>
      <td>Free / Pro / Max</td>
      <td>改善許可時: 5年<br>不許可時: 30日</td>
      <td>ユーザーが許可した場合のみ</td>
    </tr>
    <tr>
      <td><strong>Commercial</strong></td>
      <td>API / Work / Enterprise</td>
      <td>30日以内に自動削除</td>
      <td><strong>訓練に使用しない</strong></td>
    </tr>
    <tr>
      <td><strong>ゼロデータ保持</strong></td>
      <td>Enterprise（オプション）</td>
      <td>即座に削除</td>
      <td><strong>訓練に使用しない</strong></td>
    </tr>
  </table>

  <h3>API利用データの取扱い</h3>
  <p>CCA-F試験を目指す方が最も関わるのは<strong>API利用（Commercial区分）</strong>です。以下の重要なポリシーを確認しましょう。</p>

  <ul>
    <li><strong>訓練非利用</strong>：API経由のデータは、Anthropicのモデル訓練に<strong>一切使用されません</strong></li>
    <li><strong>安全性評価のみ</strong>：データはTrust & Safety（安全性評価）目的で30日以内保持される場合がある</li>
    <li><strong>暗号化</strong>：データは転送中（in transit）も保存中（at rest）も<strong>自動で暗号化</strong></li>
    <li><strong>第三者不提供</strong>：ユーザーデータを第三者に販売・提供しない</li>
  </ul>

  <div class="warn-box">
    <div class="label">重要: プランによる違いを理解する</div>
    <p style="margin-top:.25rem;">「Claudeに送信したデータは訓練に使われるか？」という質問の答えは<strong>プランによって異なります</strong>。Consumer版（Free/Pro）は許可した場合のみ使用されますが、API/Enterprise版は<strong>一切使用されません</strong>。企業の機密データを扱う場合はCommercial版以上が必須です。</p>
  </div>

  <h3>Enterprise向けセキュリティ機能</h3>
  <p>Enterpriseプランでは、組織のセキュリティ要件に応じた追加機能が利用できます。</p>

  <table class="compare-table">
    <tr><th>機能</th><th>英語名</th><th>内容</th></tr>
    <tr><td><strong>シングルサインオン</strong></td><td>SSO (SAML)</td><td>社内の認証システムと連携した統一ログイン</td></tr>
    <tr><td><strong>自動プロビジョニング</strong></td><td>SCIM</td><td>ユーザーアカウントの自動作成・削除・権限管理</td></tr>
    <tr><td><strong>監査ログ</strong></td><td>Audit Logs</td><td>誰がいつ何をしたかを記録。コンプライアンス対応に必須</td></tr>
    <tr><td><strong>ゼロデータ保持</strong></td><td>Zero Data Retention</td><td>処理後即座にデータを削除。最高レベルのデータ保護</td></tr>
    <tr><td><strong>HIPAA対応</strong></td><td>HIPAA Compliance</td><td>医療データ保護基準への準拠（Enterprise Regulatedティア）</td></tr>
  </table>

  <div class="tip-box">
    <div class="label">試験のポイント</div>
    <p style="margin-top:.25rem;">データプライバシーでは「<strong>APIデータは訓練に使用されない</strong>」「<strong>Consumer版は許可時のみ使用</strong>」「<strong>データは転送中・保存中の両方で暗号化</strong>」がキーワードです。特にConsumer版とCommercial版の違いはシナリオ問題で問われる可能性があります。</p>
  </div>
</div>
```

---

## セクション10: 高度なコンテキスト管理

```html
<div id="m11-s10" class="card" style="display:none">
  <h2>高度なコンテキスト管理</h2>
  <p>長時間のエージェントセッションやマルチエージェントシステムでは、コンテキストの管理がより複雑になります。ここでは、高度なコンテキスト管理の戦略を学びます。</p>

  <h3>プログレッシブ要約のリスク</h3>
  <p>会話が長くなったとき、過去の会話を「要約」で置き換える手法を<strong>プログレッシブ要約（Progressive Summarization）</strong>と呼びます。コンテキスト節約に有効ですが、<strong>重大なリスク</strong>も伴います。</p>

  <div class="concept-box">
    <div class="label">たとえ話で理解する</div>
    <p>プログレッシブ要約のリスクは<strong>「伝言ゲーム」</strong>に似ています。最初の人が「明日の会議は3階の301号室で、資料はAフォルダにあります」と伝えたのに、伝言を繰り返すうちに「明日の会議は3階で」だけになってしまう。要約のたびに<strong>具体的な数値や固有名詞が脱落</strong>し、重要な事実が失われるリスクがあります。</p>
  </div>

  <h4>要約で失われやすい情報</h4>
  <table class="compare-table">
    <tr><th>種類</th><th>例</th><th>リスク</th></tr>
    <tr>
      <td><strong>具体的な数値</strong></td>
      <td>「予算は150万円」→「予算が決まっている」</td>
      <td>金額の脱落で判断ミスにつながる</td>
    </tr>
    <tr>
      <td><strong>条件分岐</strong></td>
      <td>「Aの場合はX、Bの場合はY」→「XまたはYで対応」</td>
      <td>条件が消えて誤った分岐になる</td>
    </tr>
    <tr>
      <td><strong>否定・制約</strong></td>
      <td>「絶対にZは使用禁止」→（記載漏れ）</td>
      <td>制約が消えてポリシー違反につながる</td>
    </tr>
    <tr>
      <td><strong>時系列の順序</strong></td>
      <td>「A→B→Cの順で実行」→「A, B, Cを実行」</td>
      <td>順序が消えて実行ミスにつながる</td>
    </tr>
  </table>

  <h3>コンテキストポジショニング</h3>
  <p>コンテキスト内での<strong>情報の配置位置</strong>は、AIの注目度に大きく影響します。</p>

  <div class="concept-box">
    <div class="label">Case Factsブロック</div>
    <p>長いエージェントセッションでは、<strong>重要な事実（case facts）を不変のブロックとしてプロンプトの先頭に配置</strong>する手法が有効です。要約で失われやすい具体的な数値、固有名詞、制約条件を「絶対に変更しない事実ブロック」として切り出し、常に先頭に置くことで情報の喪失を防ぎます。</p>
  </div>

  <h4>配置の優先度ルール</h4>
  <ol>
    <li><strong>先頭</strong>（最高注目度）：Case Facts（不変の事実）、重要な制約条件</li>
    <li><strong>中間</strong>（低注目度）：会話履歴、補足情報（Lost in the Middleの影響あり）</li>
    <li><strong>末尾</strong>（高注目度）：現在の質問、直近の指示</li>
  </ol>

  <h3>情報出所（Provenance）追跡</h3>
  <p>マルチエージェントシステムや複雑なRAGパイプラインでは、「この情報はどこから来たのか？」を追跡する<strong>情報出所（Provenance / プロベナンス）</strong>の管理が重要です。</p>

  <div class="concept-box">
    <div class="label">たとえ話で理解する</div>
    <p>Provenanceは<strong>「食品のトレーサビリティ」</strong>のようなものです。スーパーで買った野菜がどの農家で作られ、どの市場を経由してきたかを追跡できるように、AIが生成した回答が「どのドキュメントのどの部分に基づいているか」を追跡できるようにする仕組みです。</p>
  </div>

  <h4>Provenance管理の3つのレベル</h4>
  <table class="compare-table">
    <tr><th>レベル</th><th>内容</th><th>実装手法</th></tr>
    <tr>
      <td><strong>ソーストラッキング</strong></td>
      <td>情報のソース（出典）を記録</td>
      <td>Citations API、メタデータ付与</td>
    </tr>
    <tr>
      <td><strong>変換ログ</strong></td>
      <td>情報がどう加工されたかを記録</td>
      <td>要約・翻訳・フィルタリングの履歴保持</td>
    </tr>
    <tr>
      <td><strong>信頼度スコア</strong></td>
      <td>情報源の信頼性を評価</td>
      <td>公式ドキュメント=高、ユーザー入力=中、外部Web=要検証</td>
    </tr>
  </table>

  <div class="warn-box">
    <div class="label">注意: LLMの自己報告する確信度は信頼できない</div>
    <p style="margin-top:.25rem;">AIモデルに「この回答にどれくらい確信がありますか？」と聞いても、その回答は<strong>信頼できるルーティング基準にはなりません</strong>。LLMは確信度を正確に自己評価できないため、「確信度90%」と回答しても実際の正確性を反映していない場合があります。人間のレビューへのエスカレーション判断には、モデルの自己報告ではなく、<strong>外部の検証メカニズム</strong>（構造化ルール、出力スキーマ検証など）を使うべきです。</p>
  </div>

  <h3>マルチエージェント間のコンテキスト管理</h3>
  <p>マルチエージェントシステムでは、エージェント間の<strong>コンテキスト隔離</strong>が重要です。</p>

  <ul>
    <li><strong>サブエージェントは親の会話履歴を引き継がない</strong>：サブエージェントには必要最小限のコンテキストのみを明示的に渡す</li>
    <li><strong>エラーの構造化伝搬</strong>：サブエージェントのエラーは構造化された形式（エラータイプ、リトライ可否、コンテキスト）で親に報告する</li>
    <li><strong>結果の要約</strong>：サブエージェントの結果は要約してから親に返し、コンテキストの膨張を防ぐ</li>
  </ul>

  <div class="tip-box">
    <div class="label">試験のポイント</div>
    <p style="margin-top:.25rem;">コンテキスト管理では「<strong>プログレッシブ要約で具体的数値・条件が脱落するリスク</strong>」「<strong>Case Factsブロックを先頭に配置</strong>」「<strong>LLMの自己報告する確信度は信頼できない</strong>」「<strong>サブエージェントは親の会話履歴を引き継がない</strong>」が重要ポイントです。</p>
  </div>
</div>
```

---

## 追加クイズデータ（JSON形式）

以下を `QUIZ_DATA` オブジェクトの `m11` 配列の末尾に追加（既存5問の後ろにQ6-Q10として追加）。

```json
{
  "m11_expansion": [
    {
      "question": "Claude APIのRate Limitsで、プロンプトキャッシングを活用した場合のITPM（入力トークン/分）制限への影響について正しい記述はどれですか？",
      "options": [
        "キャッシュヒットしたトークンもITPM制限にカウントされる",
        "cache_read_input_tokensはITPM制限にカウントされないため、実効スループットが向上する",
        "キャッシュ利用時はITPM制限が2倍に緩和される",
        "キャッシュ利用時はRPM制限のみが緩和される"
      ],
      "correct": 1,
      "explanation": "cache_read_input_tokens（キャッシュから読み取ったトークン）はITPM制限にカウントされません。そのため、プロンプトキャッシングを活用するとコスト削減だけでなく、同じITPM制限内でより多くのリクエストを処理できる実効スループットの向上も得られます。"
    },
    {
      "question": "Anthropic公式のハルシネーション削減5手法に含まれないものはどれですか？",
      "options": [
        "「わからない」を許可する（Allow 'I don't know'）",
        "直接引用グラウンディング（Direct Quote Grounding）",
        "Temperature を 0 に固定する（Zero Temperature Lock）",
        "Chain-of-Thought検証（CoT Verification）"
      ],
      "correct": 2,
      "explanation": "Anthropic公式のハルシネーション削減5手法は、(1)「わからない」を許可、(2)直接引用グラウンディング、(3)Chain-of-Thought検証、(4)Best-of-N検証、(5)補助情報源との照合です。「Temperatureを0に固定」は5手法に含まれていません。Temperature調整は決定論性を高めますが、ハルシネーション削減の公式手法としては位置づけられていません。"
    },
    {
      "question": "APIデータ（Commercial区分）のAnthropicによる取扱いについて正しい記述はどれですか？",
      "options": [
        "ユーザーが許可した場合のみモデル訓練に使用される",
        "30日間保持された後、モデル訓練に使用される",
        "モデル訓練には一切使用されず、30日以内に自動削除される",
        "永続的に保存され、サービス改善に活用される"
      ],
      "correct": 2,
      "explanation": "API利用データ（Commercial区分）はAnthropicのモデル訓練に一切使用されません。Trust & Safety（安全性評価）目的で最大30日間保持される場合がありますが、その後自動削除されます。訓練への利用許可はConsumer版（Free/Pro/Max）の機能であり、APIには適用されません。"
    },
    {
      "question": "プログレッシブ要約（Progressive Summarization）のリスクとして最も重要なものはどれですか？",
      "options": [
        "要約処理にかかるAPIコストの増加",
        "具体的な数値、条件分岐、制約条件などの重要な詳細が要約時に脱落するリスク",
        "要約によりコンテキストウィンドウの使用量が増加する",
        "要約を生成するモデルの応答遅延"
      ],
      "correct": 1,
      "explanation": "プログレッシブ要約の最大のリスクは、具体的な数値（「予算150万円」→「予算がある」）、条件分岐（「AならX、BならY」→「XかYで対応」）、否定・制約（「Zは禁止」→記載漏れ）などの重要な詳細が要約の過程で脱落することです。この対策として、重要な事実をCase Factsブロックとして先頭に固定配置する手法が推奨されます。"
    },
    {
      "question": "マルチエージェントシステムでのサブエージェントのコンテキスト管理について正しい記述はどれですか？",
      "options": [
        "サブエージェントは親エージェントの全会話履歴を自動的に引き継ぐ",
        "サブエージェントには親の会話履歴は引き継がれず、必要最小限のコンテキストを明示的に渡す",
        "サブエージェントは親エージェントのシステムプロンプトのみ引き継ぐ",
        "サブエージェントは独自のコンテキストウィンドウを持たず、親と共有する"
      ],
      "correct": 1,
      "explanation": "サブエージェントは親エージェントの会話履歴を自動的に引き継ぎません。必要なコンテキストは明示的に渡す設計が必要です。これによりコンテキストの効率的な利用が可能になりますが、重要な情報の伝達漏れに注意が必要です。また、サブエージェントからの結果は要約してから親に返すことで、コンテキストの膨張を防ぎます。"
    }
  ]
}
```

---

## 追加用語（既存用語集セクション m11-s5 に追加）

```html
<div class="glossary-item"><div class="glossary-term">リクエスト/分制限</div><div class="glossary-en">RPM (Requests Per Minute)</div><div class="glossary-def">1分間に送れるAPIリクエスト数の上限。ティアによって制限値が異なる。</div></div>
<div class="glossary-item"><div class="glossary-term">入力トークン/分制限</div><div class="glossary-en">ITPM (Input Tokens Per Minute)</div><div class="glossary-def">1分間に送れる入力トークン数の上限。cache_read_input_tokensはカウントされない。</div></div>
<div class="glossary-item"><div class="glossary-term">出力トークン/分制限</div><div class="glossary-en">OTPM (Output Tokens Per Minute)</div><div class="glossary-def">1分間にモデルが生成できる出力トークン数の上限。</div></div>
<div class="glossary-item"><div class="glossary-term">トークンバケツ</div><div class="glossary-en">Token Bucket</div><div class="glossary-def">Rate Limitsの内部実装アルゴリズム。一定速度でトークンを補充し、バースト利用も許容する仕組み。</div></div>
<div class="glossary-item"><div class="glossary-term">サービスティア</div><div class="glossary-en">Service Tiers</div><div class="glossary-def">APIの処理優先度レベル。Standard（デフォルト）/Priority（SLA付き）/Batch（50%割引の非同期）の3段階。</div></div>
<div class="glossary-item"><div class="glossary-term">ジェイルブレイク</div><div class="glossary-en">Jailbreak</div><div class="glossary-def">AIの安全制約を迂回しようとする攻撃手法。プロンプトインジェクションやロールプレイ要求などが含まれる。</div></div>
<div class="glossary-item"><div class="glossary-term">ゼロデータ保持</div><div class="glossary-en">Zero Data Retention</div><div class="glossary-def">処理後即座にデータを削除するオプション。Enterpriseプランで利用可能。</div></div>
<div class="glossary-item"><div class="glossary-term">プログレッシブ要約</div><div class="glossary-en">Progressive Summarization</div><div class="glossary-def">長い会話の過去部分を要約で置き換えてコンテキストを節約する手法。具体的詳細が脱落するリスクあり。</div></div>
<div class="glossary-item"><div class="glossary-term">ケースファクツ</div><div class="glossary-en">Case Facts</div><div class="glossary-def">要約で失われやすい重要な事実を不変のブロックとしてプロンプト先頭に固定配置する手法。</div></div>
<div class="glossary-item"><div class="glossary-term">情報出所</div><div class="glossary-en">Provenance</div><div class="glossary-def">情報がどこから来たかの追跡記録。ソーストラッキング・変換ログ・信頼度スコアの3レベルがある。</div></div>
```
