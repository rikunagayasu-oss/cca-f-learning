# M5 拡充コンテンツ — ツール設計の高度なトピック

> 作成日: 2026-03-26
> Phase A: HTMLスニペット（Phase Bでindex.htmlに統合）

## 統合ガイド（Phase B用メモ）

既存M5は s1-s6（s5=用語集, s6=クイズ）。以下の新セクション5つを s4 の後、用語集の前に挿入する。
挿入後のセクション構成:
- s1: Tool Useとは（既存）
- s2: ツール定義の設計（既存）
- s3: 実行フローと制御（既存）
- s4: エラー処理と注意点（既存）
- **s5: ツール説明文のLLM最適化（新規）**
- **s6: 構造化エラーハンドリング（新規）**
- **s7: ツール分配と管理（新規）**
- **s8: 推論オーバーロード防止（新規）**
- **s9: tool_choiceの高度な使い方（新規）**
- s10: 用語集（既存s5 → リナンバリング + 新用語追加）
- s11: クイズ（既存s6 → リナンバリング + 新規5問追加）

セクションタブに追加するボタン（既存の「エラー処理と注意点」と「用語集」の間に挿入）:
```html
<button class="section-tab" onclick="showSection('m5','s5',this)">説明文のLLM最適化</button>
<button class="section-tab" onclick="showSection('m5','s6',this)">構造化エラー設計</button>
<button class="section-tab" onclick="showSection('m5','s7',this)">ツール分配と管理</button>
<button class="section-tab" onclick="showSection('m5','s8',this)">推論オーバーロード防止</button>
<button class="section-tab" onclick="showSection('m5','s9',this)">tool_choice応用</button>
```

---

## 新規セクション HTML

### セクション5: ツール説明文のLLM最適化

