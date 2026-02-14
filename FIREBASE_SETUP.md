# Firebase Ranking System Setup Guide

このガイドでは、タイピングゲームのオンラインランキングシステムを有効にするためのFirebaseセットアップ手順を説明します。

## 前提条件

- Googleアカウント
- Firebase Console へのアクセス

## セットアップ手順

### 1. Firebaseプロジェクトの作成

1. [Firebase Console](https://console.firebase.google.com/) にアクセス
2. 「プロジェクトを追加」をクリック
3. プロジェクト名を入力（例：typing-game-ranking）
4. Google Analyticsの設定（オプション）を行い、プロジェクトを作成

### 2. Firestore Databaseの設定

1. Firebase Consoleのサイドバーから「Firestore Database」を選択
2. 「データベースの作成」をクリック
3. セキュリティルールで「本番環境モード」を選択（推奨）
4. ロケーションを選択（asia-northeast1を推奨）
5. 「有効にする」をクリック

### 3. セキュリティルールの設定

Firestore Databaseのルールタブで、以下のルールを設定します：

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // ランキングコレクションへのアクセスルール
    match /rankings/{document=**} {
      // 読み取りは誰でも可能
      allow read: if true;
      // 書き込みは認証済みユーザーのみ（または制限なし）
      allow write: if true; // 注意: 本番環境では適切な認証を実装することを推奨
    }
  }
}
```

### 4. Web アプリの追加と設定情報の取得

1. Firebase Consoleのプロジェクト設定（⚙️アイコン）を開く
2. 「全般」タブを選択
3. 「マイアプリ」セクションまでスクロール
4. Webアプリのアイコン（</>）をクリック
5. アプリのニックネームを入力（例：typing-game-web）
6. 「Firebase Hostingを設定する」はオプション（必要に応じてチェック）
7. 「アプリを登録」をクリック
8. 表示される設定情報（firebaseConfig）をコピー

### 5. アプリへの設定情報の適用

1. `woo.html` ファイルを開く
2. ファイルの先頭にある `firebaseConfig` オブジェクトを探す
3. 以下の値を Firebase Console から取得した値に置き換える：

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",           // ← Firebase Consoleの値に置き換え
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",  // ← Firebase Consoleの値に置き換え
  projectId: "YOUR_PROJECT_ID",     // ← Firebase Consoleの値に置き換え
  storageBucket: "YOUR_PROJECT_ID.appspot.com",  // ← Firebase Consoleの値に置き換え
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",  // ← Firebase Consoleの値に置き換え
  appId: "YOUR_APP_ID"              // ← Firebase Consoleの値に置き換え
};
```

### 6. 動作確認

1. アプリを起動
2. ゲームをプレイして結果画面に到達
3. 「ランキングに登録」ボタンをクリック
4. ユーザー名を入力して登録
5. 「ランキングを見る」ボタンまたはメニュー画面左上のランキングボタンをクリック
6. 登録したスコアが表示されることを確認

## データ構造

Firestore Database の `rankings` コレクションには以下の構造でデータが保存されます：

```javascript
{
  username: "ユーザー名",  // string
  score: 12345,           // number
  timestamp: Timestamp    // Firebase Timestamp
}
```

## トラブルシューティング

### エラー: "Firebase initialization error"

- Firebase設定が正しく入力されているか確認
- Firebase Consoleでプロジェクトが正しく作成されているか確認

### エラー: "Error submitting score" または "Error fetching rankings"

- Firestore Databaseが有効になっているか確認
- セキュリティルールが正しく設定されているか確認
- ブラウザの開発者ツール（Console）でエラーの詳細を確認

### ランキングが表示されない

- Firestoreにデータが正しく保存されているか確認（Firebase Console > Firestore Database）
- ネットワーク接続を確認
- ブラウザのキャッシュをクリアして再試行

## セキュリティに関する注意事項

本実装では、誰でもスコアを登録・閲覧できる設定になっています。本番環境では以下の対策を推奨します：

1. **認証の実装**: Firebase Authentication を使用してユーザー認証を追加
2. **レート制限**: スコア送信の頻度制限を実装
3. **バリデーション**: Cloud Functions でスコアの妥当性を検証
4. **セキュリティルールの強化**: より厳格なアクセス制御を設定

## 参考リンク

- [Firebase Documentation](https://firebase.google.com/docs)
- [Cloud Firestore Documentation](https://firebase.google.com/docs/firestore)
- [Firebase Security Rules](https://firebase.google.com/docs/rules)
