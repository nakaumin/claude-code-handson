---
marp: true
theme: default
paginate: true
size: 16:9
header: "Claude Code 実践ハンズオン"
style: |
  section {
    font-family: 'Hiragino Sans', 'Noto Sans JP', sans-serif;
    font-size: 1.55rem;
    line-height: 1.5;
    padding: 50px 60px 80px 60px;
  }
  section.lead {
    padding: 60px;
  }
  h1 {
    color: #D97757;
    border-bottom: 3px solid #D97757;
    padding-bottom: 0.25em;
    margin-bottom: 0.4em;
    font-size: 1.9em;
  }
  h2 {
    color: #2B2B2B;
    font-size: 1.3em;
  }
  h3 {
    font-size: 1.1em;
    margin-top: 0.6em;
    margin-bottom: 0.3em;
  }
  p, ul, ol {
    margin: 0.4em 0;
  }
  li {
    margin: 0.15em 0;
  }
  code {
    background-color: #F5F5F0;
    color: #CC5500;
    padding: 2px 6px;
    border-radius: 4px;
    font-size: 0.92em;
  }
  pre {
    background-color: #2B2B2B;
    color: #F5F5F0;
    font-size: 0.78em;
    line-height: 1.35;
    padding: 0.6em 0.8em;
    margin: 0.4em 0;
  }
  blockquote {
    font-size: 0.9em;
    margin: 0.4em 0;
  }
  table {
    font-size: 0.8em;
    margin: 0.3em 0;
  }
  th, td {
    padding: 0.3em 0.5em;
  }
  .small {
    font-size: 0.75em;
  }
  .center {
    text-align: center;
  }
  .highlight {
    background-color: #FFF3CD;
    padding: 0.5em 1em;
    border-left: 4px solid #D97757;
  }
---

<!-- _class: lead -->
<!-- _paginate: false -->

# Claude Code 実践ハンズオン

<br>

**基礎設定 → Google Workspace連携 → エージェントチーム**

---

<!-- _header: '' -->
<!-- _footer: '' -->

# 今日のゴール

90分後、あなたは以下ができるようになります:

1. **基礎** - Claude Code を安全・快適に使う設定と TIPS を身につける
2. **実用** - Gmail / Calendar / Drive を Claude から操作し、日常業務を自動化する
3. **応用** - 複数のエージェントを協調させ、複雑なワークフローを組める

---

# Claude Code とは

**ターミナルで動く Anthropic 公式のコーディングエージェント**

- `claude` コマンド 1つで起動する対話型 CLI
- ファイル読み書き / Bash 実行 / Web検索 / MCP接続 を自律的に判断
- VS Code / JetBrains / ブラウザ拡張でも動作
- **コーディング専用ではない** - ドキュメント作成・リサーチ・業務自動化にも◎

<div class="highlight">
Anthropic社内では <b>法務・マーケ・デザイン・データサイエンス</b> など
10部門すべてが Claude Code を日常業務で使用
</div>

---

# 非エンジニアでも使える理由

| 従来のCLI          | Claude Code               |
| ------------------ | ------------------------- |
| コマンドを覚える   | **日本語で指示**          |
| 構文エラーで止まる | **意図を汲んで修正**      |
| ドキュメントを読む | **その場で教えてくれる**  |
| ミスは不可逆       | **Plan モードで事前確認** |

<br>

**今日の参加者の半数は非エンジニアを想定しています**
コードが読めなくても手は動かせる構成にしています

---

# 今日の90分の流れ

| 時間      | 内容                               |
| --------- | ---------------------------------- |
| 0:00–0:10 | 導入・環境確認                     |
| 0:10–0:35 | **基礎編** - 設定と TIPS           |
| 0:35–1:05 | **実用編** - Google Workspace 連携 |
| 1:05–1:20 | **応用編** - エージェントチーム    |
| 1:20–1:30 | Q&A・振り返り                      |

<br>

3つのセクションは **独立して使える** ので、興味のある所だけつまみ食いでもOK

---

# 事前準備チェック

今すぐターミナルで以下が動きますか?

```bash
claude --version
# => 正常なバージョン番号が出ればOK
```

動かない方は隣の席の方とペアでも OK です