```html
<!-- Section 5: ツール説明文のLLM最適化 -->
<div id="m5-s5" class="card" style="display:none">
  <h2>ツール説明文（description）のLLM最適化</h2>
  <p>Module 5の前半で「descriptionがツール選択の最大の判断材料」と学びました。ここでは一歩進んで、<strong>LLMの視点から最適な説明文をどう書くか</strong>を深掘りします。</p>

  <h3>Claudeはdescriptionをどう「読んで」いるか</h3>
  <p>Claudeがツールを選ぶ仕組みを、<strong>図書館の司書</strong>にたとえてみましょう。ユーザーが「最新の売上データを見たい」と言ったとき、Claudeは手元にある全ツールの説明文を一通り「読んで」、最も適切なツールを選びます。</p>
  <p>このとき、Claudeは以下の順序で判断しています。</p>
  <ol>
    <li><strong>ツール名（name）</strong>で大まかな候補を絞る — <code>get_sales_data</code>は候補に入る、<code>send_email</code>は外す</li>
    <li><strong>説明文（description）</strong>で最終判断する — 「月次売上データ」なのか「リアルタイム売上」なのか</li>
    <li><strong>パラメータ定義</strong>で引数を組み立てる — 必要な情報（期間、部門など）を判断</li>
  </ol>
  <p>つまり、descriptionは<strong>ツール選択の「決定打」</strong>です。名前が同じでも説明が曖昧なら選ばれません。</p>

  <div class="concept-box">
    <div class="label">重要概念</div>
    <p><strong>LLM最適化されたツール説明文（LLM-Optimized Tool Description）：</strong>AIモデルがツールを正確に選択できるよう、「何をするか」「いつ使うか」「いつ使わないか」「戻り値は何か」を明確に記述した説明文。人間向けのドキュメントとは異なり、<strong>AIの判断プロセスに合わせて書く</strong>ことがポイント。</p>
  </div>

  <h3>良い説明文の5つの要素</h3>
  <p>優れたツール説明文には、以下の5つの要素が含まれています。</p>

  <table class="compare-table">
    <tr><th>#</th><th>要素</th><th>説明</th><th>例</th></tr>
    <tr><td>1</td><td>機能の概要</td><td>ツールが何をするかの1文要約</td><td>「顧客IDから過去の注文履歴を取得する」</td></tr>
    <tr><td>2</td><td>使用条件</td><td>いつこのツールを使うべきか</td><td>「ユーザーが注文状況や購入履歴を質問した場合に使用」</td></tr>
    <tr><td>3</td><td>非使用条件</td><td>いつこのツールを使うべきでないか</td><td>「商品の在庫確認にはcheck_inventoryを使うこと」</td></tr>
    <tr><td>4</td><td>戻り値の説明</td><td>何が返ってくるか</td><td>「過去90日分の注文を新しい順で最大50件返す」</td></tr>
    <tr><td>5</td><td>制約・注意事項</td><td>使用時の制限やルール</td><td>「顧客IDが不明な場合はまずsearch_customerで検索すること」</td></tr>
  </table>

  <h3>悪い説明文 vs 良い説明文 — 実例比較</h3>
  <p>同じ「顧客検索」ツールでも、説明文の質で選択精度は大きく変わります。</p>

  <table class="compare-table">
    <tr><th>観点</th><th>悪い例</th><th>良い例</th></tr>
    <tr>
      <td>全体</td>
      <td><code>"description": "顧客を検索する"</code></td>
      <td><code>"description": "顧客名またはメールアドレスで顧客レコードを検索する。部分一致で最大20件を返す。顧客IDが既知の場合はget_customer_by_idを使うこと。"</code></td>
    </tr>
    <tr>
      <td>問題点 / 改善点</td>
      <td>何で検索するのか、何が返るのか、他ツールとの使い分けが不明</td>
      <td>検索キー、返却件数、他ツールとの使い分けが明確</td>
    </tr>
  </table>

  <div class="tip-box">
    <div class="label">試験のポイント</div>
    <p style="margin-top:.25rem;">CCA-F試験のシナリオ問題では「複数のツールから適切なツールを選択するためのベストプラクティス」が問われます。正解は「descriptionに使用条件と非使用条件の両方を記載する」パターンです。descriptionは「人間向けのヘルプ文」ではなく「AIの判断材料」として書くという視点を持ちましょう。</p>
  </div>

  <h3>description最適化チェックリスト</h3>
  <p>ツールのdescriptionを書いたら、以下の項目を確認しましょう。</p>
  <ul>
    <li>1文目で「何をするツールか」が明確に分かるか</li>
    <li>似た機能の他ツールとの違い・使い分けが明記されているか</li>
    <li>戻り値の形式や件数制限が記載されているか</li>
    <li>前提条件（「先に別のツールで○○を取得すること」）が明記されているか</li>
    <li>曖昧な表現（「データを処理する」「情報を取得する」）を避けているか</li>
  </ul>

  <div class="warn-box">
    <div class="label">注意</div>
    <p style="margin-top:.25rem;">descriptionが長すぎるのも問題です。数百文字を超えるとClaudeの判断が遅くなり、コンテキストウィンドウも消費します。<strong>簡潔かつ明確に</strong>、目安として2-3文で書くのがベストです。</p>
  </div>
</div>
```

### セクション6: 構造化エラーハンドリング

