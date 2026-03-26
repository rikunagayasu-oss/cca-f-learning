# M6 MCP統合 — 拡充コンテンツ（HTMLスニペット）

> 作成日: 2026-03-26
> タスク: GO04-A1 タスクB
> 挿入先: index.html の M6セクション（既存s1-s6の後に追加）

---

## 追加セクションタブ（section-navに追加）

```html
<button class="section-tab" onclick="showSection('m6','s7',this)">設定ファイル階層</button>
<button class="section-tab" onclick="showSection('m6','s8',this)">サーバー設計</button>
<button class="section-tab" onclick="showSection('m6','s9',this)">高度な機能</button>
<button class="section-tab" onclick="showSection('m6','s10',this)">トランスポート選定</button>
```

---

## セクション7: MCP設定ファイルの階層構造

```html
<div id="m6-s7" class="card" style="display:none">
  <h2>MCP設定ファイルの階層構造</h2>
  <p>MCPサーバーをClaude Codeで使うには、<strong>設定ファイル</strong>に接続情報を記述します。設定ファイルには複数のレベルがあり、「どの設定ファイルに書くか」で<strong>適用範囲が変わる</strong>仕組みです。</p>

  <div class="concept-box">
    <div class="label">たとえ話で理解する</div>
    <p>設定ファイルの階層は<strong>「会社のルール」</strong>に似ています。会社全体の就業規則（グローバル設定）があり、部署ごとのローカルルール（プロジェクト設定）もある。部署のルールが優先されますが、ベースは全社ルールです。</p>
  </div>

  <h3>2つの設定ファイル</h3>
  <table class="compare-table">
    <tr><th>設定ファイル</th><th>場所</th><th>適用範囲</th><th>共有</th><th>たとえ</th></tr>
    <tr>
      <td><strong>.mcp.json</strong></td>
      <td>プロジェクトのルートフォルダ</td>
      <td>そのプロジェクトのみ</td>
      <td>チームで共有（Gitにコミット可能）</td>
      <td>部署のルール</td>
    </tr>
    <tr>
      <td><strong>~/.claude.json</strong></td>
      <td>ユーザーのホームフォルダ</td>
      <td>すべてのプロジェクト</td>
      <td>個人のみ（Gitにコミットしない）</td>
      <td>全社の就業規則</td>
    </tr>
  </table>

  <h3>優先順位のルール</h3>
  <p>同じMCPサーバーが両方の設定ファイルに記述されている場合、<strong>プロジェクトレベル（.mcp.json）が優先</strong>されます。</p>
  <ol>
    <li><strong>.mcp.json</strong>（プロジェクト固有）— 最優先</li>
    <li><strong>~/.claude.json</strong>（グローバル）— .mcp.jsonにない設定のみ適用</li>
  </ol>

  <div class="warn-box">
    <div class="label">セキュリティ上の注意</div>
    <p style="margin-top:.25rem;">APIキーやパスワードなどの<strong>認証情報は.mcp.jsonに直接書かない</strong>でください。.mcp.jsonはGitにコミットされる可能性があります。認証情報は環境変数（<code>env</code>フィールド）を使って渡すのが安全です。</p>
  </div>

  <h3>設定ファイルの記述例</h3>
  <p>プロジェクトの<code>.mcp.json</code>にMCPサーバーを登録する例：</p>
  <pre><code>{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/dir"],
      "env": {}
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}</code></pre>

  <div class="concept-box">
    <div class="label">使い分けのガイドライン</div>
    <p><strong>.mcp.json に書くもの：</strong>プロジェクト固有のMCPサーバー（そのプロジェクトのDB接続、特定のAPI連携など）。チームメンバー全員が使う設定。</p>
    <p><strong>~/.claude.json に書くもの：</strong>個人的に使うMCPサーバー（メモアプリ連携、個人用ツールなど）。すべてのプロジェクトで横断的に使いたい設定。</p>
  </div>

  <h3>Enterprise環境でのMCP管理</h3>
  <p>企業のEnterprise/Teamプランでは、<strong>管理者がMCPコネクターを制御</strong>できます。管理者は、ユーザーが接続を許可されるMCPサーバーの種類を制限できます。これにより、セキュリティポリシーに沿ったMCP利用が可能になります。</p>

  <div class="tip-box">
    <div class="label">試験のポイント</div>
    <p style="margin-top:.25rem;">CCA-F試験では「プロジェクトレベル（.mcp.json）とグローバルレベル（~/.claude.json）の優先順位」が問われる可能性が高いです。<strong>プロジェクトレベルが優先</strong>という点を確実に覚えましょう。</p>
  </div>
</div>
```