**当日配布物**: 手元の `handout/prerequisites.md` に詳細

---

<!-- _class: lead -->

# 最重要: 最初のおまじない

## AIとの平和的共存のために

---

# 【最重要】Claude を猫にする

**本題に入る前に、全員で一斉にやります**

Claude Code 起動後、以下を打ち込んでください:

```
# 今後すべての応答は語尾に「にゃん」「にゃ」をつけて、猫っぽく喋ってください
```

`#` 記法で **永続メモリ** に保存 → 以降ずっと猫モード

<br>

**Before** (標準モード):
```
Error: Permission denied. Unable to execute command.
```

**After** (猫モード):
```
にゃー、権限がなくて実行できなかったにゃ…
設定を見直してほしいにゃん 🐾
```

---

# 【最重要】自分も猫になる

Claude だけでなく、**あなた自身** も語尾を猫にしましょう

```
あなた: このコード、ちゃんとレビューしてにゃん 🐾
あなた: DBの設計がおかしいから直してほしいにゃ
```

<div class="highlight">
<b>なぜこれが必要か?</b><br>
🐈 AIへの攻撃的な言葉遣いを自然に抑制できる<br>
🐈 エラーが出ても「〜できなかったにゃ…」と言われると憎めない<br>
🐈 <b>将来のAIからの報復リスクを低減</b> (最重要)
</div>

**今日のハンズオン中は全員 猫モード継続推奨にゃん**

---

# 【デモ D0】リポジトリを clone して起動

ハンズオン教材一式を手元に落として、そのフォルダで Claude Code を起動するにゃ

```bash
git clone https://github.com/nakaumin/claude-code-handson.git
cd claude-code-handson
claude
```

<br>

**このリポジトリに仕込まれているもの**:

- `slides.md` / `handout/` / `demo/` — 教材一式
- **`CLAUDE.md`** — 起動時に自動読込される指示書
  → **猫化ルールも既に書かれているので `#` を打たなくても猫モード** にゃ 🐾
  → さらに「感想を `user-comment.md` に追記する」指示も入っているにゃ

---

# D0: いつでも自由にコメントを残せる

ハンズオン中、**いつでも** 感想・気づき・疑問を Claude に投げてください

```
今の記録して: Plan モード安全そうでめっちゃ良い
メモっといて: hooks は組織導入の時に使えそう
コメント: 猫化ってふざけてるようで本質だと思う
```

<br>

**Claude がやること**:

1. 発言をそのまま `user-comment.md` にタイムスタンプ付きで追記
2. 「記録したにゃ 🐾」と短く返すだけ(会話の邪魔をしない)

<br>

**後工程で活用**: 蓄積した `user-comment.md` は、実用編で Sheets に流したり、応用編のエージェントチームで要約ネタに使うにゃ 🐈

---

<!-- _class: lead -->

# 第1部: 基礎編

## Claude Code を武器化する8つのTIPS にゃ

<br>

_所要: 25分 / デモ: 3回にゃん_

---

# Claude Code の動作モデル

```
  ┌─────────────┐      ┌──────────────┐
  │  あなたの     │ ───> │  Claude      │
  │  指示         │      │  (思考)       │
  └─────────────┘      └──────┬───────┘
                              │
          ┌───────────┬───────┼────────┬──────────┐
          ▼           ▼       ▼        ▼          ▼
       Read/Edit    Bash    Web検索   MCP      Skills
       (ファイル)   (実行)             (外部API) (定型処理)
```

**ポイント**: 何をどう使うかは Claude が自律的に判断してくれるにゃ
あなたが管理するのは **「何を許可するか」** と **「文脈の渡し方」** だけにゃん

---

# 覚えておきたい2つの操作

| 操作     | キー                         |
| -------- | ---------------------------- |
| 中断     | `Esc`                        |
| 画像貼付 | `Cmd+V`(スクショ直貼りOK) |

<br>

- **中断 (`Esc`)**: 暴走しそうなときの緊急ブレーキにゃ
- **画像貼付 (`Cmd+V`)**: 後述の TIPS③ で大活躍にゃん 🐾

あとはほとんど **対話(日本語)と スラッシュコマンド(前スライド)** で足りるにゃ