```html
<!-- Section 6: 構造化エラーハンドリング -->
<div id="m5-s6" class="card" style="display:none">
  <h2>構造化エラーハンドリング（isError / isRetryable）</h2>
  <p>前半のセクションで<code>is_error</code>フラグの基本を学びました。ここでは、本番環境で重要になる<strong>構造化されたエラーレスポンス</strong>の設計を深掘りします。</p>

  <h3>なぜ「構造化」が必要なのか</h3>
  <p>病院の診察にたとえると、「具合が悪い」と言うだけでは医師は判断できません。「熱が38.5度あり、昨日から咳が出ている」と伝えれば、適切な対応ができます。エラーレスポンスも同じで、<strong>何が、なぜ、どうすれば直るか</strong>を構造的に伝えることで、Claudeはより賢い対応ができます。</p>

  <div class="concept-box">
    <div class="label">重要概念</div>
    <p><strong>構造化エラーレスポンス（Structured Error Response）：</strong>エラーの種類・原因・リトライ可否を体系的に含むtool_result。単なるエラー文字列ではなく、Claudeが次のアクションを判断できる情報を提供する。</p>
  </div>

  <h3>エラーレスポンスの設計パターン</h3>
  <p>tool_resultに構造化されたエラー情報を含めることで、Claudeの判断を助けます。</p>

  <pre style="background:#F3F4F6;padding:1rem;border-radius:8px;font-size:.85rem;overflow-x:auto;margin:1rem 0;">
// tool_resultの内容として返すJSON
{
  "error": {
    "type": "invalid_parameter",
    "message": "都市名 'Toko' が見つかりません",
    "isRetryable": true,
    "suggestion": "正しい都市名を入力してください（例: Tokyo）"
  }
}</pre>

  <h3>isRetryable — リトライすべきかの判断</h3>
  <p>エラーには<strong>「やり直せるもの」と「やり直しても無駄なもの」</strong>の2種類があります。この区別をClaudeに伝えることが重要です。</p>

  <table class="compare-table">
    <tr><th>エラーの種類</th><th>isRetryable</th><th>Claudeの対応</th><th>例</th></tr>
    <tr><td>一時的なエラー</td><td><code>true</code></td><td>パラメータを修正して再試行</td><td>入力値のスペルミス、一時的なAPI障害、レート制限</td></tr>
    <tr><td>恒久的なエラー</td><td><code>false</code></td><td>再試行せず、ユーザーに状況を説明</td><td>権限不足、リソースが存在しない、サポート外の操作</td></tr>
  </table>

  <p>自動販売機にたとえると、<strong>「お釣りが足りません」</strong>はリトライ可能（少額の商品を選べばよい）ですが、<strong>「この商品は販売終了です」</strong>はリトライ不可（何回ボタンを押しても出てきません）です。</p>

  <h3>エラータイプの分類</h3>
  <p>よく使われるエラータイプを整理すると、以下のようになります。</p>

  <table class="compare-table">
    <tr><th>エラータイプ</th><th>意味</th><th>isRetryable</th><th>Claudeへの期待アクション</th></tr>
    <tr><td><code>invalid_parameter</code></td><td>パラメータの値が不正</td><td>true</td><td>値を修正して再試行</td></tr>
    <tr><td><code>missing_parameter</code></td><td>必要なパラメータが欠落</td><td>true</td><td>ユーザーに不足情報を確認</td></tr>
    <tr><td><code>rate_limited</code></td><td>APIの呼び出し上限に到達</td><td>true</td><td>時間をおいて再試行するか、代替手段を提案</td></tr>
    <tr><td><code>not_found</code></td><td>指定したリソースが存在しない</td><td>false</td><td>ユーザーに「見つかりません」と伝える</td></tr>
    <tr><td><code>permission_denied</code></td><td>アクセス権限がない</td><td>false</td><td>権限不足を説明し、管理者への相談を促す</td></tr>
    <tr><td><code>service_unavailable</code></td><td>外部サービスが停止中</td><td>true</td><td>一時的な問題と伝え、後でもう一度試すか代替手段を提案</td></tr>
  </table>

  <div class="tip-box">
    <div class="label">試験のポイント</div>
    <p style="margin-top:.25rem;">CCA-F試験では、「エラー時にClaudeに適切な対応を取らせるための設計」が問われます。<code>is_error: true</code>と具体的なエラーメッセージに加え、<strong>リトライ可否（isRetryable）</strong>の情報を含めることで、Claudeが「再試行すべきか」「ユーザーに報告すべきか」を正しく判断できます。</p>
  </div>

  <h3>エラーハンドリングのベストプラクティス</h3>
  <ul>
    <li><strong>エラーメッセージは具体的に</strong> — 「エラーが発生しました」ではなく「顧客ID 'ABC' は無効な形式です。数字6桁の形式を指定してください」</li>
    <li><strong>次のアクションを示す</strong> — Claudeが何をすべきかの「ヒント」を含める</li>
    <li><strong>リトライ可否を明示する</strong> — isRetryable情報でClaude の判断を補助する</li>
    <li><strong>エラーでも必ずtool_resultを返す</strong> — 無応答はAPIエラーの原因になる</li>
    <li><strong>機密情報をエラーに含めない</strong> — 内部のスタックトレースやDB接続文字列を返さない</li>
  </ul>

  <div class="warn-box">
    <div class="label">注意</div>
    <p style="margin-top:.25rem;">エラーメッセージに内部実装の詳細（データベースのテーブル名、APIキー、ファイルパスなど）を含めないでください。Claudeがその情報をユーザーに伝えてしまう可能性があります。エラーメッセージは「ユーザーに見せても問題ない情報」だけで構成しましょう。</p>
  </div>
</div>
```

### セクション7: ツール分配と管理

