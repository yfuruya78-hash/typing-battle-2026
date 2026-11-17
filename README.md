# タイピングバトル（Z世代トレンドアワード2026）

音声入力 vs 速記 の対決をライブ配信で見せるためのタイピングゲーム。
Baidu社内ツール非依存。GitHub Pages + Firebase で外部公開できます。

## 画面
- 配信側（進行・採点）： `?role=admin` をSPブラウザで開く
- 参加者（対戦者2名）： PCブラウザで `?role=user` を開いて「ユーザー側で入場」

## セットアップ（初回のみ・約10分）

1. **Firebaseプロジェクト作成**
   - https://console.firebase.google.com をGoogleアカウントで開く
   - 「プロジェクトを追加」→ 任意の名前（例: typing-battle-2026）
   - GoogleアナリティクスはオフでOK

2. **Realtime Database作成**
   - 左メニュー「構築」→「Realtime Database」→「データベースを作成」
   - ロケーション: **シンガポール（asia-southeast1）** 推奨（日本からレイテンシが低い）
   - 「テストモードで開始」を選択

3. **Webアプリ登録 & config取得**
   - プロジェクト設定（歯車）→ 一般 → 「アプリを追加」→ 「</>（Web）」
   - アニックネームは任意。登録すると `firebaseConfig` が表示される

4. **index.html の設定を書き換え**
   - `<head>` 内の `window.FIREBASE_CONFIG` に `apiKey` / `authDomain` / `databaseURL` / `projectId` を貼る
   - `databaseURL` は Realtime Database の画面上部に表示されているURL（`*.firebasedatabase.app`）

5. **セキュリティルール**
   - Realtime Database → 「ルール」タブ → このリポジトリの `rules.json` の中身を貼り付け → 「公開」
   - ※URLを知る人なら誰でも書けるルールです。URLは関係者のみに共有してください

6. **GitHub Pages公開**
   - このリポジトリを push → Settings → Pages → Branch: main / root → Save
   - 数分後に `https://<ユーザー名>.github.io/<リポジトリ名>/` で公開される

## 運用
- リハ前に配信側で「部屋をリセット」（全参加者・過去データを削除）
- 参加者2人が入室 → 「お題を選ぶ」が解禁 → ゲームスタート
- 採点は正確率7点＋速さ3点の10点満点

## データ構造（Firebase Realtime Database）
- `players/{key}`: 参加者（name, icon, state, sent, sentAt, text, roundSent...）
- `boards/current`: 現在の試合（phase, round, topicNo, theme, startedAt, scores）
- `boards/all/{key}`: 試合の履歴