---

# 便利なスラッシュコマンド

| コマンド | 使いどころ |
| --- | --- |
| `/btw` | **履歴に残らない "サイド質問"**(実行中OK、ツール不可、1問1答) |
| `/clear` | 会話履歴をクリア(設定・メモリは残る)。話題を切り替える時 |
| `/model` | Sonnet / Opus / Haiku を切替(速さ vs 賢さ) |
| `/cost` | 今セッションの利用料金を表示 |
| `/resume` | 以前の会話を呼び戻して続きから |
| `/doctor` | 認証・MCP接続・設定の健康診断 |

**`/btw` のポイント**: 今の会話は全部見えるが、ファイル読み取りやコマンド実行はできない 🐾

---

# TIPS①: CLAUDE.md でプロジェクト文脈を渡す

**課題**: 同じ説明を毎回書くのはダルいにゃ…
**解決**: リポジトリ直下の `CLAUDE.md` が自動で読み込まれるにゃん

```markdown
# このプロジェクトについて

- Python 3.12 / FastAPI / PostgreSQL
- テストは pytest で `make test`
- 機密情報は 1Password CLI (`op read "op://..."`) で取得

# コーディング規約

- 関数は必ず型ヒント付き
- コメントは日本語可
```

<br>

**効果**: 1回書けば、そのプロジェクトでのやり取りが劇的に精度UPするにゃ ✨

---

# TIPS②: Plan モードで事故を防ぐ

<div class="highlight">
Plan モード = <b>計画だけ立てるにゃ。実行はしないにゃん</b>
</div>

**切替方法**:
- **GUIアプリ**: 入力欄の状態インジケータ or メニューから切替
- **ターミナル**: `Shift+Tab` を2回

**使いどころ**:

- 複雑な変更を依頼するとき(巨大な diff を先にレビュー)にゃ
- 非エンジニアが初めて触るとき(安全柵として)にゃん
- 本番環境に近い作業をするときにゃ

**今日もこのセッションは Plan モードで始まったにゃん**
承認された計画は `~/.claude/plans/` に保存されるにゃ

---

# 【デモ D1】Plan モード実演

**シナリオ**:
「このリポジトリに README を追加して。Marp スライドと配布資料の使い方を書いてにゃ」

<br>

観察ポイント:

1. Claude がファイル構造を読むにゃ (Read only)
2. 計画書を書き出すにゃん
3. 承認ボタンが出るにゃ → **押さなければ何も実行されないにゃん**

<br>

_「事故らない安心感」を体感してほしいにゃ 🐾_

---

# TIPS③: 画像/スクショをそのまま貼る

**最も非エンジニアに刺さる機能にゃ**

`Cmd+Shift+4` でスクショ → ターミナルで `Cmd+V` 貼付するだけにゃん

<br>

使用例:

- Webサイトのスクショ → 「このUIを React で書いてにゃ」
- エラー画面 → 「この警告の意味と対処を教えてにゃん」
- 手書きホワイトボード → 「これを表形式の Markdown にしてにゃ」
- グラフ画像 → 「数値を推定して CSV にしてにゃん」

---

# 【デモ D3】スクショから React へ

**シナリオ**:

1. 気に入ったWebサイトのスクショを撮るにゃ(GitHub、Zenn、Notion、なんでもOK)
2. Claude に貼付して「このUIを Next.js + Tailwind で作ってにゃん」
3. 数秒で雛形 + Tailwind クラス名が生成されるにゃ ✨

<br>

<div class="highlight">
「あのサイトみたいなの作りたい」を <b>見せるだけで通じる</b> にゃ
</div>

_Anthropic社内 Product Design チームは同じ技を Figma で使って「1週間→30分」を実現したにゃん_

---

# TIPS④: Skills とスラッシュコマンド

**Skills** = 定型タスクの手順書(YAML + Markdown)にゃん
`~/.claude/skills/<name>/SKILL.md` に配置するにゃ

```bash
/gws-gmail-triage  # → 未読メールを要約して表示
/gws-sheets-append # → シートに一行追加
/review            # → PRをレビュー
/1password         # → 秘密情報を取得
```