```html
<!-- Section 7: ツール分配と管理 -->
<div id="m5-s7" class="card" style="display:none">
  <h2>ツール分配（Tool Distribution）と管理</h2>
  <p>実際のアプリケーションでは、数十個以上のツールを扱うことが珍しくありません。このとき、すべてのツールを毎回Claudeに渡すのではなく、<strong>必要なツールだけを適切に配分する</strong>戦略が重要になります。</p>

  <h3>なぜツール分配が必要なのか</h3>
  <p>レストランのメニューを想像してください。100ページのメニューを渡されたら、注文を決めるのに時間がかかりますよね。Claudeも同じで、渡されるツールが多すぎると、<strong>選択に迷い、応答速度が低下し、間違ったツールを選びやすくなります</strong>。</p>

  <div class="concept-box">
    <div class="label">重要概念</div>
    <p><strong>ツール分配（Tool Distribution）：</strong>大量のツールを効率的に管理するために、リクエストの文脈に応じてClaudeに渡すツールのセットを動的に制御する設計パターン。</p>
  </div>

  <h3>ツール分配の3つの戦略</h3>

  <table class="compare-table">
    <tr><th>戦略</th><th>仕組み</th><th>適用場面</th><th>たとえ</th></tr>
    <tr>
      <td><strong>カテゴリ別グルーピング</strong></td>
      <td>ツールを機能カテゴリに分類し、会話の文脈に応じて該当カテゴリのツールだけを渡す</td>
      <td>多機能なカスタマーサポートボット</td>
      <td>百貨店の各フロア案内 — お客さんが「靴を探している」と言ったら靴売り場のフロアだけ案内する</td>
    </tr>
    <tr>
      <td><strong>段階的開示</strong></td>
      <td>最初は基本ツールのみ渡し、必要に応じて高度なツールを追加する</td>
      <td>初心者と上級者が混在するシステム</td>
      <td>ゲームの武器解放 — 基本操作をマスターしたら新しい武器が使えるようになる</td>
    </tr>
    <tr>
      <td><strong>ルーティング型</strong></td>
      <td>最初のリクエストでユーザーの意図を判別し、以降はその意図に特化したツールセットを渡す</td>
      <td>複数業務を扱う統合アシスタント</td>
      <td>病院の受付 — まず症状を聞いて、適切な診療科に案内する</td>
    </tr>
  </table>

  <h3>カテゴリ別グルーピングの実践例</h3>
  <p>たとえば、ECサイトのサポートボットが30個のツールを持つ場合。</p>

  <table class="compare-table">
    <tr><th>カテゴリ</th><th>含まれるツール例</th><th>渡すタイミング</th></tr>
    <tr><td>注文管理</td><td>search_orders, get_order_status, cancel_order, modify_order</td><td>ユーザーが注文について質問したとき</td></tr>
    <tr><td>商品検索</td><td>search_products, get_product_details, check_inventory</td><td>ユーザーが商品を探しているとき</td></tr>
    <tr><td>アカウント</td><td>get_profile, update_address, reset_password</td><td>ユーザーがアカウント情報に関して質問したとき</td></tr>
    <tr><td>決済</td><td>get_payment_methods, process_refund, apply_coupon</td><td>支払いや返金の話題が出たとき</td></tr>
  </table>

  <p>会話の流れに応じて適切なカテゴリのツールだけを渡すことで、Claudeの選択精度を高く保てます。</p>

  <div class="tip-box">
    <div class="label">試験のポイント</div>
    <p style="margin-top:.25rem;">CCA-F試験のシナリオ問題（特にカスタマーサポートシナリオ）では、「大量のツールを効率的に管理する方法」が問われます。正解は「文脈に応じてツールセットを動的にフィルタリングする」アプローチです。全ツールを常に渡すのはアンチパターンであることを覚えておきましょう。</p>
  </div>

  <h3>ツール分配における注意点</h3>
  <ul>
    <li><strong>必要なツールを渡し忘れない</strong> — フィルタリングが厳しすぎると、必要なツールが欠落して機能しなくなる</li>
    <li><strong>カテゴリ横断のツール</strong> — 「ユーザーへのメッセージ送信」のように複数カテゴリで使うツールは共通セットに含める</li>
    <li><strong>tools配列の順序</strong> — 重要なツールを先頭に置くと、Claudeの注目を集めやすくなる</li>
  </ul>

  <div class="warn-box">
    <div class="label">注意</div>
    <p style="margin-top:.25rem;">ツール分配の仕組みはクライアント側（開発者のコード）で実装します。Claude自身が「今はこのツールだけ使おう」と判断するのではなく、開発者がリクエストごとに<code>tools</code>パラメータの内容を制御します。</p>
  </div>
</div>
```