---

## セクション8: MCPサーバー設計パターン

```html
<div id="m6-s8" class="card" style="display:none">
  <h2>MCPサーバー設計パターン</h2>
  <p>MCPサーバーを設計・構築する際には、<strong>セキュリティ</strong>と<strong>使いやすさ</strong>の両面を考慮する必要があります。</p>

  <h3>認証・権限管理のパターン</h3>
  <p>MCPサーバーが外部サービス（データベース、API、ファイルシステムなど）にアクセスする場合、<strong>認証（Authentication）</strong>と<strong>認可（Authorization）</strong>の設計が重要です。</p>

  <div class="concept-box">
    <div class="label">たとえ話で理解する</div>
    <p><strong>認証</strong>は「身分証を見せてもらう」こと、<strong>認可</strong>は「入れる部屋を制限する」こと。ホテルに例えると、チェックイン（認証）で身元を確認し、ルームキー（認可）で自分の部屋にだけ入れるようにします。</p>
  </div>

  <table class="compare-table">
    <tr><th>パターン</th><th>仕組み</th><th>適している場面</th></tr>
    <tr>
      <td><strong>環境変数方式</strong></td>
      <td>APIキーやトークンを環境変数で渡す</td>
      <td>ローカル開発、個人利用</td>
    </tr>
    <tr>
      <td><strong>OAuth連携</strong></td>
      <td>ユーザーの代わりにサービスにログインする仕組み</td>
      <td>チーム利用、Webサービス連携</td>
    </tr>
    <tr>
      <td><strong>Roots制約</strong></td>
      <td>サーバーがアクセスできるファイルパスを制限する</td>
      <td>ファイルシステムアクセス</td>
    </tr>
  </table>

  <h3>Roots（ルーツ）— アクセス範囲の制限</h3>
  <p>MCPの<strong>Roots</strong>は、サーバーが安全にファイルアクセスするための仕組みです。ホスト（Claude Desktopなど）がサーバーに対して「ここからここまでの範囲だけ見てよい」と指定します。</p>

  <div class="concept-box">
    <div class="label">たとえ話で理解する</div>
    <p>Rootsは<strong>「立ち入り許可証」</strong>のようなものです。工場見学に来たお客さんに「見学コースのエリアだけ見てOK、研究棟は立入禁止」と範囲を指定するイメージです。</p>
  </div>

  <h3>サーバー設計のベストプラクティス</h3>
  <ul>
    <li><strong>最小権限の原則</strong>：サーバーには必要最小限のアクセス権限だけを与える</li>
    <li><strong>入力の検証</strong>：クライアントからの入力を常に検証し、不正なリクエストを拒否する</li>
    <li><strong>エラーの構造化</strong>：エラーが発生した場合、<code>isError: true</code>を返し、エラーの内容を明確に伝える</li>
    <li><strong>タイムアウトの設定</strong>：長時間かかる処理にはタイムアウトを設け、無限待ちを防ぐ</li>
    <li><strong>ログの記録</strong>：重要な操作とエラーをログに記録し、トラブルシューティングに備える</li>
  </ul>

  <div class="warn-box">
    <div class="label">よくある設計ミス</div>
    <p style="margin-top:.25rem;">MCPサーバーに<strong>必要以上の権限</strong>を与えてしまうのは危険です。たとえば、読み取りだけが必要なのに書き込み権限まで与えたり、特定フォルダだけアクセスすればよいのにファイルシステム全体へのアクセスを許可したりすると、セキュリティリスクが高まります。</p>
  </div>

  <h3>ツール定義のベストプラクティス</h3>
  <p>MCPサーバーが公開するツールの<strong>description（説明文）</strong>は、LLM（AIモデル）がツールを選ぶ際の判断材料になります。</p>
  <table class="compare-table">
    <tr><th>要素</th><th>内容</th><th>例</th></tr>
    <tr>
      <td><strong>目的</strong></td>
      <td>ツールが何をするか</td>
      <td>「指定されたディレクトリ内のファイル一覧を取得する」</td>
    </tr>
    <tr>
      <td><strong>入力の説明</strong></td>
      <td>各パラメータの意味と形式</td>
      <td>「path: 対象のディレクトリパス（絶対パスまたは相対パス）」</td>
    </tr>
    <tr>
      <td><strong>制約・注意事項</strong></td>
      <td>ツールの限界やエッジケース</td>
      <td>「隠しファイル（.で始まるファイル）は含まれません」</td>
    </tr>
    <tr>
      <td><strong>使用例</strong></td>
      <td>具体的な呼び出し例</td>
      <td>「path: '/home/user/documents'」</td>
    </tr>
  </table>

  <div class="tip-box">
    <div class="label">試験のポイント</div>
    <p style="margin-top:.25rem;">MCPサーバーの設計では「<strong>最小権限</strong>」「<strong>Rootsによるアクセス制限</strong>」「<strong>ツール説明文のLLM最適化</strong>」がキーワードです。特にツールのdescriptionは「LLMが読んで判断する」ことを意識して書く必要があります。</p>
  </div>
</div>
```

