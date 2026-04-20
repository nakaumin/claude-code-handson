# 事前準備リスト

ハンズオン当日までに以下をご準備ください。**★印は必須、☆印は任意**です。

---

## ★ 1. Claude Code のインストール

```bash
npm i -g @anthropic-ai/claude-code
```

**動作確認**:
```bash
claude --version
```
バージョン番号が表示されればOK。

**依存**: Node.js 20 以上
- `node --version` で確認
- 未インストールなら [nodejs.org](https://nodejs.org/) または `nvm` 経由で

---

## ★ 2. Anthropic アカウントでログイン

```bash
claude
```
初回起動で OAuth ログインが走ります。ブラウザで Anthropic アカウントにサインイン。

**法人利用の方**: 管理者から組織アカウントへの招待を受けていることをご確認ください。

---

## ★ 3. Google アカウント

実用編で Gmail / Calendar / Drive / Sheets に接続します。

**推奨**: ハンズオン専用の**使い捨てGoogleアカウント**を用意
(業務データに触れるのが不安な方向け)

---

## ★ 4. ターミナル / 端末

- macOS: ターミナル.app または iTerm2
- Windows: WSL2 + Windows Terminal
- Linux: 任意

**注**: VS Code 内蔵ターミナルや JetBrains 統合ターミナルでも動作します

---

## ★ 5. ハンズオン教材リポジトリを clone

**当日の D0 デモで全員一斉に使います**。事前に clone できればスムーズにゃ。

```bash
git clone https://github.com/nakaumin/claude-code-handson.git
cd claude-code-handson
claude    # ここで起動
```

**同梱されているもの**:
- `slides.md` — Marp スライド本体
- `handout/` — 配布資料 (このファイルも含む)
- `demo/` — 応用編デモ用サンプル議事録
- **`CLAUDE.md`** — 起動時に自動読込、感想を対話的に `user-comment.md` に追記してくれる

> 💡 `user-comment.md` はあなた専用のログファイル。あとで見返せるようになっています。

---

## ☆ 6. gws CLI + Google Cloud 設定 (実用編 道B 体験者向け)

### 6-1. gws CLI のインストール

`gws` は Google Workspace CLI。公式(非公認)ツール。
<https://github.com/googleworkspace/cli>

**インストール方法** (好きな方法で):

```bash
# Homebrew (macOS/Linux 推奨)
brew install googleworkspace-cli

# npm (Node.js 環境がある方)
npm install -g @googleworkspace/cli

# Rust cargo
cargo install --git https://github.com/googleworkspace/cli --locked

# GitHub Release からバイナリDL
# https://github.com/googleworkspace/cli/releases
```

**動作確認**:
```bash
gws --version      # => gws 0.22.5 など
gws --help         # サービス一覧が出ればOK
```

---

### 6-2. Google Cloud Console の設定

`gws` は OAuth で Google API を叩くため、**GCP プロジェクト** と **OAuth クライアントID** が必要です。以下の5ステップで完了します。

**① GCPプロジェクトを作成** ← まずこれが必要

<https://console.cloud.google.com/projectcreate>

- プロジェクト名を入力(例: `my-gws-cli`)
- 組織が紐づいている場合は適切な組織を選択
- 既存プロジェクトがあれば再利用でOK

作成後、プロジェクトIDが発行される(これ以降のURLで `?project=<ID>` に使う)

**② OAuth 同意画面を設定**

<https://console.cloud.google.com/apis/credentials/consent>

**User Type の選び方が重要にゃ**:

| あなたのアカウント | User Type | Test users |
|---|---|---|
| **Google Workspace (会社/組織)** | **Internal** | 不要 ✨ |
| 個人の Gmail | External (Testモード) | 必須 ⚠️ |

- **Internal を選べる場合は断然これが楽**: 公開審査不要、Test users 登録不要、組織メンバーなら全員使える
- **Externalを選んだ場合のみ**:
  - アプリ情報(アプリ名・サポートメール・デベロッパーメール)を入力
  - **Test users に自分のGoogleアカウントを必ず追加** (これ忘れると `Access blocked` エラー)

> 💡 一度 Internal で作成すれば、その組織内の他のメンバーも同じ client_secret.json を使い回せます

**③ OAuth クライアントIDを作成**

<https://console.cloud.google.com/apis/credentials>

- 「認証情報を作成」→「OAuthクライアントID」
- **アプリケーションの種類**: **Desktop app**
- 作成後、「JSONをダウンロード」

ダウンロードした JSON を `~/.config/gws/client_secret.json` に配置:

```bash
mkdir -p ~/.config/gws
mv ~/Downloads/client_secret_*.json ~/.config/gws/client_secret.json
```

**④ 使う API を有効化**

<https://console.cloud.google.com/apis/library> から有効化します。

**今回のデモで使うAPI**(どのデモで必要かの対応表):

| API | 必要なデモ | 用途 |
|---|---|---|
| Google Sheets API | D5 / 応用編 | スプレッドシート読書き |
| Google Docs API | D6 / 応用編 | ドキュメント作成・読取 |
| Google Drive API | D5 / D6 / 応用編 | Sheets・Docs 作成の依存 API |
| Google Calendar API | D6 | 予定取得 |
| Gmail API | 応用編 | メール下書き作成 |
| Google Chat API | D6 / 応用編 | メッセージ送信 |

> 💡 **Tips**: 全部有効化しなくても、未有効APIを呼んだ時 `gws` が
> 「このAPIを有効化するURL」を自動で案内してくれます。必要になった時でOK。
>
> 急ぐ場合は、最低限 **Sheets / Drive / Docs** を有効化しておけば D5 までは動きます。

**⑤ ログイン**

```bash
gws auth login
# ブラウザが開く → Googleアカウントでログイン → 認可
# 「This is not verified」の警告が出ますが、テストモード中は正常
```

スコープが多すぎるとエラーになる場合は、必要なサービスだけ指定:
```bash
gws auth login -s drive,gmail,sheets,calendar,docs
```

---

### 6-3. 動作確認

```bash
# Drive のファイル一覧を5件取得
gws drive files list --params '{"pageSize": 5}'

# Gmail の自分のプロフィール取得
gws gmail users getProfile --params '{"userId": "me"}'

# カレンダー一覧
gws calendar calendarList list
```

JSON が返ってくれば成功にゃ。

---

### 6-4. 1Password CLI (任意)

gws-skill で認証情報を 1Password Vault から注入したい場合のみ必要。
ローカルファイルの `~/.config/gws/` で管理するなら不要。

**インストール** (macOS):
```bash
brew install 1password-cli
op signin
```

**使い方**: プロジェクトの `.env` に op:// 参照を記述
```
GOOGLE_WORKSPACE_CLI_CLIENT_ID=op://<vault>/gws/client_id
GOOGLE_WORKSPACE_CLI_CLIENT_SECRET=op://<vault>/gws/client_secret
```

実行時は:
```bash
op run --no-masking --env-file=.env -- gws drive files list
```

1Password を使っていない方は、当日は **道A (Connectors)** のみ体験で OK にゃ。

---

## ☆ 7. スクショ素材の準備 (基礎編 D3 体験者向け)

D3 のスクショ貼付デモを自分で試したい方は、お好みの Webサイトのスクショを手元に用意しておくとスムーズにゃ。

**素材例**:
- GitHub / Zenn / Notion / Twitter など普段使うサイト
- お気に入りのランディングページ
- 既存プロダクトのUI
- 手書きホワイトボードの写真(iPhoneからAirDropでもOK)

Figma / Sketch のmockupがあればもちろんそれでもOK(アカウントは必須ではない)。

---

## 当日の持ち物

- ノートPC (電源アダプタも)
- Wi-Fi 接続できる端末
- メモ帳 (紙でもデジタルでも)

---

## トラブルシュート

うまく動かない場合は `faq.md` を参照するか、ハンズオン開始10分前にお越しください。
サポートスタッフが対応します。

---

## よくあるご質問

**Q. Windows PowerShell でも動きますか?**
A. 動きますが、WSL2 推奨です。一部 hook 系が POSIX 前提で書かれています。

**Q. 無料で使えますか?**
A. Claude Code 自体は無料、ただし Claude API 利用料が発生します。Anthropic のサブスクリプション or API クレジットが必要です。

**Q. 業務データを使っても大丈夫ですか?**
A. 自己責任でお願いします。不安なら使い捨てアカウント推奨。会社ポリシーも要確認。

**Q. 遅刻しそうです**
A. スライドと配布資料は後日共有するのでご安心を。ただし実演は再現されません。