### セクション8: 推論オーバーロード防止

```html
<!-- Section 8: 推論オーバーロード防止 -->
<div id="m5-s8" class="card" style="display:none">
  <h2>推論オーバーロード防止</h2>
  <p>ツールの数が増えすぎると、Claudeのパフォーマンスが劣化する現象を<strong>推論オーバーロード（Inference Overload）</strong>と呼びます。レストランのメニューが多すぎると注文に悩むのと同じ原理です。</p>

  <div class="concept-box">
    <div class="label">重要概念</div>
    <p><strong>推論オーバーロード（Inference Overload）：</strong>Claudeに渡すツールが多すぎることで発生するパフォーマンス劣化。具体的には、(1) ツール選択の精度が低下する、(2) 応答速度が遅くなる、(3) コンテキストウィンドウが圧迫される、という3つの問題が起きる。</p>
  </div>

  <h3>なぜオーバーロードが起きるのか</h3>
  <p>Claudeに渡すツール定義は、すべてコンテキストウィンドウ（会話の「作業スペース」）を消費します。ツールが10個なら数百トークンで済みますが、50個になると数千トークンに達し、<strong>本来の会話やユーザーの質問に使えるスペースが減ります</strong>。</p>
  <p>また、選択肢が多いほどClaudeの「迷い」が増えます。テストで4択なら正答率が高いですが、20択になると間違えやすくなるのと同じです。</p>

  <h3>オーバーロードの影響</h3>

  <table class="compare-table">
    <tr><th>影響</th><th>詳細</th><th>ユーザーへの影響</th></tr>
    <tr><td>選択精度の低下</td><td>似た機能のツールが多いとClaudeが誤ったツールを選ぶ</td><td>期待と違う結果が返ってくる</td></tr>
    <tr><td>応答速度の低下</td><td>ツール定義の処理にトークンを消費し、推論時間が増加</td><td>応答が遅くなる</td></tr>
    <tr><td>コンテキスト圧迫</td><td>ツール定義がコンテキストウィンドウを占有</td><td>長い会話で情報が欠落しやすくなる</td></tr>
    <tr><td>コスト増加</td><td>ツール定義の分だけ入力トークン数が増える</td><td>API利用料金が増加</td></tr>
  </table>

  <h3>オーバーロードを防ぐ5つの対策</h3>

  <table class="compare-table">
    <tr><th>#</th><th>対策</th><th>説明</th><th>効果</th></tr>
    <tr><td>1</td><td><strong>ツール数の削減</strong></td><td>本当に必要なツールだけを定義する。「あったら便利」程度のツールは省く</td><td>根本対策</td></tr>
    <tr><td>2</td><td><strong>ツールの統合</strong></td><td>似た機能のツールを1つに統合し、パラメータで切り替える</td><td>高い</td></tr>
    <tr><td>3</td><td><strong>動的フィルタリング</strong></td><td>前セクションのツール分配戦略で、文脈に応じてツールを絞り込む</td><td>高い</td></tr>
    <tr><td>4</td><td><strong>description の簡潔化</strong></td><td>冗長な説明文を簡潔にし、1ツールあたりのトークン消費を減らす</td><td>中程度</td></tr>
    <tr><td>5</td><td><strong>キャッシングの活用</strong></td><td>ツール定義にcache_controlを付与し、コストとレイテンシを削減</td><td>コスト面で有効</td></tr>
  </table>

  <h3>ツール統合の具体例</h3>
  <p>たとえば、顧客管理で以下の3つのツールがある場合。</p>
  <ul>
    <li><code>search_customer_by_name</code> — 名前で検索</li>
    <li><code>search_customer_by_email</code> — メールで検索</li>
    <li><code>search_customer_by_phone</code> — 電話番号で検索</li>
  </ul>
  <p>これらを1つの<code>search_customer</code>ツールに統合し、<code>search_type</code>パラメータ（enum: ["name", "email", "phone"]）で切り替える方が効率的です。3ツール→1ツールに削減でき、Claudeの選択も簡単になります。</p>

  <div class="tip-box">
    <div class="label">試験のポイント</div>
    <p style="margin-top:.25rem;">CCA-F試験では「ツール数が多い場合のパフォーマンス最適化」が問われます。正解のアプローチは(1)文脈に応じたツールのフィルタリング、(2)類似ツールの統合、(3)プロンプトキャッシングの活用です。「全ツールを常に渡す」は不正解です。</p>
  </div>

  <h3>推奨ツール数の目安</h3>
  <p>明確な上限値が公式に定義されているわけではありませんが、一般的には以下が目安です。</p>
  <ul>
    <li><strong>1リクエストあたり10-20個程度</strong> — この範囲なら選択精度は安定</li>
    <li><strong>20個を超える場合</strong> — ツール分配やカテゴリ分けの導入を検討</li>
    <li><strong>50個以上</strong> — パフォーマンスへの影響が顕著になるため、アーキテクチャの見直しが必要</li>
  </ul>

  <div class="warn-box">
    <div class="label">注意</div>
    <p style="margin-top:.25rem;">ツール数の削減は重要ですが、必要なツールまで削ってしまうと機能が損なわれます。「ツールを減らすこと」が目的ではなく、「Claudeが正確に選べる状態を保つこと」が本質です。プロンプトキャッシングはコスト削減に有効ですが、選択精度の改善には直接寄与しません。</p>
  </div>
</div>
```