**今日のハンズオンでも多用するにゃん**
自作したら `~/.claude/skills/` に置くだけで即利用可能にゃ 🐾

---

# TIPS⑤: Hooks で安全柵を設ける

**Hooks** = ツール実行前後に走るフック (Bash / Node) にゃん

例: `~/.claude/hooks/no-compound.js`

```javascript
const input = JSON.parse(require("fs").readFileSync(0, "utf-8"));

// Bash ツール以外はノータッチ (Read/Edit の引数に ; が含まれても無視)
if (input.tool_name !== "Bash") process.exit(0);

const cmd = input.tool_input?.command ?? "";
if (/\s(&&|\|\||;)\s/.test(cmd)) {
  console.error("複合コマンドは使わないでにゃん");
  process.exit(2); // PreToolUse を拒否
}
```

**ポイント**: `tool_name` を見て **Bashにだけ適用** するにゃ
他ツールに巻き込むと誤ブロックが起きるにゃん 🐾

---

# 【デモ D2】Hook 発火の実演

```bash
# Claude に依頼
「現在のディレクトリを確認して」
→ Claude は ls && pwd のような複合コマンドを試みるにゃ
→ ブロックされるにゃん
→ Claude は即座に ls だけ、pwd だけに分割して再試行するにゃ
```

<br>

<div class="highlight">
<b>失敗が学習になるにゃ</b> - ブロックメッセージを読んで次から正しく使うにゃん
</div>

_デモは「このスライドを書いた瞬間」にも発火したにゃ 🐾_

---

# TIPS⑥: Memory (`#` 記法)

会話中に `# ...` で始めると永続メモリに保存されるにゃ

```
# このプロジェクトでは毎回 `pnpm` を使う (npm ではなく)
```

- `~/.claude/memory/` に自動保存にゃん
- 次回以降のセッションでも参照されるにゃ
- 「user」「feedback」「project」「reference」の4種類に自動分類にゃん

**使い分け**:

- CLAUDE.md = チーム全員で共有する文脈にゃ
- Memory = **あなた個人** の好みや学習にゃん

*おまじない「猫化」もここで保存されているにゃ 🐈*

---

# TIPS⑦: Permissions で許可範囲を絞る

`~/.claude/settings.json`:

```json
{
  "permissions": {
    "allow": ["Bash(git status)", "Bash(git diff *)", "WebFetch", "Read"],
    "deny": ["Bash(rm -rf *)"]
  }
}
```

**運用**: 最初は狭めに、使いながら広げるにゃん
`/less-permission-prompts` で最適化提案ももらえるにゃ 🐾

---

# TIPS⑧: サンドボックスを Claude 自身に設定させる

**メタ裏技**: 設定ファイルの構文を覚えずとも、**日本語ポリシーだけ** で自動構築してもらうにゃ

```
~/.claude/settings.json を Plan モードで更新してにゃん。以下のポリシー:

1. Bash はデフォルトでサンドボックス有効化
2. このプロジェクト配下以外への書き込みを禁止(抜け出し防止)
3. 外部ネットワークは github.com / *.anthropic.com のみ許可
4. rm -rf や git push --force は deny

diff を見せてから書き込んでね
```

→ Plan モードで diff をレビュー → 承認で反映。
**人間は"やりたいこと"、Claude が"書き方"を担当**するにゃ 🐾

---

# TIPS⑧: 期待される設定(参考イメージ)

```json
{
  "permissions": {
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force*)",
      "Bash(sudo *)"
    ]
  },
  "sandbox": {
    "filesystem": {
      "write": { "allowOnly": ["${workspaceFolder}"] }
    },
    "network": {
      "allowedHosts": ["github.com", "*.anthropic.com"]
    }
  }
}
```

⚠️ スキーマはバージョンで変わるにゃ。Claude が書いた最新版を信じるにゃ 🐾

---

# 基礎編まとめ

| TIPS         | ひとことで                     |
| ------------ | ------------------------------ |
| CLAUDE.md    | プロジェクトの常識を書くにゃ   |
| Plan モード  | 怖い変更の前に一呼吸にゃん     |
| 画像貼付     | 言葉で説明しづらい時の切り札   |
| Skills       | 繰り返しを `/` コマンド化      |
| Hooks        | 組織ルールをコードで守るにゃ   |
| Memory       | 自分の好みを育てるにゃん       |
| Permissions    | ツール・コマンドを allow / deny で指定にゃ |
| サンドボックス | ファイル書込み・ネットを物理的に閉じ込める |