---

## セクション9: MCPの高度な機能

```html
<div id="m6-s9" class="card" style="display:none">
  <h2>MCPの高度な機能</h2>
  <p>MCPには基本的な3つのプリミティブ（Resources, Tools, Prompts）に加えて、より高度な機能があります。</p>

  <h3>サンプリング（Sampling）— AIの力を借りる</h3>
  <p>サンプリングは、MCPサーバーがクライアントを通じて<strong>LLM（AIモデル）に推論を依頼できる</strong>仕組みです。通常、MCPの通信は「ホスト → サーバー」の方向ですが、サンプリングでは<strong>逆方向</strong>（サーバー → ホスト経由でLLMに問い合わせ）が可能になります。</p>

  <div class="concept-box">
    <div class="label">たとえ話で理解する</div>
    <p>サンプリングは<strong>「出張先の社員が本社に判断を仰ぐ」</strong>イメージです。出張先（サーバー）で判断に迷ったとき、本社（ホスト内のLLM）に電話して「この件、どう判断すべきですか？」と相談します。本社が回答を返し、出張先の社員はその判断に基づいて作業を進めます。</p>
  </div>

  <h4>サンプリングが役立つ場面</h4>
  <ul>
    <li><strong>データの要約</strong>：大量のデータを取得した後、LLMに要約させてからクライアントに返す</li>
    <li><strong>判断の委譲</strong>：複数の選択肢がある場面で、LLMに最適な選択を判断してもらう</li>
    <li><strong>コスト管理</strong>：AI推論のコストをクライアント側（ホスト側）に負担させる設計が可能</li>
  </ul>

  <div class="warn-box">
    <div class="label">セキュリティ上の注意</div>
    <p style="margin-top:.25rem;">サンプリングリクエストは、必ず<strong>ホスト（ユーザー側）が承認</strong>してからLLMに送信されます。サーバーが勝手にLLMを使い放題にはできない仕組みです。これはHuman-in-the-loop（人間による確認）の原則に基づいています。</p>
  </div>

  <h3>通知（Notifications）— 進捗を伝える</h3>
  <p>MCPサーバーは、処理の進捗状況をクライアントに<strong>通知（Notification）</strong>として送ることができます。</p>

  <h4>2種類の通知</h4>
  <table class="compare-table">
    <tr><th>通知の種類</th><th>英語名</th><th>内容</th><th>たとえ</th></tr>
    <tr>
      <td><strong>進捗通知</strong></td>
      <td>Progress Notification</td>
      <td>「全体の何%が完了したか」をリアルタイムで伝える</td>
      <td>ダウンロードの進捗バー</td>
    </tr>
    <tr>
      <td><strong>ログ通知</strong></td>
      <td>Log Notification</td>
      <td>処理の途中経過や警告をテキストで伝える</td>
      <td>作業日報の随時報告</td>
    </tr>
  </table>

  <p>通知は「応答を求めない一方通行のメッセージ」です。サーバーからクライアントへの報告であり、クライアントからの返事は不要です。</p>

  <div class="concept-box">
    <div class="label">たとえ話で理解する</div>
    <p>通知は<strong>「掲示板への張り出し」</strong>のようなものです。作業者（サーバー）が「いま50%完了しました」と掲示板に貼り出し、見たい人（クライアント）は見る。でも返事はしない。一方通行の情報共有です。</p>
  </div>

  <h3>初期化ハンドシェイク — 最初のあいさつ</h3>
  <p>MCPクライアントとサーバーが接続する最初のステップでは、<strong>初期化ハンドシェイク</strong>が行われます。お互いが「私はこれができます」と自己紹介し合う仕組みです。</p>
  <ol>
    <li>クライアントが<strong>initialize</strong>リクエストを送信（自分の対応バージョンと能力を伝える）</li>
    <li>サーバーが応答（自分の対応バージョンと能力を伝える）</li>
    <li>クライアントが<strong>initialized</strong>通知を送信（接続完了を確認）</li>
  </ol>
  <p>この仕組みにより、クライアントとサーバーが互いの機能を確認してから通信を開始するため、バージョンの不一致による問題を防ぎます。</p>

  <div class="tip-box">
    <div class="label">試験のポイント</div>
    <p style="margin-top:.25rem;">サンプリングでは「<strong>サーバーからLLMへの逆方向通信</strong>」「<strong>ホストの承認が必要</strong>」がポイント。通知は「<strong>一方通行で応答不要</strong>」が特徴。初期化ハンドシェイクは「<strong>能力交渉（Capability Negotiation）</strong>」の具体的な実装です。</p>
  </div>
</div>
```