### セクション9: tool_choiceの高度な使い方

```html
<!-- Section 9: tool_choiceの高度な使い方 -->
<div id="m5-s9" class="card" style="display:none">
  <h2>tool_choiceの高度な使い方</h2>
  <p>前半で<code>tool_choice</code>の4つのモード（auto / any / tool / none）を学びました。ここでは、これらを<strong>ワークフロー制御に応用する</strong>高度なパターンを見ていきます。</p>

  <h3>tool_choiceでワークフローを制御する</h3>
  <p>tool_choiceは単なるオプション設定ではなく、<strong>Claudeの行動を「プログラム的に」制御する強力な道具</strong>です。料理のレシピのように、「この段階ではこの作業をしなさい」と指定できます。</p>

  <div class="concept-box">
    <div class="label">重要概念</div>
    <p><strong>プログラム的エンフォースメント（Programmatic Enforcement）：</strong>tool_choiceやツール定義の設計によって、Claudeの行動を決定論的に（確実に）制御すること。プロンプトでの「お願い」ではなく、API設定での「強制」であり、100%確実に動作する。</p>
  </div>

  <h3>パターン1: 強制ツール実行</h3>
  <p><code>tool_choice: {"type": "tool", "name": "特定ツール名"}</code>を使うと、Claudeは<strong>必ずそのツールを呼び出します</strong>。これは「まず必ずこの情報を取得してから回答してほしい」という場面で使います。</p>
  <p>たとえば、カスタマーサポートで「最初に必ず顧客情報を確認する」というルールがある場合。</p>

  <table class="compare-table">
    <tr><th>アプローチ</th><th>方法</th><th>確実性</th></tr>
    <tr><td>プロンプトで指示</td><td>system promptに「必ずget_customer_infoを最初に呼んでください」</td><td>高いが100%ではない（Claudeが判断をスキップする可能性あり）</td></tr>
    <tr><td>tool_choiceで強制</td><td>最初のリクエストで<code>tool_choice: {"type": "tool", "name": "get_customer_info"}</code></td><td><strong>100%確実</strong>（APIレベルで強制）</td></tr>
  </table>

  <h3>パターン2: ステップバイステップのワークフロー</h3>
  <p>複数のステップを順番に実行する場合、各ステップで<code>tool_choice</code>を切り替えることで、確実な処理フローを構築できます。</p>

  <table class="compare-table">
    <tr><th>ステップ</th><th>tool_choice設定</th><th>目的</th></tr>
    <tr><td>Step 1</td><td><code>{"type": "tool", "name": "classify_intent"}</code></td><td>ユーザーの意図を必ず分類させる</td></tr>
    <tr><td>Step 2</td><td><code>{"type": "auto"}</code> + 分類結果に応じたツールセット</td><td>分類結果に基づいてClaudeが自律的にツールを選択</td></tr>
    <tr><td>Step 3</td><td><code>{"type": "tool", "name": "format_response"}</code></td><td>必ず所定のフォーマットで回答を生成させる</td></tr>
  </table>

  <div class="tip-box">
    <div class="label">試験のポイント</div>
    <p style="margin-top:.25rem;">CCA-F試験では「プロンプトベースのガイダンス」と「プログラム的エンフォースメント」の使い分けが頻出します。<strong>100%の確実性が必要な場面</strong>（データ検証、フォーマット統一、必須ステップの実行）ではtool_choiceの強制を使い、<strong>柔軟性が必要な場面</strong>（自由回答、複数の対応パターン）ではautoを使うのが正解パターンです。</p>
  </div>

  <h3>パターン3: ツール結果のバリデーション</h3>
  <p>ツール実行の結果を、別のツールで検証するパターンです。</p>
  <ol>
    <li>Claudeがデータ抽出ツールで情報を取得</li>
    <li>クライアント側で結果を受け取り、<code>tool_choice: {"type": "tool", "name": "validate_output"}</code>で検証ツールを強制呼び出し</li>
    <li>検証結果に問題があれば、修正を促す</li>
  </ol>
  <p>この「抽出→検証→修正」のループは、正確性が求められる業務（データ入力、書類作成など）で重要なパターンです。</p>

  <h3>tool_choice: noneの活用場面</h3>
  <p><code>none</code>は「ツールを渡しているが使わせない」モードです。一見矛盾しますが、以下のような場面で活躍します。</p>
  <ul>
    <li><strong>最終回答の生成</strong> — ツール結果を取得した後、最終的なテキスト回答だけを生成させたい場合</li>
    <li><strong>確認フェーズ</strong> — 「ツールを使わずに、ユーザーに確認を取ってから次に進む」という流れを強制する場合</li>
    <li><strong>テスト</strong> — ツールなしでClaudeがどう回答するかを確認する場合</li>
  </ul>

  <div class="warn-box">
    <div class="label">注意</div>
    <p style="margin-top:.25rem;">tool_choiceで特定ツールを強制した場合、Claudeは<strong>そのツールを呼ぶことしかできません</strong>。通常のテキスト回答は返せないため、ツール呼び出し後にtool_resultを返す処理が必ず必要です。ワークフローの最後のステップでは<code>auto</code>か<code>none</code>に戻すことを忘れないようにしましょう。</p>
  </div>
</div>
```