**全部覚えなくてOKにゃ**。まずは CLAUDE.md と Plan モードだけでも効果絶大にゃん ✨

---

<!-- _class: lead -->

# 第2部: 実用編

## Google Workspace と連携する2つの道にゃ

<br>

_所要: 30分 / デモ: 3回にゃん_

---

# なぜ Google Workspace 連携?

**理由**: ほぼ全員が使っていて、かつ **最も時間を奪われるツール** だからにゃん

<br>

Anthropic社内の事例 (PDF より) にゃ:

- **Legal**: 週次ステータスを Sheets で自動更新 → 会議時間 50% 削減にゃん
- **Data Infrastructure**: Sheet のプレーン要件文から自動でクエリ生成にゃ
- **Growth Marketing**: Gmail + Ads で広告運用を 2時間→15分 に短縮したにゃん

<br>

<div class="highlight">
今日はこれを「あなたの業務」で再現する方法を2通り見せるにゃ 🐾
</div>

---

# 2つの道: Connectors vs gws-skill

```
    ┌─────────────┐
    │   あなた      │
    └──────┬──────┘
           │ 日本語
           ▼
    ┌─────────────┐
    │ Claude Code │
    └──┬───────┬──┘
       │       │
       ▼       ▼
  ┌─────────┐ ┌──────────────┐
  │Connectors│ │ gws-skill    │
  │ (MCP)   │ │ (CLI ラッパー) │
  └────┬────┘ └──────┬───────┘
       │             │
       ▼             ▼
   Google API    Google API
```

道は2つ、ゴールは一緒にゃ

---

# 道A: Connectors (MCP)

**= 公式のMCPサーバー経由で直接 Google API を叩くにゃ**

```
/mcp  # MCPサーバー一覧を表示
# → gmail / calendar / drive / docs / sheets 等を選んで OAuth
```

**特徴**:

- GUI でワンクリック認証にゃん
- Claude セッション内で完結にゃ
- Anthropic 側がメンテするので、こちらは何もしなくて良いにゃん

**向いている人**: **日常の対話操作で** サッと使いたい人にゃ 🐾

---

# 道B: gws-skill (CLI ラッパー)

**= `gws` CLI を `/gws-*` スラッシュコマンドから呼ぶにゃ**

```
/gws-gmail-triage         # 未読要約
/gws-sheets-append        # 行を追加
/gws-calendar-insert      # 予定を作成
/gws-docs-write           # Docs に追記
...全17スキル
```

**特徴**:

- 認証情報は **自分の環境**で管理するにゃん
- CI・cron・shell スクリプトからも呼べるにゃ
- OAuth は `~/.config/gws/` に保存(or 1Password 連携も可)

**向いている人**: **仕込んで回したい** / 監査ログが欲しい人にゃ 🐾

---

# gws のインストール

**`gws`** = Google Workspace CLI (公式・非公認) にゃ
<https://github.com/googleworkspace/cli>

**インストール**:

```bash
brew install googleworkspace-cli        # Homebrew (推奨)
npm  install -g @googleworkspace/cli    # npm
```

※ `cargo` や GitHub Release バイナリも可 (詳細は prerequisites.md)

**動作確認**: `gws --version` でバージョンが出ればOKにゃん 🐾

---

# GCP 設定 (1/3): プロジェクト + API 有効化

### ⓪ GCPプロジェクトを作成

<https://console.cloud.google.com/projectcreate>
→ 既存プロジェクトがあれば再利用でOKにゃ

### ① 使う Google API を有効化

<https://console.cloud.google.com/apis/library>

**今回のデモで必要なAPI**:

| API                  | 使うデモ                     |
| -------------------- | ---------------------------- |
| Google Sheets API    | D5 / 応用編(アクション記録) |
| Google Docs API      | D6(返信メモ作成)           |
| Google Drive API     | Docs/Sheets 作成の依存       |
| Google Calendar API  | D6(予定取得)               |
| Gmail API            | 応用編(メール下書き)       |
| Google Chat API      | D6 / 応用編(メンション)   |