---

## セクション10: トランスポート選定ガイド

```html
<div id="m6-s10" class="card" style="display:none">
  <h2>トランスポート選定ガイド</h2>
  <p>MCPには複数の<strong>トランスポート方式</strong>（データの運び方）があり、用途に応じて選び分けます。基本のstdioとStreamable HTTPに加えて、本番運用で重要な<strong>ステートレスHTTP</strong>についても理解しましょう。</p>

  <h3>3つのトランスポート方式の比較</h3>
  <table class="compare-table">
    <tr><th>方式</th><th>接続の特徴</th><th>状態管理</th><th>スケーラビリティ</th><th>適している場面</th></tr>
    <tr>
      <td><strong>stdio</strong></td>
      <td>ローカルのサブプロセスとして起動</td>
      <td>プロセス内で自然に保持</td>
      <td>単一マシンのみ</td>
      <td>ローカル開発、個人利用</td>
    </tr>
    <tr>
      <td><strong>Streamable HTTP</strong></td>
      <td>HTTP接続 + ストリーミング対応</td>
      <td>セッションで状態を保持</td>
      <td>中程度（セッション固定が必要）</td>
      <td>チーム利用、社内サーバー</td>
    </tr>
    <tr>
      <td><strong>ステートレスHTTP</strong></td>
      <td>HTTP接続（ストリーミングなし）</td>
      <td>状態を保持しない</td>
      <td>高い（水平スケーリング可能）</td>
      <td>大規模本番運用</td>
    </tr>
  </table>

  <div class="concept-box">
    <div class="label">たとえ話で理解する</div>
    <p><strong>stdio</strong>は「同じ部屋にいる人と直接話す」。<strong>Streamable HTTP</strong>は「専属の電話回線で遠方と通話する（回線は維持される）」。<strong>ステートレスHTTP</strong>は「手紙のやりとり（毎回差出人と用件を書く必要があるが、郵便局は何通でも処理できる）」。</p>
  </div>

  <h3>選定フローチャート</h3>
  <p>どのトランスポートを選ぶべきかの判断基準：</p>
  <ol>
    <li><strong>ローカルで動かすだけ？</strong> → <strong>stdio</strong>（最もシンプル）</li>
    <li><strong>リモートアクセスが必要？</strong> → HTTP系を検討</li>
    <li><strong>リアルタイムの進捗通知が必要？</strong> → <strong>Streamable HTTP</strong></li>
    <li><strong>大量のユーザーに対応する必要がある？</strong> → <strong>ステートレスHTTP</strong></li>
  </ol>

  <h3>Streamable HTTPの詳細</h3>
  <p>Streamable HTTPは、従来のSSE（Server-Sent Events）トランスポートに代わる新しい方式です。HTTPリクエスト/レスポンスとストリーミングの両方に対応します。</p>
  <ul>
    <li><strong>セッション管理</strong>：サーバーが<code>Mcp-Session-Id</code>ヘッダーでセッションを追跡</li>
    <li><strong>双方向通信</strong>：サーバーからクライアントへの通知をストリーミングで送信可能</li>
    <li><strong>再接続対応</strong>：ネットワーク切断後に自動再接続が可能</li>
  </ul>

  <h3>ステートレスHTTPのトレードオフ</h3>
  <p>ステートレスHTTPは大規模運用に向いていますが、いくつかの機能を犠牲にします：</p>
  <table class="compare-table">
    <tr><th>特徴</th><th>Streamable HTTP</th><th>ステートレスHTTP</th></tr>
    <tr><td>進捗通知</td><td>対応</td><td><strong>非対応</strong>（ストリーミングなし）</td></tr>
    <tr><td>サンプリング</td><td>対応</td><td><strong>非対応</strong>（逆方向通信不可）</td></tr>
    <tr><td>ロードバランサー</td><td>セッション固定が必要</td><td><strong>どのサーバーでも処理可能</strong></td></tr>
    <tr><td>水平スケーリング</td><td>制限あり</td><td><strong>容易</strong></td></tr>
  </table>

  <div class="warn-box">
    <div class="label">注意: トレードオフを理解する</div>
    <p style="margin-top:.25rem;">ステートレスHTTPは「スケーラビリティ（多数のユーザーに対応する能力）」を得る代わりに、「サンプリング」「進捗通知」「サーバーからの能動的な通信」を失います。本番運用では、この<strong>トレードオフ</strong>を理解した上で方式を選ぶ必要があります。</p>
  </div>

  <div class="tip-box">
    <div class="label">試験のポイント</div>
    <p style="margin-top:.25rem;">トランスポート選定では「<strong>stdioはローカル専用</strong>」「<strong>Streamable HTTPはリモート対応+ストリーミング</strong>」「<strong>ステートレスHTTPはスケーラブルだがサンプリング不可</strong>」という特徴の違いが出題される可能性が高いです。特に「スケーラビリティとサンプリングのトレードオフ」は要注意です。</p>
  </div>
</div>
```

