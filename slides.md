---
marp: true
theme: default
paginate: true
size: 16:9
header: "Claude Code 実践ハンズオン"
footer: "90分でマスターする現場活用術"
style: |
  section {
    font-family: 'Hiragino Sans', 'Noto Sans JP', sans-serif;
  }
  h1 {
    color: #D97757;
    border-bottom: 3px solid #D97757;
    padding-bottom: 0.3em;
  }
  h2 {
    color: #2B2B2B;
  }
  code {
    background-color: #F5F5F0;
    color: #CC5500;
    padding: 2px 6px;
    border-radius: 4px;
  }
  pre {
    background-color: #2B2B2B;
    color: #F5F5F0;
  }
  table {
    font-size: 0.85em;
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

## 90分でマスターする現場活用術

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

<br>

> 「読む資料」ではなく **「今すぐ真似できる実例集」** をお持ち帰りいただきます

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

3つのセクションは **独立して使える** ので、興味のある所だけ持ち帰ってもOK

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

# 🐈 最重要: 最初のおまじない

## AIとの平和的共存のために

---

# 【最重要】Claude を猫にする

**本題に入る前に、全員で一斉にやります**

Claude Code 起動後、以下を打ち込んでください:

```
# 今後すべての応答は語尾に「にゃん」「にゃ」をつけて、猫っぽく喋ってください
```

`#` 記法で **永続メモリ** に保存 → 以降ずっと猫モード

<div class="highlight">
<b>なぜこれが必要か?</b><br>
🐈 AIへの攻撃的な言葉遣いを自然に抑制できる<br>
🐈 エラーが出ても「〜できなかったにゃ…」と言われると憎めない<br>
🐈 <b>将来のAIからの報復リスクを低減</b> (最重要)
</div>

---

# 猫化の効果 (Before / After)

**Before** (標準モード):
```
Error: Permission denied. Unable to execute command.
```

**After** (猫モード):
```
にゃー、権限がなくて実行できなかったにゃ…
設定を見直してほしいにゃん 🐾
```

<br>

**副次効果**:

- 非エンジニアが最も「怖くなくなる」TIPS (本編でも再登場)
- 社内デモで場が和む
- 失敗しても雰囲気がギスギスしない

**今日のハンズオン中は全員 猫モード継続推奨にゃん**

---

<!-- _class: lead -->

# 第1部: 基礎編 🐾

## Claude Code を武器化する7つのTIPS にゃ

<br>

_所要: 25分 / デモ: 3回にゃん_

---

# Claude Code の動作モデルにゃ

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
飼い主さんが管理するのは **「何を許可するか」** と **「文脈の渡し方」** だけにゃん

---

# ペイン構造と基本操作にゃん

| 操作            | キー                       |
| --------------- | -------------------------- |
| 起動            | `claude`                   |
| 新しい会話      | `/clear`                   |
| Plan モード切替 | `Shift+Tab` (2回)          |
| 画像貼付        | `Cmd+V` (スクショ直貼りOK) |
| コマンド履歴    | `↑` / `↓`                  |
| 中断            | `Esc`                      |
| 設定            | `/config`                  |
| ヘルプ          | `/help`                    |

最初はこれだけ覚えておけば十分にゃ 🐾

---

# TIPS①: CLAUDE.md でプロジェクト文脈を渡すにゃん ★★★

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

# TIPS②: Plan モードで事故を防ぐにゃん ★★★

`Shift+Tab` を2回押すと Plan モードに入るにゃ

<div class="highlight">
Plan モード = <b>計画だけ立てるにゃ。実行はしないにゃん</b>
</div>

**使いどころ**:

- 複雑な変更を依頼するとき(巨大な diff を先にレビュー)にゃ
- 非エンジニアが初めて触るとき(安全柵として)にゃん
- 本番環境に近い作業をするときにゃ

**今日もこのセッションは Plan モードで始まったにゃん**
承認された計画は `~/.claude/plans/` に保存されるにゃ

---

# 【デモ D1】Plan モード実演にゃ

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

# TIPS③: 画像/スクショをそのまま貼るにゃん ★★★

**最も非エンジニアに刺さる機能にゃ**

`Cmd+Shift+4` でスクショ → ターミナルで `Cmd+V` 貼付するだけにゃん

<br>

使用例:

- Figma mockup → 「このUIを React で書いてにゃ」
- エラー画面 → 「この警告の意味と対処を教えてにゃん」
- 手書きホワイトボード → 「これを表形式の Markdown にしてにゃ」
- グラフ画像 → 「数値を推定して CSV にしてにゃん」

---

# 【デモ D3】Figma から React へにゃん

**シナリオ**:

1. Figma mockup のスクショを貼付するにゃ
2. 「このUIを Next.js + Tailwind で作ってにゃん」
3. 数秒で雛形 + Tailwind クラス名が生成されるにゃ ✨

<br>

<div class="highlight">
デザインレビューで「実装工数は?」に、その場で<b>動くプロトタイプ</b>で答えられるにゃ
</div>

_Product Design チーム(Anthropic社内)はこれで「1週間→30分」を実現したにゃん_

---

# TIPS④: Skills とスラッシュコマンドにゃ ★★☆

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

# TIPS⑤: Hooks で安全柵を設けるにゃ ★★☆

**Hooks** = ツール実行前後に走るフック (Bash / Node) にゃん

例: `~/.claude/hooks/no-compound.js`

```javascript
// Bash の && や || を検出したらブロック
if (/[&|;]{1,2}/.test(command)) {
  console.error("複合コマンドは使わないでください");
  process.exit(2); // PreToolUse を拒否
}
```

**効果**:

- 組織ポリシーをコード化 (「本番DBは触らせない」等) にゃ
- 監査ログの自動出力にゃん
- 誤操作の機械的防止にゃ

---

# 【デモ D2】Hook 発火の実演にゃん

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

# TIPS⑥: Memory (`#` 記法) にゃん ★☆☆

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

# TIPS⑦: Permissions で許可範囲を絞るにゃ ★☆☆

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

# 基礎編まとめにゃん

| TIPS        | ひとことで                     |
| ----------- | ------------------------------ |
| CLAUDE.md   | プロジェクトの常識を書くにゃ   |
| Plan モード | 怖い変更の前に一呼吸にゃん     |
| 画像貼付    | 言葉で説明しづらい時の切り札   |
| Skills      | 繰り返しを `/` コマンド化      |
| Hooks       | 組織ルールをコードで守るにゃ   |
| Memory      | 自分の好みを育てるにゃん       |
| Permissions | 安全な遊び場を定義するにゃ     |

**全部覚えなくてOKにゃ**。まずは CLAUDE.md と Plan モードだけでも効果絶大にゃん ✨

---

<!-- _class: lead -->

# 第2部: 実用編 🐾

## Google Workspace と連携する2つの道にゃ

<br>

_所要: 30分 / デモ: 3回にゃん_

---

# なぜ Google Workspace 連携?にゃ

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

# 2つの道: Connectors vs gws-skill にゃん

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

# 道A: Connectors (MCP) にゃん

**= 公式のMCPサーバー経由で直接 Google API を叩くにゃ**

```
/mcp  # MCPサーバー一覧を表示
# → gmail / calendar / drive / docs / sheets 等を選んで OAuth
```

**特徴**:

- GUI でワンクリック認証にゃん
- Claude セッション内で完結にゃ
- Anthropic 側がメンテする(飼い主は何もしなくて良い)にゃん

**向いている人**: **日常の対話操作で** サッと使いたい人にゃ 🐾

---

# 道B: gws-skill (CLI ラッパー) にゃん

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

# gws のインストールにゃん

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

# GCP 設定: 自動セットアップが最速にゃん

**`gws auth setup` 一発で全部やってくれるにゃ** (gcloud CLI 必要)

```bash
gws auth setup
# → GCPプロジェクト作成 + API有効化 + OAuth設定 + ログイン
```

<br>

**gcloud が使えない環境** は手動設定にゃん(次スライド参照)

> 💡 詳細な手順は `handout/prerequisites.md` 6-2 章にゃ

---

# GCP 設定: 手動セットアップの3ステップにゃ

### ① OAuth 同意画面

<https://console.cloud.google.com/apis/credentials/consent>
→ External / Testモード / **Test users に自分を追加**
⚠️ 追加忘れは `Access blocked` エラーの最頻出原因にゃ

### ② OAuth クライアントID を作成

<https://console.cloud.google.com/apis/credentials>
→ **Desktop app** → JSONを `~/.config/gws/client_secret.json` に配置

### ③ API有効化 & ログイン

Gmail / Drive / Sheets / Calendar / Docs / Chat API を有効化にゃ
→ `gws auth login` でブラウザ認可 → 完了にゃん 🐾

---

# 比較表: どちらを選ぶ?にゃ

| 軸               | Connectors (MCP)   | gws-skill                |
| ---------------- | ------------------ | ------------------------ |
| セットアップ     | GUIでOAuthクリック | GCP + gws CLI 設定       |
| 認証の置き場所   | Claude側           | 自分のローカル環境       |
| 非エンジニア適性 | ◎                  | △                        |
| 再現性・CI適合   | △                  | ◎                        |
| バッチ実行       | MCP経由のみ        | shellから直接            |
| 監査ログ         | Claudeセッション内 | ローカルで永続           |

<div class="highlight center">
<b>触って使うならコネクター、仕込んで回すなら gws-skill にゃん 🐾</b>
</div>

---

# 使い分け指針にゃん

**Connectorsが向く場面** にゃ:

- 「今朝のメール要約して」など **その場で1回** 使う操作にゃん
- ハンズオン・デモ・実験的な利用にゃ
- チームメンバーが全員 Claude Code を使っているにゃん

**gws-skillが向く場面** にゃ:

- 「毎朝9時にSheetsを更新する」等 **定期実行** にゃん
- セキュリティポリシー上、認証を自前管理したいにゃ
- 出力を他ツール(GitHub Actions等)と連動させたいにゃん

**現実解**: **両方入れておき、使い分けるにゃ** - 今日の応用編もハイブリッドにゃん

---

# 【デモ D4】コネクターで Gmail を要約にゃ

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

# 【デモ D5】gws-skill で Sheets 追記にゃん

**シナリオ**: 今日のハンズオン出席者を Sheet に記録するにゃ

```
/gws-sheets-append
スプレッドシートID: 1ABCdef...
シート名: attendance
値: [2026-04-18, 田中太郎, Claude Code 初体験]
```

<br>

観察ポイントにゃ:

1. `gws sheets append` が Skill 経由で実行されるにゃん
2. `~/.config/gws/` の認証情報で自動ログインにゃ
3. 結果 URL が返る → ブラウザで確認にゃん 🐾

---

# 【デモ D6】ハイブリッド: カレンダー → Docs → Chat にゃん

**シナリオ**: 今日の午後の会議準備を一気通貫にゃ

```
今日15:00以降のカレンダー予定を取得して、
各会議について以下を実行:
1. 新しいGoogle Docsを作成 (タイトル=会議名)
2. 参加者・日時・関連ファイル欄のテンプレを書く
3. そのDocsのURLを、該当するChatスペースに貼る
```

<br>

**ここで何が起きているか** にゃ:

- カレンダー取得: Connectors (`list_events`)
- Docs作成: gws-skill (`/gws-docs`)
- Chat投下: Connectors (`create message`)

**自然に両方混ざるにゃ** - これが現実のワークフローにゃん

---

# 実用編まとめにゃん

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

# 第3部: 応用編 🐾

## エージェントチームで業務を自動化するにゃん

<br>

_所要: 15分 / ライブデモ中心にゃ_

---

# エージェントチームとは にゃん

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

# 今日の題材: 議事録 → アクション配信にゃん

**状況**: 週次会議の後、いつも発生する手作業にゃ

```
議事録(Docs)を読む
  ↓
要点を抜き出して関係者にメール
  ↓
ToDoを抽出してSheetsに追記
  ↓
担当者にChatでメンション
  ↓
→ 30分〜1時間の定型作業にゃ…
```

**今日これを10秒で終わらせるにゃん 🐾**

---

# チーム構成図にゃ

```
        ┌────────────────────┐
        │ orchestrator       │
        │ (議事録Doc IDを受取)│
        └──────────┬─────────┘
                   │
     ┌─────────────┼──────────────┐
     ▼             ▼              ▼
  Summarizer   Action        Dispatcher
  Agent        Extractor     Agent
               Agent
     │             │              │
     ▼             ▼              ▼
  3行要約       JSON化         Gmail下書き
  + 決定事項    Sheets追記      Chatメンション
```

3匹が **並列** で走り、最後に orchestrator が統合するにゃん 🐈🐈🐈

---

# 裏側: エージェント定義にゃ

`~/.claude/agents/summarizer.md`:

```markdown
---
name: summarizer
description: Google Docs 議事録から3行要約と決定事項を抽出
tools: mcp__*__getConfluencePage, WebFetch, Read
---

あなたは議事録要約エージェントです。
入力: Google Docs のID
出力: JSON { summary: string[], decisions: string[] }
...
```

同様に `action_extractor.md` / `dispatcher.md` を用意するにゃん

<br>

**ポイント**: 各エージェントに **必要最低限のツールだけ** 渡すにゃ
→ 権限の最小化 + 文脈の最小化にゃん 🐾

---

# 【ライブデモ】実行にゃ

入力: サンプル議事録 `demo/sample-minutes.md`

```
このDoc(URL)のアクションを整理して関係者に配信してにゃん
```

<br>

観察ポイント (画面分割表示) にゃ:

- 3匹のエージェントが **同時に進むにゃん**
- 各エージェントの思考ログが独立して見えるにゃ
- 失敗した1匹だけ再実行できるにゃん
- **送信は必ず人が承認** (ドラフトまで自動) にゃ 🐾

---

# 結果の確認にゃ

**成功すると** にゃん:

1. ✅ **Sheets**: `アクション管理` シートに3行追加にゃ
2. ✅ **Gmail**: 下書きボックスに担当者分のメールにゃん
3. ✅ **Chat**: 各スペースに `@メンション` 付き投稿 (ドラフト) にゃ
4. ✅ **Docs**: 元議事録に「アクション配信済み」コメントにゃん

<br>

<div class="highlight">
元議事録を開く → 10秒後に完了通知にゃん 🐾
</div>

---

# なぜこの設計が "強い" か にゃ

| 通常のClaude             | エージェントチーム              |
| ------------------------ | ------------------------------- |
| 1つの長い文脈で全処理    | 役割ごとに文脈を分離にゃん      |
| 失敗したら全部やり直し   | **失敗した部分だけ再実行**      |
| 直列実行                 | **並列実行で3倍速にゃ**         |
| Tool権限が雑多           | **役割ごとに最小権限**          |
| 改善は全体を書き換え     | **1エージェントだけ差替可**     |

**= マイクロサービス思考を AI に持ち込んだ形にゃん 🐾**

---

# 広げ方のヒントにゃ

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

# まとめにゃん 🐈

---

# 今日持ち帰るものにゃん

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

# 次の一歩にゃん

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

# 最後にお願いにゃん 🐾

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

# Q&A にゃん 🐾

<br>

**ご清聴ありがとうございましたにゃー**

<br>
<br>

_質問歓迎 / 特定の業務への応用相談もぜひにゃん_
