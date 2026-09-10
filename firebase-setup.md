# Firebaseセットアップ手順 (別デバイス操作用)

コントローラーと表示画面を別デバイスで同期させるための手順です。
所要時間: 約5分。費用: 無料枠で十分です。

## 手順

1. https://console.firebase.google.com/ を開く
2. 「プロジェクトを作成」→ 名前は任意 (例 `robocon-score-board`) → アナリティクスはOFFでOK → 作成
3. 左メニュー「構築」→「Realtime Database」→「データベースを作成」
   - ロケーション: `asia-southeast1` (なければ `us-central1`)
   - セキュリティルール: 「テストモードで開始」
4. 左上の歯車「プロジェクトの設定」→「全般」→「マイアプリ」→ Webアイコン (`</>`) → アプリを登録
   - 表示される `firebaseConfig = { ... }` の中身をコピー
5. このリポジトリの `firebase-config.js` に値を貼り付け (`PASTE_ME` を全て置換)
6. 「Realtime Database」→「ルール」タブを開き、以下に更新して「公開」:

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

7. コミット・push して GitHub Pages に反映:

```bash
git add firebase-config.js index.html control.html
git commit -m "Firebase同期を設定"
git push origin 2026
```

8. 動作確認:
   - 表示PCで `index.html?room=任意名` を開く (ヒント欄に `fb:●接続中` と出ればOK)
   - 操作端末で `control.html` を開き、同じルーム名を入力→「適用」(`Firebase: ●接続中` と出ればOK)
   - 操作すると両画面が同期されます

## 注意

- URL (とルーム名) を知る人は誰でも操作できます。大会では `robocon-a-2026` のような推測困難なルーム名を使ってください
- 大会終了後は FirebaseコンソールでDBを削除するか、ルールを `{ ".read": false, ".write": false }` に戻してください
- `firebase-config.js` 未設定のままでは自動でMQTTモードになります (従来通り)

## トラブルシュート

| 表示 | 意味・対処 |
|---|---|
| `Firebase: ○未設定` | `firebase-config.js` が `PASTE_ME` のまま。手順5を確認 |
| `Firebase: ○切断` | DB未作成・ルール未公開・ネット接続を確認。F12コンソールの `[fb]` ログも参照 |
| `Firebase: ○SDK読込失敗` | `gstatic.com` への接続が塞がれている環境です。MQTTモードで利用してください |