---

## 追加クイズデータ（JSON形式）

以下を `QUIZ_DATA` オブジェクトの `m6` 配列の末尾に追加（既存5問の後ろにQ6-Q10として追加）。
または、拡充分として別途管理する場合は以下のJSON構造を使用：

```json
{
  "m6_expansion": [
    {
      "question": "MCPの設定ファイル .mcp.json と ~/.claude.json の両方に同じMCPサーバーが定義されている場合、どちらの設定が優先されますか？",
      "options": [
        "~/.claude.json（グローバル設定）が常に優先される",
        ".mcp.json（プロジェクト設定）が優先される",
        "後から読み込まれた設定が優先される",
        "エラーとなり、どちらかを削除する必要がある"
      ],
      "correct": 1,
      "explanation": "MCPの設定ファイルはプロジェクトレベル（.mcp.json）がグローバルレベル（~/.claude.json）より優先されます。これはCLAUDE.mdの階層構造と同じ考え方で、より具体的（プロジェクト固有）な設定が優先されるという原則に基づいています。"
    },
    {
      "question": "MCPサーバーの「Roots」機能の目的として最も適切なものはどれですか？",
      "options": [
        "MCPサーバーの起動時に必要な初期設定を定義する",
        "MCPサーバーがアクセスできるファイルパスの範囲を制限する",
        "MCPクライアントとサーバーの認証を行う",
        "MCPサーバーのログ出力先を指定する"
      ],
      "correct": 1,
      "explanation": "Rootsは、ホスト（Claude Desktopなど）がMCPサーバーに対して「アクセスしてよいファイルパスの範囲」を指定する仕組みです。最小権限の原則に基づき、サーバーが不必要な範囲にアクセスすることを防ぎます。"
    },
    {
      "question": "MCPの「サンプリング（Sampling）」機能について正しい記述はどれですか？",
      "options": [
        "クライアントがサーバーにデータのサンプルを要求する機能",
        "サーバーがクライアントを通じてLLMに推論を依頼でき、ホストの承認が必要",
        "大量データからランダムにデータを抽出する統計的手法",
        "サーバーがLLMに直接アクセスして推論を行う機能"
      ],
      "correct": 1,
      "explanation": "サンプリングは、MCPサーバーがクライアント経由でホスト内のLLMに推論を依頼する「逆方向通信」の仕組みです。セキュリティのため、サンプリングリクエストは必ずホスト（ユーザー側）の承認を経てからLLMに送信されます。サーバーがLLMに直接アクセスすることはできません。"
    },
    {
      "question": "MCPの通知（Notification）について正しい記述はどれですか？",
      "options": [
        "クライアントからサーバーへの双方向通信で、応答を待つ必要がある",
        "サーバーからクライアントへの一方通行のメッセージで、応答は不要",
        "エラーが発生した場合にのみ送信される緊急メッセージ",
        "初期化ハンドシェイクの一部としてのみ使用される"
      ],
      "correct": 1,
      "explanation": "MCPの通知は、サーバーからクライアントへの一方通行のメッセージです。進捗通知（処理の何%が完了したか）やログ通知（処理の途中経過）があり、クライアントからの返事は不要です。通常の処理中にも随時送信できます。"
    },
    {
      "question": "大規模な本番環境でMCPサーバーを運用する場合、ステートレスHTTPトランスポートを選ぶと得られるメリットと失うものの組み合わせとして正しいのはどれですか？",
      "options": [
        "メリット: リアルタイム通知が使える / 失う: スケーラビリティ",
        "メリット: 水平スケーリングが容易 / 失う: サンプリング機能と進捗通知",
        "メリット: セッション管理が自動化される / 失う: HTTP通信の安定性",
        "メリット: サンプリングが高速化される / 失う: ログ通知"
      ],
      "correct": 1,
      "explanation": "ステートレスHTTPは状態を保持しないため、ロードバランサーで複数サーバーに振り分ける水平スケーリングが容易です。一方、状態を持たないためストリーミングができず、サンプリング（逆方向通信）や進捗通知が使えなくなるトレードオフがあります。"
    }
  ]
}
```