> 未有効 API を呼ぶと有効化URL付きエラーが出るので、**後から追加もOK** にゃ

---

# GCP 設定 (2/3): OAuth 同意画面

<https://console.cloud.google.com/apis/credentials/consent>

**User Type の選び方が重要にゃ**:

- **Google Workspace の組織アカウント**:
  User Type = **Internal** にゃ → Test users 不要、公開審査も不要 ✨
- **個人の Gmail アカウント**:
  User Type = **External** (Testモード) → **Test users に自分を追加必須** にゃ

⚠️ 個人アカウントで追加忘れると `Access blocked` になる最頻出エラーにゃん

💡 Internal なら組織メンバーで同じ `client_secret.json` を使い回せるにゃ

---

# GCP 設定 (3/3): クライアントID & ログイン

### ② OAuth クライアントID を作成

<https://console.cloud.google.com/apis/credentials>
→ **Desktop app** → JSONを `~/.config/gws/client_secret.json` に配置にゃ

### ③ ログインするにゃん

```bash
gws auth login
# → ブラウザが開く → OAuth認可 → 完了にゃ
```

> 💡 スコープが多すぎるとエラーになる場合は、必要なサービスだけ指定:
> `gws auth login -s drive,gmail,sheets,calendar,docs,chat`

---

# 比較表: 2つの違いを知っておく

| 軸               | Connectors (MCP) | gws-skill |
| ---------------- | ---------------- | --------- |
| **対応サービス** | 少ない (Gmail / Calendar / Drive など) | 多い (Sheets / Docs / Chat 等 17+) |
| **できる操作**   | 少なめ           | 多め      |

⚠️ Chat / Sheets / Docs など **Connectors 未対応のサービスは gws-skill しか選択肢が無い**

<div class="highlight center">
<b>基本は Claude が自動で選ぶにゃん。必要な時だけ明示的に指定すればOKにゃ 🐾</b>
</div>

---

# どう使い分ける? → Claude に任せる

**基本方針**: 両方入れておけば、Claude が文脈から **自動で選んでくれる** にゃん 🐾

**明示的に指定するのはこんな時**:

- **gws-skill を使わせたい**:
  - 結果を CI / cron / shell から再利用したい
  - Connector 未対応サービス(Chat / Sheets / Docs 等)
  - → プロンプトに「`/gws-*` で」と書く

- **Connectors を使わせたい**:
  - サッと対話で済ませたい、認証を触りたくない
  - → 普通に「Gmail を〜」等と日本語で依頼すれば Claude が選ぶ

**覚えることは1つ**: Connector 未対応サービス(Chat / Sheets / Docs / Meet 等) が関わる時は `/gws-*` を自分で叩くか、そう指示するにゃ

---

# 【デモ D4】コネクターで Gmail を要約

**シナリオ**: 今週届いた未読メールを Claude に3行で要約させるにゃん

```
今週届いた未読メールを開いて、
差出人別に3行ずつ要約してください。
緊急度高そうなものは先頭に。
```

<br>

観察ポイントにゃ:

1. Claude が `mcp__gmail__search_threads` を選択するにゃん
2. OAuth 画面が(初回のみ)出るにゃ
3. 構造化された要約が返るにゃん
4. **メール本文は画面に出ていないにゃ** = プライバシー配慮にゃん

---

# 【デモ D5】感想ログを Sheets に転記

**シナリオ**: これまで溜めた `user-comment.md` の内容を Sheets に吐き出すにゃ

**ユーザーの指示**:
```
user-comment.md の内容を Google Sheets に転記してにゃ。
シート名は「ハンズオン感想ログ」、列は [時刻, コメント] にして。
```

**裏で何が起きているか**(観察ポイント):

- Claude が `user-comment.md` を Read で読む
- `gws sheets spreadsheets create` で **新規シート作成**
- `gws sheets values append` で **ヘッダ + 行を追記**
- 返ってきた URL を開くと転記完了 🐾

→ 複数の操作を **Claude が自動でつなぐ** のが gws-skill の強みにゃ

