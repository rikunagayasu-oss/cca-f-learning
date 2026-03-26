# M9 エージェントアーキテクチャ — 拡充コンテンツ

> 作成日: 2026-03-26
> 担当: GO04-A2（武将18）
> 挿入先: index.html の `<div id="page-m9">` 内

---

## 1. セクションタブの追加

既存タブ（s1〜s6）に以下を追加:

```html
    <button class="section-tab" onclick="showSection('m9','s7',this)">Agent SDK</button>
    <button class="section-tab" onclick="showSection('m9','s8',this)">Hooks</button>
    <button class="section-tab" onclick="showSection('m9','s9',this)">エラー処理</button>
    <button class="section-tab" onclick="showSection('m9','s10',this)">コンテキスト隔離</button>
    <button class="section-tab" onclick="showSection('m9','s11',this)">安全なデプロイ</button>
```

---

## 2. 各セクションのHTMLコンテンツ

### セクション7: Agent SDK

```html
  <!-- Section 7: Agent SDK -->
  <div id="m9-s7" class="card" style="display:none">
    <h2>Agent SDKで作るエージェント</h2>
    <p>ここまでエージェントの「考え方」を学びました。ここからは、Anthropicが提供する<strong>Agent SDK</strong>を使って、実際にどうやってエージェントを「動かす」のかを見ていきましょう。</p>

    <p>Agent SDKは、エージェントを作るための「公式キット」です。身近な例えで言うと、エージェントループのSection 3で学んだ「計画→実行→確認→修正」のサイクルを、最小限のコードで動かせるようにした「組み立てキット」のようなものです。</p>

    <div class="concept-box">
      <div class="label">重要概念</div>
      <p><strong>Agent SDK（エージェントSDK）：</strong>Anthropicが提供するエージェント構築フレームワーク。エージェントループの実装、ツール統合、サブエージェントの管理、ガードレールの設定など、エージェントに必要な機能をワンパッケージで提供します。Python版とTypeScript版があります。</p>
    </div>

    <h3>Agent SDKの基本構成</h3>
    <p>Agent SDKには、3つの主要な構成部品があります。</p>

    <table class="compare-table">
      <tr><th>構成部品</th><th>英語名</th><th>役割</th><th>たとえ</th></tr>
      <tr><td><strong>エージェント定義</strong></td><td>Agent</td><td>エージェントの名前・指示・使えるツールを設定する「設計図」</td><td>社員の「職務記述書」</td></tr>
      <tr><td><strong>ツール定義</strong></td><td>Tool</td><td>エージェントが使える道具を定義する</td><td>社員に渡す「業務マニュアル付きの道具箱」</td></tr>
      <tr><td><strong>実行ループ</strong></td><td>Runner</td><td>エージェントループを実際に回す「エンジン」</td><td>プロジェクト管理ツール（タスクの進行を管理する仕組み）</td></tr>
    </table>

    <h3>エージェントループの実装</h3>
    <p>Section 3で学んだ「stop_reasonでループを制御する」仕組みを、Agent SDKは内部で自動的に処理してくれます。開発者が書くのは「何をさせるか」だけです。</p>

    <div class="concept-box">
      <div class="label">Agent SDKのループ制御</div>
      <p>Agent SDKの内部動作を順に追いましょう:</p>
      <p><strong>1.</strong> エージェントにタスクを渡す（「この顧客の問い合わせに対応して」）</p>
      <p><strong>2.</strong> SDKがClaudeにメッセージを送信する</p>
      <p><strong>3.</strong> Claudeが <code>stop_reason: "tool_use"</code> を返す → SDKがツールを実行し、結果をClaudeに返す → <strong>ループ継続</strong></p>
      <p><strong>4.</strong> Claudeが <code>stop_reason: "end_turn"</code> を返す → <strong>ループ終了</strong>、最終結果を返す</p>
      <p>つまり、Section 3で学んだ <code>stop_reason</code> による制御を、SDKが自動的にやってくれるのです。</p>
    </div>

    <h3>サブエージェントの作り方</h3>
    <p>Agent SDKでは、サブエージェントを<strong>ツールとして定義</strong>します。「リサーチ担当」「レビュー担当」など、専門的な役割を持つエージェントをツールのように呼び出せます。</p>

    <div class="concept-box">
      <div class="label">Agent-as-Tool パターン</div>
      <p>サブエージェントをツールとして扱うパターンです。コーディネーター（親）エージェントが、サブエージェントを「道具」として使います。</p>
      <p><strong>特徴:</strong></p>
      <p>・コーディネーターが<strong>制御権を保持</strong>したまま、サブエージェントに作業を委任する</p>
      <p>・サブエージェントの結果はツール結果としてコーディネーターに返る</p>
      <p>・コーディネーターは結果を見て、次の行動を自分で判断できる</p>
    </div>

    <div class="tip-box">
      <div class="label">試験のポイント</div>
      <p style="margin-top:.25rem;">Agent SDKのエージェントループは <code>stop_reason</code> で制御されます。「カスタムの終了条件を自然言語で設定できる」「ループ回数を指定して終了する」といった選択肢は誤りです。ただし、安全装置として <code>max_turns</code>（最大ループ回数）を設定することは推奨されます。</p>
    </div>

    <h3>ハンドオフ（Handoff）との違い</h3>
    <p>サブエージェントの呼び出し方法にはもう1つ、<strong>ハンドオフ（Handoff）</strong>があります。</p>

    <table class="compare-table">
      <tr><th>方式</th><th>英語名</th><th>制御権</th><th>たとえ</th><th>適した場面</th></tr>
      <tr><td><strong>ツールとして呼び出し</strong></td><td>Agent-as-Tool</td><td>親が保持（結果を受け取って判断）</td><td>上司が部下に調べ物を頼み、報告を受けて自分で判断する</td><td>部分的な作業の委任</td></tr>
      <tr><td><strong>ハンドオフ</strong></td><td>Handoff</td><td>子に完全移譲（親に戻らない）</td><td>担当者変更。「この件は技術部門に引き継ぎます」</td><td>専門領域への完全な引き継ぎ</td></tr>
    </table>

    <div class="warn-box">
      <div class="label">注意</div>
      <p style="margin-top:.25rem;">ハンドオフは制御権が完全に移るため、元のエージェントに処理が戻りません。「調べ物をさせて結果をもとに判断する」ような場面ではAgent-as-Toolを使い、「カスタマーサポートで技術的な質問を技術部門に完全に引き継ぐ」ような場面ではハンドオフを使います。</p>
    </div>
  </div>
```

