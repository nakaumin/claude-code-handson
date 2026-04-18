# Hook デモ用サンドボックス

基礎編 D2「Hook 発火の実演」で使用するディレクトリです。

## 使い方

1. このディレクトリに `cd`
2. Claude Code を起動 (`claude`)
3. 以下のプロンプトを投げる:

```
現在のディレクトリとファイル一覧を一度に確認して
```

4. Claude は `ls && pwd` のような複合コマンドを組み立てようとする
5. `~/.claude/hooks/no-compound.js` が発火してブロック
6. Claude が自動で `ls` と `pwd` を個別実行に分割して再試行

---

## 観察ポイント

- **ブロックメッセージ**: `コマンドに複合コマンド(&&, ||, ;)を使わないでください`
- **Claude の挙動**: エラーを読んで自律的に修正
- **settings.json の役割**: PreToolUse で常にチェック

---

## 参考: `~/.claude/hooks/no-compound.js` (抜粋イメージ)

```javascript
#!/usr/bin/env node
const input = JSON.parse(require('fs').readFileSync(0, 'utf-8'));

// Bash 以外のツールは対象外 (Read/Edit などに ; を含むパスが渡っても誤検出しない)
if (input.tool_name !== 'Bash') {
  process.exit(0);
}

const command = input.tool_input?.command ?? '';
if (/\s(&&|\|\||;)\s/.test(command) || /[|;]{2}/.test(command)) {
  console.error('コマンドに複合コマンド(&&, ||, ;)を使わないでください。各コマンドを個別に実行してください。');
  process.exit(2);  // PreToolUse の拒否コード
}
process.exit(0);
```

**二重のBash限定**:

1. settings.json の `matcher: "Bash"` でフックの発火を絞る
2. スクリプト側の `tool_name !== "Bash"` でセーフティネット

どちらか一方でも十分だが、両方かけると運用中に matcher を外しても誤爆しないにゃん 🐾

## 参考: settings.json (抜粋)

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "node ~/.claude/hooks/no-compound.js"
          }
        ]
      }
    ]
  }
}
```

---

## 応用: 組織ポリシー実装のヒント

Hook で実装できるポリシー例:
- 本番DBへの接続コマンドをブロック (`psql prod-*` 等)
- `rm -rf` を常にブロック
- 深夜の push を警告
- sudo を要求したらログ記録
- 秘密情報らしき文字列(API key パターン)が含まれる出力をマスク

**ポイント**: 組織のルールを「書き下す」ことで、人間の記憶に頼らない運用が可能