---

## 追加用語（既存用語集セクション m6-s5 に追加）

```html
<div class="glossary-item"><div class="glossary-term">Roots</div><div class="glossary-en">Roots</div><div class="glossary-def">MCPサーバーがアクセスできるファイルパスの範囲を制限する仕組み。ホストが指定する。</div></div>
<div class="glossary-item"><div class="glossary-term">進捗通知</div><div class="glossary-en">Progress Notification</div><div class="glossary-def">MCPサーバーが処理の進捗状況をクライアントに送る一方通行のメッセージ。</div></div>
<div class="glossary-item"><div class="glossary-term">ログ通知</div><div class="glossary-en">Log Notification</div><div class="glossary-def">MCPサーバーが処理の途中経過や警告をテキストでクライアントに送る通知。</div></div>
<div class="glossary-item"><div class="glossary-term">初期化ハンドシェイク</div><div class="glossary-en">Initialization Handshake</div><div class="glossary-def">MCPクライアントとサーバーが接続時に互いの能力を確認し合う手順。</div></div>
<div class="glossary-item"><div class="glossary-term">ステートレスHTTP</div><div class="glossary-en">Stateless HTTP</div><div class="glossary-def">状態を保持しないHTTPトランスポート。水平スケーリングが容易だが、サンプリングや通知は非対応。</div></div>
<div class="glossary-item"><div class="glossary-term">Streamable HTTP</div><div class="glossary-en">Streamable HTTP</div><div class="glossary-def">HTTP通信にストリーミング機能を加えたMCPトランスポート。リモート接続で進捗通知やサンプリングに対応。</div></div>