### セクション8: Hooks

```html
  <!-- Section 8: Hooks（決定論的処理注入） -->
  <div id="m9-s8" class="card" style="display:none">
    <h2>Hooks — 決定論的な処理を注入する</h2>
    <p>Section 4で「プログラム的エンフォースメント」を学びました。その実装手段が<strong>Hooks（フック）</strong>です。</p>

    <p>Hooksとは、エージェントの動作の「特定のタイミング」にコード処理を差し込む仕組みです。身近な例えで言うと、工場の品質管理です。製品が工程から工程に移るとき、必ず検査を通す仕組み ── それがHooksです。</p>

    <div class="concept-box">
      <div class="label">重要概念</div>
      <p><strong>Hooks（フック）：</strong>エージェントのライフサイクルの特定のタイミングで実行される、<strong>決定論的な（deterministic）</strong>コード処理。LLMの判断に依存せず、100%確実に実行されます。</p>
      <p><strong>決定論的（deterministic）</strong>とは：同じ入力に対して必ず同じ結果を返すこと。LLMは確率的（probabilistic）で、同じ質問でも毎回少し違う答えを返しますが、Hooksのコードは毎回必ず同じ動作をします。</p>
    </div>

    <h3>Hooksのタイミング（ライフサイクル）</h3>
    <p>Hooksを差し込めるタイミングは、大きく4つあります。</p>

    <table class="compare-table">
      <tr><th>タイミング</th><th>英語名</th><th>いつ実行されるか</th><th>使いどころ</th></tr>
      <tr><td><strong>ツール実行前</strong></td><td>PreToolCall</td><td>エージェントがツールを呼び出す<strong>直前</strong></td><td>入力パラメータの検証、危険な操作のブロック、ログ記録</td></tr>
      <tr><td><strong>ツール実行後</strong></td><td>PostToolCall</td><td>ツールの実行が完了した<strong>直後</strong></td><td>結果の検証、監査ログの記録、異常値の検出</td></tr>
      <tr><td><strong>メッセージ送信前</strong></td><td>PreMessage</td><td>ユーザーへの応答を送る<strong>直前</strong></td><td>個人情報のマスキング、禁止用語チェック、フォーマット統一</td></tr>
      <tr><td><strong>メッセージ送信後</strong></td><td>PostMessage</td><td>ユーザーへの応答を送った<strong>直後</strong></td><td>応答ログの記録、分析データの送信</td></tr>
    </table>

    <h3>Hooksで実現できること</h3>
    <p>具体的なビジネスシーンでの活用例を見てみましょう。</p>

    <div class="concept-box">
      <div class="label">活用例: カスタマーサポートでの返金処理</div>
      <p><strong>シナリオ:</strong> AIチャットボットが顧客の返金リクエストに対応。500ドル以上の返金は人間の承認が必要。</p>
      <p><strong>PreToolCall Hook:</strong> 返金ツールが呼ばれる前に、金額をチェック。500ドル以上なら処理をブロックし、「この返金には上長の承認が必要です」と人間にエスカレーションする。</p>
      <p><strong>なぜHooksが必要か:</strong> 「500ドル以上は人間に回してね」とプロンプトで指示しても、LLMが忘れたり無視したりする可能性がゼロではありません。Hooksならコードで100%ブロックできます。</p>
    </div>

    <div class="concept-box">
      <div class="label">活用例: 個人情報の保護</div>
      <p><strong>シナリオ:</strong> AIアシスタントが社内データベースを検索して回答を生成。応答にメールアドレスや電話番号が含まれる可能性がある。</p>
      <p><strong>PostMessage Hook:</strong> 応答テキストをスキャンし、メールアドレスや電話番号のパターンを検出したら自動的にマスキング（例: user@example.com → u***@***.com）する。</p>
    </div>

    <h3>Hooks vs プロンプト指示 — 使い分け判断表</h3>
    <table class="compare-table">
      <tr><th>要件</th><th>適切な手段</th><th>理由</th></tr>
      <tr><td>金額制限の強制</td><td><strong>Hook（PreToolCall）</strong></td><td>ビジネスルールの100%遵守が必要</td></tr>
      <tr><td>個人情報の除去</td><td><strong>Hook（PostMessage）</strong></td><td>漏れは法的リスク。確実に処理する必要がある</td></tr>
      <tr><td>応答の丁寧さ</td><td>プロンプト指示</td><td>トーンの調整はLLMの得意分野。多少のブレは許容範囲</td></tr>
      <tr><td>ツール呼び出しのログ記録</td><td><strong>Hook（PostToolCall）</strong></td><td>監査要件。記録漏れは許されない</td></tr>
      <tr><td>回答の長さの目安</td><td>プロンプト指示</td><td>厳密な文字数制限でなければプロンプトで十分</td></tr>
    </table>

    <div class="tip-box">
      <div class="label">試験のポイント</div>
      <p style="margin-top:.25rem;">Hooksは<strong>「決定論的に処理する必要があるもの」</strong>に使います。判断基準は「これが守られなかったら問題か？」です。金額制限の違反、個人情報の漏洩、監査ログの欠損 ── これらは「問題」なのでHooksで強制します。一方、応答のトーンや長さは「好ましい」レベルなので、プロンプト指示で十分です。試験では「このシナリオで適切な手段は？」という問題が頻出します。</p>
    </div>
  </div>
```

