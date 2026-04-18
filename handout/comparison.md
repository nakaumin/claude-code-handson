# Connectors (MCP) vs gws-skill 詳細比較

Google Workspace を Claude Code から操作する2つの方法の比較資料です。

---

## 早見表

| 軸 | Connectors (MCP) | gws-skill (CLI ラッパー) |
|---|---|---|
| **セットアップ** | GUI で OAuth クリック | `gws` CLI + 1Password 参照を `.env` に |
| **認証の置き場所** | Claude 側が保持 | 手元の 1Password Vault (自分で管理) |
| **対象範囲** | 接続したアカウント全体 | `.env` の op:// 参照で切替可 (個人/業務分離が楽) |
| **非エンジニア適性** | ◎ ワンクリック | △ 初期設定でつまずきやすい |
| **バージョン/再現性** | Anthropic 側更新 | 自分でピン留め可、CI に載せやすい |
| **バッチ/スクリプト** | MCP経由で呼ぶ必要 | `gws-run` で shell から直接叩ける |
| **監査ログ** | Claude セッション内 | ローカルで `gws` ログが残る |
| **推奨用途** | 日常の対話操作 | 自動化・定期実行・チーム配布スクリプト |

**一言サマリ**: *「触って使うならコネクター、仕込んで回すなら gws-skill」*

---

## Connectors (MCP) 詳細

### 仕組み

Claude Code → MCP サーバー (HTTP / stdio) → Google API

```
claude → mcp__gmail__search_threads(...) → Google Gmail API
```

### セットアップ手順

1. Claude Code を起動
2. `/mcp` でサーバー一覧を表示
3. 接続したいサービス (Gmail / Calendar 等) を選択
4. ブラウザで OAuth 認可
5. 以後、自動で使われる

### メリット

- **簡単**: ワンクリックで認証完了
- **メンテフリー**: Anthropic がサーバーを管理
- **対話向き**: Claude が自律的にツール選択
- **複数サービスを一元管理**

### デメリット

- **認証情報の所在**: Claude 側にあるので、組織ポリシー次第で NG の場合も
- **CI からは使いづらい**: MCP 経由前提なので対話セッション必須
- **バージョン管理不可**: Anthropic がバージョンを決める

### 利用可能な主なツール (2026-04 時点)

| カテゴリ | 例 |
|---|---|
| Gmail | `search_threads`, `get_thread`, `create_draft` |
| Calendar | `list_events`, `create_event`, `update_event` |
| Drive | `search_files`, `read_file_content`, `create_file` |
| Chat | (コネクター拡張により対応) |

---

## gws-skill (CLI ラッパー) 詳細

### 仕組み

Claude Code → `/gws-*` skill → `gws-run` → `op read` → `gws` CLI → Google API

```
/gws-gmail-send
  → gws-run が .env の op:// を解決
  → gws gmail send --to=... --subject=... --body=...
  → Google Gmail API
```

### セットアップ手順

1. `gws` CLI をインストール
   ```bash
   # 例: 独自実装 or 既存 gws-client
   brew install <your-gws>
   ```

2. 1Password にサービスアカウント JSON を保存
   - Vault 名: `<your vault>`
   - Item 名: `gws-service-account`

3. プロジェクトの `.env` に参照を記述
   ```
   GOOGLE_APPLICATION_CREDENTIALS=op://<vault>/gws-service-account/credentials.json
   ```

4. `/gws-run` 経由で動作確認
   ```
   /gws-run  gws gmail list --limit=5
   ```

### メリット

- **認証が手元**: 組織のセキュリティ要件に合わせやすい
- **CI 対応**: GitHub Actions 等で同じコマンドが動く
- **再現性**: `gws` のバージョン固定可能
- **監査**: コマンド履歴がローカルに残る

### デメリット

- **初期セットアップが重い**: 1Password、`gws`、`.env` の3つを揃える
- **対話性に欠ける**: 定型タスクが得意、自由な探索は苦手
- **非エンジニアにはハードル高い**

### ユーザー環境の skill 一覧 (抜粋)

| Skill | 用途 |
|---|---|
| `gws-gmail-triage` | 未読メールを要約表示 |
| `gws-gmail-send` | メール送信 |
| `gws-gmail-reply` | 返信 (スレッド処理自動) |
| `gws-gmail-forward` | 転送 |
| `gws-calendar-insert` | 予定作成 |
| `gws-sheets-read` | シート読取 |
| `gws-sheets-append` | 行追加 |
| `gws-docs-write` | Docs に追記 |
| `gws-drive-upload` | ファイルアップロード |
| `gws-chat-send` | Chat メッセージ送信 |
| `gws-meet` | Meet 会議管理 |

---

## 使い分けの判断フロー

```
質問: この作業は...

┌─ 今この場で 1回だけやる? ─────> Connectors 推奨
│
├─ 毎日/毎週 繰り返す? ────────> gws-skill 推奨
│
├─ CI/cron から呼びたい? ──────> gws-skill ほぼ必須
│
├─ 複数メンバーで同じ操作を? ──> gws-skill (チーム配布しやすい)
│
├─ 認証情報は組織管理? ────────> gws-skill (1Password 経由)
│
└─ 探索的に試行錯誤? ──────────> Connectors (対話が強い)
```

---

## ハイブリッド活用例

実運用では **混ぜて使う** のが現実解:

### シナリオ: 週次レポート自動化

```
[月曜 9:00 cron 起動]
  ↓
gws-sheets-read       ← 先週のKPI取得 (gws-skill、再現性重視)
  ↓
Claude に分析依頼     ← 対話的に異常値を探す (Connectors でも可)
  ↓
gws-docs-write        ← レポートを Docs に出力 (gws-skill、定型処理)
  ↓
gws-gmail-send        ← 関係者に配信 (gws-skill、送信先リスト固定)
```

### シナリオ: アドホック調査

```
[思いついた時に実行]
  ↓
Connectors/Gmail      ← 特定顧客のメール全て検索
  ↓
Connectors/Drive      ← 関連資料を横断検索
  ↓
Claude で要約
  ↓
(結果をコピペして人間が判断)
```

---

## セキュリティ観点

| 懸念 | Connectors | gws-skill |
|---|---|---|
| 認証情報漏洩リスク | Anthropicに預ける | 自分で管理 |
| トークン失効時の対応 | 自動再認証 | 手動(opで更新) |
| 権限スコープの細かさ | OAuth スコープに準拠 | サービスアカウント単位 |
| 組織ポリシー適合 | 要確認 | 柔軟に設計可能 |
| ログ監査 | Claude 側ログ | ローカル + 組織の監査基盤 |

**社内ポリシー上、どちらが許可されているか** を必ず確認してください。

---

## 参考リンク

- Claude Code 公式: <https://docs.claude.com/en/docs/claude-code/overview>
- MCP 仕様: <https://modelcontextprotocol.io/>
- 1Password CLI: <https://developer.1password.com/docs/cli/>
- Google Workspace API: <https://developers.google.com/workspace>