---

## 用語集に追加する項目

既存の用語集（s5 → リナンバリング後s10）に以下を追加:

```html
<div class="glossary-item">
  <div class="glossary-term">構造化エラーレスポンス</div>
  <div class="glossary-en">Structured Error Response</div>
  <div class="glossary-def">エラーの種類・原因・リトライ可否を体系的に含むtool_result。単なるエラー文字列ではなく、Claudeが次のアクションを判断できる情報を提供する設計パターン。</div>
</div>
<div class="glossary-item">
  <div class="glossary-term">リトライ可能フラグ</div>
  <div class="glossary-en">isRetryable</div>
  <div class="glossary-def">エラー発生時に再試行すべきか否かをClaudeに伝える指標。一時的なエラー（レート制限等）はtrue、恒久的なエラー（権限不足等）はfalseとする。</div>
</div>
<div class="glossary-item">
  <div class="glossary-term">ツール分配</div>
  <div class="glossary-en">Tool Distribution</div>
  <div class="glossary-def">多数のツールを持つシステムで、リクエストの文脈に応じてClaudeに渡すツールセットを動的に制御する設計パターン。</div>
</div>
<div class="glossary-item">
  <div class="glossary-term">推論オーバーロード</div>
  <div class="glossary-en">Inference Overload</div>
  <div class="glossary-def">Claudeに渡すツールが多すぎることで、選択精度の低下・応答速度の遅延・コンテキスト圧迫が発生する現象。</div>
</div>
<div class="glossary-item">
  <div class="glossary-term">プログラム的エンフォースメント</div>
  <div class="glossary-en">Programmatic Enforcement</div>
  <div class="glossary-def">tool_choiceやツール定義の設計によって、Claudeの行動をAPIレベルで確実に制御すること。プロンプトでの指示（ガイダンス）とは異なり、100%の確実性がある。</div>
</div>
```