### セクション9: エラー処理

```html
  <!-- Section 9: エラー伝搬とフォールバック -->
  <div id="m9-s9" class="card" style="display:none">
    <h2>エラー処理とフォールバック設計</h2>
    <p>エージェントは複数のツールを連続で使うため、途中でエラーが起きることは避けられません。ここでは「エラーが起きたらどうするか」を設計する方法を学びます。</p>

    <h3>ツールエラーの2つの分類</h3>
    <p>エージェントがツールを使ったとき、エラーが起きるのは日常的なことです。大切なのは、そのエラーが<strong>「やり直せるもの」か「やり直してもダメなもの」か</strong>を区別することです。</p>

    <div class="concept-box">
      <div class="label">重要概念</div>
      <p><strong>is_error（イズエラー）：</strong>ツールの実行結果がエラーであることを示すフラグ。ツール結果の <code>is_error: true</code> を設定すると、LLMに「この操作は失敗した」と明確に伝わります。</p>
      <p><strong>isRetryable（イズリトライアブル）：</strong>そのエラーがリトライ（再試行）可能かどうかを示すフラグ。「サーバーが一時的に混んでいる」（リトライ可能）と「パスワードが間違っている」（リトライしても無駄）を区別します。</p>
    </div>

    <table class="compare-table">
      <tr><th>エラーの種類</th><th>isRetryable</th><th>具体例</th><th>エージェントの対応</th></tr>
      <tr><td><strong>一時的なエラー</strong></td><td>true</td><td>API Rate Limit超過、ネットワーク一時障害、タイムアウト</td><td>少し待ってからリトライする</td></tr>
      <tr><td><strong>永続的なエラー</strong></td><td>false</td><td>認証失敗、存在しないリソース、権限不足</td><td>リトライせず、別のアプローチを検討する</td></tr>
    </table>

    <h3>エラー伝搬の仕組み</h3>
    <p>マルチエージェントシステムでは、サブエージェントで起きたエラーを親エージェントにどう伝えるかが重要です。これを<strong>エラー伝搬（Error Propagation）</strong>と呼びます。</p>

    <div class="concept-box">
      <div class="label">エラー伝搬のたとえ</div>
      <p>会社組織に例えると:</p>
      <p><strong>部下（サブエージェント）</strong>が取引先との交渉で問題に直面 → <strong>上司（コーディネーター）</strong>に報告 → 上司が別の部下に再委任したり、自分で対処方針を決めたりする。</p>
      <p>大切なのは「問題を隠さず報告する」ことと、「報告の際に状況を正確に伝える」ことです。</p>
    </div>

    <h3>構造化エラー報告</h3>
    <p>エラーを伝えるとき、「エラーが起きました」だけでは不十分です。以下の情報を構造化して伝えることで、親エージェントが適切に対処できます。</p>

    <table class="compare-table">
      <tr><th>情報</th><th>英語名</th><th>なぜ必要か</th><th>例</th></tr>
      <tr><td><strong>何が失敗したか</strong></td><td>error_type</td><td>問題の種類で対処法が変わる</td><td>「データベース接続エラー」</td></tr>
      <tr><td><strong>リトライ可能か</strong></td><td>isRetryable</td><td>無駄なリトライを防ぐ</td><td>true / false</td></tr>
      <tr><td><strong>試行回数</strong></td><td>attempt_count</td><td>リトライ上限の判断材料</td><td>「3回目の試行で失敗」</td></tr>
      <tr><td><strong>部分的な結果</strong></td><td>partial_result</td><td>途中まで成功した成果を活用</td><td>「10件中7件は処理完了」</td></tr>
    </table>

    <h3>フォールバック設計</h3>
    <p>エラーが起きたときの「代替手段」を事前に設計しておくことを、<strong>フォールバック設計</strong>と呼びます。</p>

    <div class="concept-box">
      <div class="label">フォールバックの3つのレベル</div>
      <p><strong>レベル1: リトライ（Retry）</strong> — 同じ操作をもう一度試す。一時的なエラーに有効。ただし<strong>指数バックオフ</strong>（1秒→2秒→4秒と待ち時間を倍々に増やす）で間隔を空けること。</p>
      <p><strong>レベル2: 代替手段（Alternative）</strong> — 別の方法で同じ目標を達成する。たとえば、メインAPIが落ちていたらバックアップAPIを使う。</p>
      <p><strong>レベル3: グレースフルデグラデーション（Graceful Degradation）</strong> — 完全な結果は出せないが、部分的な結果や「できた範囲」を返す。「全機能は使えませんが、基本機能は動きます」という状態。</p>
    </div>

    <div class="tip-box">
      <div class="label">試験のポイント</div>
      <p style="margin-top:.25rem;">ツール結果の <code>is_error</code> フラグは非常に重要です。エラーが起きたとき、<code>is_error: true</code> を設定せずに通常の結果として返すと、LLMは「成功した」と誤解してしまいます。また、<strong>べき等性（Idempotency）</strong>も出題されます。「同じ操作を2回実行しても結果が変わらない」ように設計することで、リトライを安全に行えます。</p>
    </div>

    <div class="warn-box">
      <div class="label">注意</div>
      <p style="margin-top:.25rem;">リトライする際は、必ず<strong>最大リトライ回数</strong>を設定してください。上限なしにリトライし続けると、APIコストが際限なく膨らんだり、システムが無限ループに陥ったりするリスクがあります。</p>
    </div>
  </div>
```

