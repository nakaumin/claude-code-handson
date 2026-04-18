# Claude Code 実践ハンズオン 資料一式

90分のClaude Code実践ハンズオン用のプレゼン資料と補助資料です。

## ファイル構成

```
ClaudeCodeハンズオン準備/
├── README.md                        # このファイル
├── CLAUDE.md                        # Claude Code 起動時の自動ルール (感想収集)
├── .gitignore                       # user-comment.md 等を除外
├── slides.md                        # Marp スライド本体 (メイン成果物)
├── handout/
│   ├── prerequisites.md             # 参加者事前準備リスト
│   ├── prompts.md                   # 今日使ったプロンプト集
│   ├── comparison.md                # Connectors vs gws-skill 詳細比較
│   └── faq.md                       # トラブルシュート FAQ
├── demo/
│   ├── sample-minutes.md            # 応用編デモ用サンプル議事録
│   └── hook-demo/
│       └── README.md                # 基礎編 D2 Hook デモ用サンドボックス
├── user-comment.md                  # (gitignore) 各参加者がClaudeと対話して作る感想ログ
└── Anthropic Claude Code Usecase/
    └── How-Anthropic-teams-use-Claude-Code_v2.pdf  # 元ネタPDF
```

## 構成概要

| 時間 | セクション | 内容 |
|---|---|---|
| 0:00–0:10 | 導入 | 自己紹介 / ゴール / 環境確認 |
| 0:10–0:35 | **基礎編** | 設定・TIPS (CLAUDE.md / Plan / 画像貼付 / Skills / Hooks) |
| 0:35–1:05 | **実用編** | Google Workspace連携 (Connectors vs gws-skill) |
| 1:05–1:20 | **応用編** | エージェントチーム (議事録→アクション配信パイプライン) |
| 1:20–1:30 | Q&A | 振り返り |

## スライドのプレビュー・出力

**ライブプレビュー**:
```bash
npx @marp-team/marp-cli slides.md --preview
```

**PDF エクスポート**:
```bash
npx @marp-team/marp-cli slides.md --pdf
```

**PPTX エクスポート**:
```bash
npx @marp-team/marp-cli slides.md --pptx
```

**HTML エクスポート**:
```bash
npx @marp-team/marp-cli slides.md --html
```

## 配布物 (参加者向け)

当日または事前に配布すると効果的:

1. **prerequisites.md** - 事前準備案内(1週間前に配布推奨)
2. **prompts.md** - プロンプト集(当日終了時に配布)
3. **comparison.md** - Connectors vs gws-skill(実用編の参考)
4. **faq.md** - トラブル対処集(当日から共有)

## 講師向けメモ

### デモの事前準備

- **D1 (Plan モード)**: 何か適当なリポジトリで試しておく
- **D2 (Hook 発火)**: `~/.claude/hooks/no-compound.js` が有効か確認
- **D3 (Figma)**: mockup を1枚スクショで用意
- **D4 (Gmail Connectors)**: OAuth 済みか確認、未読メールが数通あること
- **D5 (Sheets gws-skill)**: テスト用スプレッドシートID を事前準備
- **D6 (ハイブリッド)**: カレンダー予定を午後に1-2件入れておく
- **応用編**: `demo/sample-minutes.md` を Google Docs にアップして ID を控える

### 保険

- 各デモを **録画 GIF** で用意 (ネット不調時の保険)
- バックアップ用の Google アカウント
- サンプル結果のスクショ

### タイムマネジメント

- 基礎編 25分: TIPS①②③ (★★★) を最優先、残りは時間次第で割愛可
- 実用編 30分: D4-D6 で15分、残り15分で解説・比較
- 応用編 15分: ライブデモ7分 + 解説5分 + バッファ3分

### 非エンジニア参加者へのケア

- 「コードを書かなくていい」を何度か言及
- Plan モードでの安全性を強調
- D3 (画像貼付) で「自分にもできそう」感を早めに作る

## 参照: 元ネタ PDF

[`Anthropic Claude Code Usecase/How-Anthropic-teams-use-Claude-Code_v2.pdf`](Anthropic%20Claude%20Code%20Usecase/How-Anthropic-teams-use-Claude-Code_v2.pdf)

10部門(Data Infrastructure, Product Dev, Security, Inference, Data Science, API, Growth Marketing, Product Design, RL Engineering, Legal)の実例が満載。スライド内で各セクションの「Anthropic事例」として引用しています。

## ライセンス・再利用

社内/コミュニティで自由に改変してご利用ください。
(元ネタPDFは Anthropic の配布物です、引用マナーに留意)