---

## 確認クイズ 5問（JSON形式）

以下をQUIZ_DATAの既存M5クイズ配列に追加:

```json
{
  "question": "大規模なカスタマーサポートシステムで30個以上のツールを管理する場合、最も効果的なアプローチはどれですか？",
  "options": [
    "全ツールを常にClaudeに渡し、system promptで使い分けを指示する",
    "会話の文脈に応じて、関連するカテゴリのツールだけを動的にフィルタリングして渡す",
    "ツール数を5個以下に削減し、1つのツールに全機能を統合する",
    "ツール定義のdescriptionを空にして、ツール名だけで判断させる"
  ],
  "correct": 1,
  "explanation": "ツール分配（Tool Distribution）の基本は、文脈に応じた動的フィルタリングです。全ツールを常に渡すと推論オーバーロードが起き、1つに統合しすぎると柔軟性が失われます。カテゴリ別に分けて必要なツールだけを渡すのがベストプラクティスです。"
},
{
  "question": "ツール実行時にAPIのレート制限エラーが発生しました。tool_resultに含めるべき情報として最も適切な組み合わせはどれですか？",
  "options": [
    "is_error: true と「エラーが発生しました」というメッセージのみ",
    "is_error: false と正常な結果を装った空のレスポンス",
    "is_error: true、エラータイプ（rate_limited）、isRetryable: true、具体的な状況説明",
    "tool_resultを返さず、次のリクエストで再試行する"
  ],
  "correct": 2,
  "explanation": "構造化エラーレスポンスのベストプラクティスは、is_error: trueに加えて、エラータイプ・リトライ可否・具体的な状況説明を含めることです。レート制限は一時的なエラーなのでisRetryable: trueとし、Claudeが適切な対応（再試行や代替提案）を判断できるようにします。"
},
{
  "question": "ツール説明文（description）のLLM最適化において、最も重要な要素はどれですか？",
  "options": [
    "ツールの内部実装の詳細（使用しているプログラミング言語やライブラリ）",
    "ツールの作成日と作成者の情報",
    "「いつ使うべきか」と「いつ使うべきでないか」の両方の使用条件",
    "ツールの実行にかかる平均時間（ミリ秒単位）"
  ],
  "correct": 2,
  "explanation": "LLM最適化されたdescriptionの核心は、使用条件（いつ使うか）と非使用条件（いつ使わないか）の明示です。特に似た機能のツールが複数ある場合、使い分けの基準を記載することでClaudeの選択精度が大幅に向上します。内部実装の詳細はClaudeの判断に不要です。"
},
{
  "question": "tool_choice: {\"type\": \"tool\", \"name\": \"validate_input\"} を設定した場合の動作として正しいものはどれですか？",
  "options": [
    "Claudeがvalidate_inputを使うかどうかを自律的に判断する",
    "Claudeは必ずvalidate_inputを呼び出し、テキストのみの応答は返さない",
    "validate_input以外のツールも含めて、最適なツールを自動選択する",
    "validate_inputの定義がなくてもエラーにならず、テキスト応答を返す"
  ],
  "correct": 1,
  "explanation": "tool_choiceで特定ツールを指定すると、Claudeは必ずそのツールを呼び出します（プログラム的エンフォースメント）。テキストのみの応答は返せません。これはプロンプトでの「お願い」とは異なり、APIレベルで100%確実に動作します。ワークフローの必須ステップを強制する場合に使います。"
},
{
  "question": "推論オーバーロードを防ぐための対策として、最も効果が低いものはどれですか？",
  "options": [
    "似た機能のツールをパラメータで切り替える1つのツールに統合する",
    "会話の文脈に応じて渡すツールを動的にフィルタリングする",
    "ツール定義にプロンプトキャッシング（cache_control）を付与する",
    "ツールのdescriptionをより簡潔に書き直す"
  ],
  "correct": 2,
  "explanation": "プロンプトキャッシングはコストとレイテンシの削減には有効ですが、Claudeのツール選択精度の改善には直接寄与しません。キャッシュしてもClaudeに渡されるツール数は変わらないため、選択の「迷い」は解消されません。推論オーバーロード対策の本質はツール数の削減と動的フィルタリングです。"
}
```