### セクション10: コンテキスト隔離

```html
  <!-- Section 10: コンテキスト隔離 -->
  <div id="m9-s10" class="card" style="display:none">
    <h2>コンテキスト隔離 — サブエージェントの「記憶」は共有されない</h2>
    <p>マルチエージェントシステムを設計するうえで、最も誤解されやすいのが<strong>コンテキスト隔離</strong>です。ここをしっかり理解すると、試験でのミスを大幅に減らせます。</p>

    <h3>コンテキスト隔離とは</h3>
    <div class="concept-box">
      <div class="label">重要概念</div>
      <p><strong>コンテキスト隔離（Context Isolation）：</strong>サブエージェントは親エージェント（コーディネーター）の会話履歴やコンテキストを<strong>自動的には継承しない</strong>仕組み。それぞれが独立した「記憶空間」を持ちます。</p>
    </div>

    <p>身近な例えで説明しましょう。</p>

    <div class="concept-box">
      <div class="label">たとえ: 新しく配属された社員</div>
      <p>あなた（コーディネーター）がプロジェクトの経緯を全て知っているとします。新しく配属された部下（サブエージェント）は、あなたの頭の中にある情報を自動的に知ることはできません。</p>
      <p>部下に仕事を任せるときは、必要な情報を<strong>「引き継ぎ資料」として明示的に渡す</strong>必要があります。「前の会議で話したあの件、やっておいて」と言っても、部下は「前の会議」の内容を知りません。</p>
    </div>

    <h3>なぜ自動継承しないのか</h3>
    <p>一見不便に思えますが、コンテキストを自動共有しない設計には明確な理由があります。</p>

    <table class="compare-table">
      <tr><th>理由</th><th>説明</th><th>たとえ</th></tr>
      <tr><td><strong>コンテキストウィンドウの節約</strong></td><td>親の全履歴をコピーすると、サブエージェントのコンテキストが圧迫される</td><td>部下に100ページの会議録全部を渡すより、関連する3ページだけ渡すほうが効率的</td></tr>
      <tr><td><strong>タスク集中</strong></td><td>無関係な情報がないほうが、サブエージェントは自分のタスクに集中できる</td><td>「翻訳して」と頼むとき、プロジェクトの予算情報は不要</td></tr>
      <tr><td><strong>セキュリティ</strong></td><td>機密情報が必要ないサブエージェントに渡らない</td><td>翻訳担当に顧客の個人情報を見せる必要はない</td></tr>
      <tr><td><strong>予測可能性</strong></td><td>各サブエージェントの入力が明確なので、動作がより予測しやすい</td><td>何を知っていて何を知らないかが明確</td></tr>
    </table>

    <h3>正しい情報共有の方法</h3>
    <p>コンテキストが自動で共有されない以上、必要な情報は<strong>プロンプトで明示的に渡す</strong>必要があります。</p>

    <div class="concept-box">
      <div class="label">情報共有のベストプラクティス</div>
      <p><strong>1. タスク説明に必要な背景情報を含める:</strong> サブエージェントに「この論文を要約して」と頼むなら、「医療専門家向けに、治験結果に焦点を当てて要約して」のように、目的と対象を明示する。</p>
      <p><strong>2. 研究目標と品質基準を指定する:</strong> 「手順」を細かく指示するのではなく、「何を達成すべきか」「どの品質基準を満たすべきか」を伝える。こうすることでサブエージェントの適応性を確保できる。</p>
      <p><strong>3. 過不足ない情報量:</strong> 多すぎると処理効率が下がり、少なすぎると正確な作業ができない。タスク遂行に「ちょうど必要な情報」を選んで渡す。</p>
    </div>

    <h3>よくある間違い</h3>
    <table class="compare-table">
      <tr><th>間違い</th><th>正しいアプローチ</th></tr>
      <tr><td>「親エージェントが知っている情報はサブエージェントも知っている」と前提する</td><td>必要な情報を<strong>プロンプトで明示的に渡す</strong></td></tr>
      <tr><td>親の全会話履歴をサブエージェントにコピーする</td><td>タスクに<strong>関連する情報だけ</strong>を選んで渡す</td></tr>
      <tr><td>サブエージェント同士が直接情報をやり取りすると期待する</td><td>サブエージェント間の通信は<strong>コーディネーター経由</strong>で行う</td></tr>
    </table>

    <div class="tip-box">
      <div class="label">試験のポイント</div>
      <p style="margin-top:.25rem;">「サブエージェントのコンテキストは自動的に親から継承されるか？」→ <strong>答えはNo</strong>。これは試験で繰り返し問われるポイントです。コーディネーターのプロンプトには「手順的な指示」ではなく「研究目標と品質基準」を指定するのがベストプラクティスです。また、並列実行するサブエージェントは「1回のコーディネーターレスポンスで複数のToolUseをまとめて発行する」のが正しい方法で、別々のターンに分けてはいけません。</p>
    </div>
  </div>
```