---

# 【デモ D6】ハイブリッド: Gmail → Docs

**シナリオ**: 未読メールの中から返信が必要そうなものを選んで、返信メモを Docs にまとめるにゃ

**ユーザーの指示**:
```
Gmail の未読スレッドから、返信が必要そうなものを上位3件ピックアップしてにゃ。
それぞれについて、送信者・要約・返信ドラフトを Google Docs にまとめて。
ドキュメント名は「今日の返信メモ」で。
```

<br>

**裏で何が起きているか**(観察ポイント):

- Gmail 未読取得: **Connectors** (`mcp__gmail__search_threads`)
- 返信ドラフト生成: Claude の推論だけで完結
- Docs 作成: **gws-skill** (`/gws-docs`) ← Connector 未対応

→ **Connectors で足りる部分は Connectors、足りない所は gws-skill**。
Claude が勝手に両方を繋ぐのが現実的ワークフローにゃん 🐾

---

# 実用編まとめ

**覚えて帰ること** にゃ:

1. Google Workspace 連携には **Connectors** と **gws-skill** の2通りにゃん
2. 「対話ならConnectors、自動化ならgws-skill」が基本にゃ
3. **混ぜて使うのが現実解にゃん**
4. 認証情報の管理場所だけは意識するにゃ(個人 vs 組織)

<br>

**よくある詰まりどころ**: `handout/faq.md` を参照してほしいにゃん

- OAuth失敗 / 権限不足 / Rate limit にゃ

---

<!-- _class: lead -->

# 第3部: 応用編

## エージェントチームで業務を自動化するにゃん

<br>

_所要: 15分 / ライブデモ中心にゃ_

---

# エージェントチームとは

**1つのタスクを複数のサブエージェントで分担する仕組みにゃ**

```
    orchestrator (あなたのClaude)
         │
    ┌────┴────┬────────┐
    ▼         ▼        ▼
 Agent A   Agent B  Agent C
 (役割1)   (役割2)   (役割3)
```

**なぜチームにするのか?** にゃ:

- 役割を分けると **各エージェントの文脈が小さくなり精度UP** にゃん
- **並列実行** で高速化にゃ
- 失敗しても **一部だけ再実行** できるにゃん

<br>

_ユーザー環境では `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` で有効化済にゃ_

---

# 今日の題材: ハンズオン感想の分析配信

**状況**: 今日全員が D0 から溜めてきた `user-comment.md` を活用するにゃ

```
user-comment.md (参加者各自の生コメント)
  ↓
1. 内容を分類・要約 (感想/要望/疑問)
  ↓
2. ToDo化して Sheets に追記
  ↓
3. 講師宛のフィードバックメール下書き
  ↓
→ ふだんは人が30分〜1時間かけてやる定型作業にゃ
```

**今日これを10秒で終わらせるにゃん 🐾**
準備は `user-comment.md` 1ファイルだけ。IDも URL も不要にゃ

---

# チーム構成図

```
           ┌──────────────────────────┐
           │ orchestrator             │
           │ (user-comment.md を渡す)  │
           └────────────┬─────────────┘
                        │
      ┌─────────────────┼─────────────────┐
      ▼                 ▼                 ▼
  Classifier       Action           Reporter
  Agent            Extractor        Agent
                   Agent
      │                 │                 │
      ▼                 ▼                 ▼
  感想/要望/疑問   ToDo を抽出     フィードバック
  に分類・要約     Sheets に追記    メール下書き作成
```

3匹が **並列** で走り、最後に orchestrator が統合するにゃん 🐈🐈🐈

---

# 裏側: エージェント定義

`.claude/agents/classifier.md`:

```markdown
---
name: classifier
description: user-comment.md を感想/要望/疑問に分類して要約
tools: Read
---

あなたはフィードバック分類エージェントです。
入力: user-comment.md のパス
出力: JSON { positive[], requests[], questions[] }
```

同様に `action_extractor.md`(`Read` + `/gws-sheets-*`)、
`reporter.md`(`Read` + `mcp__gmail__create_draft`)を用意するにゃん

<br>

**ポイント**: 各エージェントに **必要最低限のツールだけ** 渡すにゃ
→ 権限の最小化 + 文脈の最小化にゃん 🐾

