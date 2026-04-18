# トラブルシュート FAQ

ハンズオン中や後日使っていて詰まりやすいポイントの対処法集です。

---

## インストール・起動系

### Q. `claude --version` でコマンドが見つからない
**A.** npm のグローバルインストールパスが PATH に入っていません。
```bash
npm config get prefix
# 出力されたパス + /bin を PATH に追加
```
または `nvm` / `volta` 経由で Node を再インストール。

### Q. 起動後、OAuth 画面が出ない
**A.** 既に別アカウントでログイン済みの可能性。
```bash
claude /logout
claude  # 再ログイン
```

### Q. `Error: too many requests` が出る
**A.** Rate limit に引っかかっています。5分待つか、別の API key に切替。
Anthropic Console で使用状況を確認: <https://console.anthropic.com/>

---

## Plan モード系

### Q. Plan モードから抜けられない
**A.** `Shift+Tab` を2回押すと通常モードに戻ります。
状態は画面右下のインジケータで確認できます。

### Q. 承認した plan がどこに保存されているか分からない
**A.** `~/.claude/plans/` に Markdown で保存されます。
```bash
ls -t ~/.claude/plans/ | head -5
```

---

## 画像貼付系

### Q. スクショを貼っても認識されない
**A.** 貼付直後にエンターではなく、**テキストと併せて送信** してください。
```
(ここで Cmd+V で画像貼付)
このUIを実装してください
[Enter]
```

### Q. 画像が大きすぎてエラー
**A.** 5MB 以下に圧縮してください。macOS なら `sips -Z 1920 image.png` で縮小。

---

## Hooks 系

### Q. `no-compound.js` hook が効いていない
**A.** settings.json の `hooks.PreToolUse` に登録されているか確認:
```bash
cat ~/.claude/settings.json
```
hooks セクションの matcher が `Bash` になっているか、スクリプトパスが絶対パスかをチェック。

### Q. hook のエラーメッセージを Claude に読ませたい
**A.** hook 内で `console.error(...)` + `process.exit(2)` で出力すると、Claude の next turn に渡されます。`exit(1)` はユーザーに表示されるだけで Claude には渡りません。

---

## Google Workspace 連携: Connectors

### Q. OAuth で "このアプリは確認されていません" 警告
**A.** Anthropic 公式の MCP サーバーを使っている場合は「詳細 > 安全ではないページに移動」で進めて OK。
社内ポリシーで NG なら、gws-skill の道Bを使用。

### Q. Gmail コネクターで一部のスレッドが取得できない
**A.** OAuth スコープが不足している可能性。一度ログアウトして、認可時にすべての権限を許可してください。

### Q. Calendar の create_event で「Insufficient permissions」
**A.** 読み取り専用スコープで認可していませんか? 書込みを含めて再認可。

### Q. Drive の検索結果に会社ファイルが出てこない
**A.** 個人アカウントと組織アカウントのどちらで接続しているか確認。組織アカウントが必要なら `/mcp` で切り替え。

---

## Google Workspace 連携: gws-skill

### Q. `gws: command not found`
**A.** `gws` CLI が未インストールです。いずれかでインストール:
```bash
brew install googleworkspace-cli       # Homebrew
npm install -g @googleworkspace/cli    # npm
```
詳細は `prerequisites.md` の「6. gws CLI + Google Cloud 設定」参照。

### Q. `gws auth login` で `Access blocked: This app's request is invalid`
**A.** OAuth同意画面の **Test users** に自分のアカウントが追加されていません。
<https://console.cloud.google.com/apis/credentials/consent> で追加してください。

### Q. `accessNotConfigured` エラー
**A.** 呼ぼうとしたAPIが GCPプロジェクトで有効化されていません。
エラーメッセージに含まれるURLをクリック → 「有効にする」をクリック。

### Q. `gws auth setup` で `gcloud: command not found`
**A.** 自動セットアップには gcloud CLI が必要です。
```bash
brew install --cask google-cloud-sdk
gcloud auth login
```
手動セットアップなら gcloud は不要 (prerequisites.md 参照)。

### Q. ログイン時にスコープが多すぎてエラー
**A.** テストモードは最大約25スコープまで。必要なサービスだけ指定:
```bash
gws auth login -s drive,gmail,sheets,calendar
```

### Q. `client_secret.json` の置き場所が分からない
**A.** `~/.config/gws/client_secret.json` が標準位置。
変更したい場合は環境変数 `GOOGLE_WORKSPACE_CLI_CONFIG_DIR` で上書き可能。

### Q. `/gws-*` を呼ぶと「op: command not found」
**A.** 1Password CLI が未インストールです。
```bash
brew install 1password-cli
op signin
```

### Q. `op read` で "not signed in to any accounts"
**A.** 1Password CLI の認証が切れています。
```bash
op signin
# or
eval $(op signin)
```

### Q. `.env` の op:// 参照が解決されない
**A.** Vault 名にスペースや日本語が含まれていないかチェック。
参照パスの書式:
```
op://<vault名>/<item名>/<field名>
```

### Q. `gws gmail send` で認証エラー
**A.** サービスアカウントに Gmail API の権限が付与されているか、ドメイン全体委任が設定されているかを確認。G Workspace 管理者に相談。

---

## エージェントチーム系

### Q. `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` を設定してもチームが有効にならない
**A.** settings.json の `env` セクションに入れる or シェル起動時に export:
```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

### Q. サブエージェントが並列実行されない
**A.** `TeamCreate` ツールを経由していますか? 通常の `Agent` 呼び出しを複数並べても直列になりがちです。明示的にチーム化してください。

### Q. 1つのエージェントだけ失敗して全体が止まる
**A.** orchestrator のプロンプトで「失敗したエージェントはスキップして残りを継続」と明記してください。デフォルトは厳格(fail-fast)です。

---

## 権限・セキュリティ系

### Q. 毎回権限プロンプトが出てうるさい
**A.** `/less-permission-prompts` を実行すると、よく使う操作を settings.json に自動追加してくれます。

### Q. 特定のコマンドを絶対にブロックしたい
**A.** settings.json の `permissions.deny` に追加:
```json
{
  "permissions": {
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force*)"
    ]
  }
}
```

### Q. サンドボックス外のコマンドが実行できない
**A.** デフォルトで有効なサンドボックスを回避したい場合は、該当コマンドに個別許可を付与。一括無効化は非推奨。

---

## パフォーマンス系

### Q. 応答が遅い
**A.**
1. コンテキストが膨大 → `/clear` で新しい会話を開始
2. 大量のファイルを読み込ませている → Glob/Grep で絞る
3. モデルを変更: `/model` で Haiku に切替 (速度優先時)

### Q. 長い出力が途中で止まる
**A.** max_tokens の上限か、cost limit の可能性。`/compact` で会話を圧縮してから続行。

---

## ハンズオン特有

### Q. デモ D6 (ハイブリッド) で Chat に投稿されない
**A.** Chat スペースの ID が特定できていない可能性。事前に `list_spaces` で ID を確認しておいてください。

### Q. サンプル議事録の Docs ID が無効
**A.** 講師から配布された最新の ID を使用してください。古いIDは当日無効化されている場合があります。

### Q. 応用編デモで途中で止まる
**A.** 1つのエージェントの API quota が尽きた可能性。残りを individual に実行してください。

---

## どうしても解決しない場合

- ハンズオン中: サポートスタッフ or 隣の席の参加者に声かけ
- 後日: `#claude-code-handson` チャンネルで質問
- 公式 Issue: <https://github.com/anthropics/claude-code/issues>

**エラーメッセージ全文** と **実行したコマンド** を添えると解決が早いです。