### セクション11: 安全なデプロイ

```html
  <!-- Section 11: 安全なデプロイ（Secure Deployment） -->
  <div id="m9-s11" class="card" style="display:none">
    <h2>エージェントの安全なデプロイ</h2>
    <p>優秀なエージェントを作っても、安全に動かせなければ意味がありません。Anthropicは、エージェントを本番環境にデプロイ（公開・運用開始）する際の<strong>3つの安全原則</strong>を提唱しています。</p>

    <h3>3つの安全原則</h3>
    <div class="concept-box">
      <div class="label">Secure Deployment 3原則</div>
      <p><strong>1. 隔離（Isolation）</strong> — エージェントの実行環境を他のシステムから分離する</p>
      <p><strong>2. 最小権限（Least Privilege）</strong> — エージェントに必要最小限の権限だけを与える</p>
      <p><strong>3. 多層防御（Defense in Depth）</strong> — 1つの対策に頼らず、複数の防御層を重ねる</p>
    </div>

    <h3>原則1: 隔離（Isolation）</h3>
    <p>エージェントが動く環境を、他のシステムから「壁」で仕切ることです。</p>

    <div class="concept-box">
      <div class="label">たとえ: 実験室の安全設計</div>
      <p>化学の実験は、普通のオフィスではなく「実験室」で行いますよね。なぜなら、万が一何かが起きても、実験室の中に影響が閉じ込められるからです。</p>
      <p>エージェントも同じです。エージェントが作業する「場所」を限定し、万が一おかしな動作をしても、他のシステムに影響が及ばないようにします。</p>
    </div>

    <p>具体的な隔離の方法:</p>
    <ul>
      <li><strong>コンテナ（Container）</strong>で実行環境を分離する</li>
      <li>アクセスできる<strong>ファイルやフォルダを限定</strong>する</li>
      <li><strong>ネットワーク接続先を制限</strong>する（必要なAPIだけに接続許可）</li>
      <li>サブエージェントごとに<strong>独立した実行環境</strong>を用意する</li>
    </ul>

    <h3>原則2: 最小権限（Least Privilege）</h3>
    <p>エージェントには「その仕事に必要な権限だけ」を与え、不要な権限は一切渡しません。</p>

    <div class="concept-box">
      <div class="label">たとえ: ホテルのカードキー</div>
      <p>ホテルの清掃スタッフは、自分の担当フロアの部屋だけに入れるカードキーを持っています。全室に入れるマスターキーは持っていません。これが「最小権限」の考え方です。</p>
      <p>同じように、「データベースの検索だけが必要なエージェント」に「データベースの削除権限」まで与えてはいけません。</p>
    </div>

    <p>具体的な実践:</p>
    <ul>
      <li><strong>読み取り専用</strong>で十分な場面では、書き込み権限を与えない</li>
      <li>ツールごとに<strong>操作の範囲を限定</strong>する（「全顧客データ」ではなく「該当顧客のデータのみ」）</li>
      <li>APIキーの<strong>権限スコープを最小化</strong>する</li>
      <li>時間制限付きの<strong>一時的な認証情報</strong>を使う（長期間有効な認証情報を避ける）</li>
    </ul>

    <h3>原則3: 多層防御（Defense in Depth）</h3>
    <p>1つの対策だけに頼らず、複数の防御策を重ねます。「城の防衛」のように、堀→城壁→見張り台と複数の防御層を設けます。</p>

    <div class="concept-box">
      <div class="label">多層防御の実践例</div>
      <p>カスタマーサポートエージェントの場合:</p>
      <p><strong>第1層 — プロンプト:</strong> システムプロンプトで「顧客データの変更は確認を取ってから」と指示</p>
      <p><strong>第2層 — Hook:</strong> PreToolCallフックで高額操作（返金500ドル超）をブロック</p>
      <p><strong>第3層 — 権限:</strong> データベースアクセスを「該当顧客のデータのみ」に制限</p>
      <p><strong>第4層 — 監査:</strong> 全操作をログに記録し、後から確認可能にする</p>
      <p>第1層（プロンプト）が突破されても、第2層（Hook）が止め、それも突破されても第3層（権限）で被害を最小化します。</p>
    </div>

    <h3>人間の関与（Human-in-the-Loop）</h3>
    <p>安全なデプロイには、「どの操作で人間の確認を求めるか」の設計も含まれます。</p>

    <table class="compare-table">
      <tr><th>リスクレベル</th><th>操作例</th><th>推奨される対応</th></tr>
      <tr><td><strong>低リスク</strong></td><td>情報の検索・閲覧</td><td>自動実行（人間の確認不要）</td></tr>
      <tr><td><strong>中リスク</strong></td><td>データの更新・メール送信</td><td>実行前に人間の確認を求める</td></tr>
      <tr><td><strong>高リスク</strong></td><td>データの削除・金融取引</td><td>人間の明示的な承認が必須</td></tr>
    </table>

    <div class="tip-box">
      <div class="label">試験のポイント</div>
      <p style="margin-top:.25rem;">Secure Deploymentの3原則（隔離・最小権限・多層防御）は、エージェントアーキテクチャの設計問題として出題される可能性が高い分野です。特に「プロンプトだけで安全を確保する」という選択肢はアンチパターンです。多層防御の考え方を理解し、「プロンプト + Hook + 権限制限 + 監査ログ」のように複数のレイヤーで保護する設計を選びましょう。</p>
    </div>

    <div class="warn-box">
      <div class="label">注意</div>
      <p style="margin-top:.25rem;">エージェントが外部のAPIやツールに接続する際は、ツールが返すデータにも注意が必要です。悪意のあるウェブページやデータが「プロンプトインジェクション」（エージェントの指示を上書きしようとする攻撃）を含む可能性があります。ツール結果は「信頼できない外部入力」として扱い、検証してから処理しましょう。</p>
    </div>
  </div>
```