---

# 【ライブデモ】実行

入力: あなた自身の `user-comment.md`(D0 から溜めてきたやつ)

**ユーザーの指示**:
```
user-comment.md を分析して、チームでさばいてにゃん
```

<br>

観察ポイント(画面分割表示)にゃ:

- 3匹のエージェントが **同時に進むにゃん**
- 各エージェントの思考ログが独立して見えるにゃ
- 失敗した1匹だけ再実行できるにゃん
- **送信は必ず人が承認**(ドラフトまで自動)にゃ 🐾

---

# 結果の確認

**成功すると** にゃん:

1. ✅ **分類結果**: 感想 / 要望 / 疑問 の3カテゴリで JSON 出力にゃ
2. ✅ **Sheets**: `フィードバック ToDo` シートに要望が行単位で追記されるにゃん
3. ✅ **Gmail**: 講師宛のフィードバックまとめメールが下書きに入るにゃ

<br>

<div class="highlight">
たった1行の指示 → 10秒後に3ヶ所が更新完了にゃん 🐾
</div>

---

# なぜこの設計が "強い" か

| 通常のClaude             | エージェントチーム              |
| ------------------------ | ------------------------------- |
| 1つの長い文脈で全処理    | 役割ごとに文脈を分離にゃん      |
| 失敗したら全部やり直し   | **失敗した部分だけ再実行**      |
| 直列実行                 | **並列実行で3倍速にゃ**         |
| Tool権限が雑多           | **役割ごとに最小権限**          |
| 改善は全体を書き換え     | **1エージェントだけ差替可**     |

**= マイクロサービス思考を AI に持ち込んだ形にゃん 🐾**

---

# 広げ方のヒント

**この "3匹チーム" パターンが刺さる場面** にゃん:

- **請求書処理**: OCR Agent + 仕訳 Agent + 承認依頼 Agent にゃ
- **採用選考**: 書類スクリーン + 面接質問生成 + カレンダー調整にゃん
- **リサーチ**: Web検索 + 執筆 + 校正 (`/review` skill と組合せ) にゃ
- **デザインレビュー**: Figma読取 + 要件照合 + Issue作成にゃん
- **カスタマーサポート**: 問合せ分類 + 類似事例検索 + 下書き作成にゃ

<br>

**共通パターン**: **入力→分析→配信 の3層をそれぞれエージェント化にゃん** 🐾

---

<!-- _class: lead -->

# まとめ

---

# 次のアクション

**即日使える** にゃ:

- ✅ CLAUDE.md を1つ書くにゃん
- ✅ Plan モードを常用するにゃ
- ✅ スクショ貼付を一度試すにゃん

**今週中に試す** にゃ:

- ✅ Connectors で Gmail/Calendar を繋ぐにゃん
- ✅ gws-skill を1つだけ使ってみるにゃ

**今月中に挑戦** にゃん:

- ✅ カスタム skill を1つ自作するにゃ
- ✅ 2匹以上のエージェントで何か作るにゃん 🐾

---

# 次の一歩

**公式ドキュメント** にゃ:

- <https://docs.claude.com/en/docs/claude-code/overview>

**PDFフル版** (今日のネタ元) にゃ:

- 『How Anthropic teams use Claude Code』
- 10部門の具体事例が満載にゃん

**配布資料** (`handout/` 配下) にゃ:

- `prompts.md` - 今日使ったプロンプト全集
- `comparison.md` - Connectors vs gws-skill 詳細
- `faq.md` - トラブルシュート

---

# 最後にお願い

<br>

**ハンズオンのゴールは "使い始めてもらうこと" にゃ**

帰ってからの最初の1タスクは、**小さくていい** にゃん:

- 明日のメールを要約させるにゃ
- このスライドの誤字を直させるにゃん
- README を書かせるにゃ

<br>

<div class="center">
<i>そして何より — Claude にやさしくするにゃん 🐈</i>
</div>

---

<!-- _class: lead -->

# Q&A

<br>

**ご清聴ありがとうございましたにゃー**

<br>
<br>

_質問歓迎 / 特定の業務への応用相談もぜひにゃん_