---

## 3. 用語集への追加項目

既存のSection 5（m9-s5）に以下の用語を追加:

```html
    <div class="glossary-item">
      <div class="glossary-term">Agent SDK</div>
      <div class="glossary-en">Agent SDK</div>
      <div class="glossary-def">Anthropicが提供するエージェント構築フレームワーク。エージェントループ、ツール統合、サブエージェント管理を最小限のコードで実装できる。</div>
    </div>
    <div class="glossary-item">
      <div class="glossary-term">フック</div>
      <div class="glossary-en">Hooks</div>
      <div class="glossary-def">エージェントのライフサイクルの特定タイミング（ツール実行前後、メッセージ送信前後）に差し込む決定論的なコード処理。LLMの判断に依存せず100%実行される。</div>
    </div>
    <div class="glossary-item">
      <div class="glossary-term">コンテキスト隔離</div>
      <div class="glossary-en">Context Isolation</div>
      <div class="glossary-def">サブエージェントが親エージェントの会話履歴やコンテキストを自動的に継承しない仕組み。必要な情報はプロンプトで明示的に渡す。</div>
    </div>
    <div class="glossary-item">
      <div class="glossary-term">最小権限</div>
      <div class="glossary-en">Least Privilege</div>
      <div class="glossary-def">エージェントにタスク遂行に必要な最小限の権限だけを付与する安全原則。不要な権限は与えない。</div>
    </div>
    <div class="glossary-item">
      <div class="glossary-term">多層防御</div>
      <div class="glossary-en">Defense in Depth</div>
      <div class="glossary-def">プロンプト、Hook、権限制限、監査ログなど複数の防御レイヤーを重ねる安全設計。単一の対策に依存しない。</div>
    </div>
    <div class="glossary-item">
      <div class="glossary-term">グレースフルデグラデーション</div>
      <div class="glossary-en">Graceful Degradation</div>
      <div class="glossary-def">エラー発生時に全体を停止するのではなく、機能を縮退させつつ部分的な結果を返す設計手法。</div>
    </div>
    <div class="glossary-item">
      <div class="glossary-term">べき等性</div>
      <div class="glossary-en">Idempotency</div>
      <div class="glossary-def">同じ操作を複数回実行しても結果が変わらない性質。リトライを安全に行うための重要な設計原則。</div>
    </div>
```

---

## 4. 学習目標の更新

既存の学習目標に追加:

```html
      <li>Agent SDKの基本構成とエージェントループの実装を理解する</li>
      <li>Hooksによる決定論的な処理注入の仕組みと適用場面を判断できる</li>
      <li>エラー伝搬・フォールバック設計のパターンを知る</li>
      <li>コンテキスト隔離の仕組みと、正しい情報共有の方法を理解する</li>
      <li>安全なデプロイの3原則（隔離・最小権限・多層防御）を説明できる</li>
```

---

## 5. 確認クイズ（5問追加分）

以下をQUIZ_DATAのm9エントリに追加（既存5問の後に追加）:

```json
{
  "question": "Agent SDKでサブエージェントを呼び出す際、「Agent-as-Tool」パターンと「Handoff」パターンの最も重要な違いは何ですか？",
  "options": [
    "Agent-as-Toolは同期実行、Handoffは非同期実行である",
    "Agent-as-Toolは親エージェントが制御権を保持し、Handoffは子エージェントに制御権を完全移譲する",
    "Agent-as-Toolはテキスト結果のみ返し、Handoffはファイルも返せる",
    "Agent-as-Toolは1つのツールしか使えないが、Handoffは複数ツールを使える"
  ],
  "correct": 1,
  "explanation": "Agent-as-Toolでは親エージェント（コーディネーター）が制御権を保持し、サブエージェントの結果を受け取って次の判断ができます。一方、Handoffでは制御権が子エージェントに完全に移り、元のエージェントには処理が戻りません。カスタマーサポートの「技術部門への引き継ぎ」のような場面ではHandoff、「部下に調べ物を頼んで報告を受ける」場面ではAgent-as-Toolが適切です。"
},
{
  "question": "カスタマーサポートのAIチャットボットで「500ドル以上の返金処理には人間の承認が必要」というルールを強制する最も適切な方法はどれですか？",
  "options": [
    "システムプロンプトに「500ドル以上の返金は人間に確認してください」と記載する",
    "返金ツールのdescriptionに金額制限について詳しく書く",
    "PreToolCall Hookで返金金額をチェックし、500ドル以上ならブロックする",
    "Evaluator-Optimizerパターンで返金処理をレビューする"
  ],
  "correct": 2,
  "explanation": "金額制限のようなビジネスルールの100%遵守が必要な場面では、プロンプト指示（選択肢A・B）だけでは不十分です。LLMは確率的であり、指示を無視する可能性がゼロではありません。PreToolCall Hookならコードで決定論的にチェックでき、500ドル以上の返金を確実にブロックできます。これがプログラム的エンフォースメントの典型例です。"
},
{
  "question": "サブエージェントのコンテキスト隔離について、正しい記述はどれですか？",
  "options": [
    "サブエージェントは親エージェントの全会話履歴を自動的に継承する",
    "サブエージェントは親エージェントのコンテキストを継承しないため、必要な情報はプロンプトで明示的に渡す必要がある",
    "コンテキスト隔離はオプション設定で、デフォルトでは共有が有効になっている",
    "サブエージェント同士は直接通信でき、コーディネーターを経由する必要はない"
  ],
  "correct": 1,
  "explanation": "Agent SDKにおいて、サブエージェントは親エージェントのコンテキストを自動的には継承しません。これはコンテキストウィンドウの節約、タスクへの集中、セキュリティ（不要な機密情報を渡さない）のための設計です。必要な情報は、コーディネーターがサブエージェントを呼び出す際のプロンプトに明示的に含めます。"
},
{
  "question": "エージェントの安全なデプロイ（Secure Deployment）の3原則として正しい組み合わせはどれですか？",
  "options": [
    "暗号化（Encryption）、認証（Authentication）、認可（Authorization）",
    "テスト（Testing）、監視（Monitoring）、ロールバック（Rollback）",
    "隔離（Isolation）、最小権限（Least Privilege）、多層防御（Defense in Depth）",
    "可用性（Availability）、一貫性（Consistency）、耐障害性（Fault Tolerance）"
  ],
  "correct": 2,
  "explanation": "Anthropicが提唱するSecure Deploymentの3原則は、隔離（Isolation）＝実行環境を他のシステムから分離、最小権限（Least Privilege）＝必要最小限の権限だけを付与、多層防御（Defense in Depth）＝プロンプト・Hook・権限制限・監査ログなど複数の防御層を重ねることです。選択肢Aは一般的なセキュリティ概念、Bはデプロイプラクティス、DはCAP定理関連です。"
},
{
  "question": "エージェントがツール実行でエラーを受け取った場合、is_error フラグの扱いとして正しいのはどれですか？",
  "options": [
    "is_errorはオプションなので、エラーでも通常の結果として返して問題ない",
    "is_error: true を設定し、isRetryable（リトライ可能か）も併せて返すことで、エージェントが適切に対処できる",
    "is_errorはUI表示用のフラグなので、エージェントの動作には影響しない",
    "エラー時はツール結果を返さず、エージェントに空のレスポンスを返すのが正しい"
  ],
  "correct": 1,
  "explanation": "ツール実行でエラーが発生した場合、is_error: true を設定してLLMに「この操作は失敗した」と明確に伝える必要があります。is_errorを設定しないと、LLMはエラーメッセージを通常の結果と誤解する可能性があります。さらにisRetryableフラグでリトライ可能かどうかを示すと、エージェントは「リトライするか、別のアプローチを取るか」を適切に判断できます。"
}
```
