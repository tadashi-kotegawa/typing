# NEO-TYPE: Online Ranking System

オンラインランキング機能を追加したNEO-TYPEタイピングゲーム

## 🎯 機能概要

このプロジェクトにFirebase Firestoreを使用したオンラインランキングシステムを実装しました。

### 主な機能
- 🏆 **オンラインランキング**: トップ50のスコアをリアルタイムで表示
- 👤 **ユーザー名登録**: スコア送信時にユーザー名を設定可能
- 🎨 **ランキング表示**: 1位〜3位に特別なスタイリング（金・銀・銅）
- 📱 **レスポンシブデザイン**: モバイル・デスクトップ両対応
- ⚡ **リアルタイム更新**: Firestoreから最新データを取得

## 📁 ドキュメント

### セットアップガイド
詳細なFirebaseセットアップ手順は以下を参照してください：
- **[FIREBASE_SETUP.md](./FIREBASE_SETUP.md)** - Firebase プロジェクトの作成から設定まで

### 技術ドキュメント
実装の詳細については以下を参照してください：
- **[IMPLEMENTATION_SUMMARY.md](./IMPLEMENTATION_SUMMARY.md)** - 技術的な実装詳細とセキュリティ考慮事項
- **[UI_LAYOUT.md](./UI_LAYOUT.md)** - UI レイアウトと操作フロー

## 🚀 クイックスタート

### 1. Firebase プロジェクトの準備

1. [Firebase Console](https://console.firebase.google.com/) でプロジェクトを作成
2. Firestore Database を有効化
3. Web アプリを登録して設定情報を取得

詳細は [FIREBASE_SETUP.md](./FIREBASE_SETUP.md) を参照してください。

### 2. 設定の適用

`woo.html` ファイルを開き、以下の部分を編集：

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",                    // ← ここを変更
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",              // ← ここを変更
  storageBucket: "YOUR_PROJECT_ID.appspot.com",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"                       // ← ここを変更
};
```

### 3. セキュリティルールの設定

Firebase Console > Firestore Database > ルール で以下を設定：

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /rankings/{document=**} {
      allow read: if true;
      allow write: if true;
    }
  }
}
```

### 4. アプリの起動

ブラウザで `woo.html` を開くか、ローカルサーバーを起動：

```bash
# Python 3の場合
python3 -m http.server 8000

# Node.jsの場合（npx使用）
npx serve .
```

ブラウザで `http://localhost:8000/woo.html` にアクセス

## 🎮 使い方

### ランキングの表示

1. **メニュー画面**または**プレイ中**の画面左上の👑ボタンをクリック
2. トップ50のランキングが表示されます

### スコアの登録

1. ゲームをプレイしてリザルト画面へ
2. 「👑 ランキングに登録」ボタンをクリック
3. ユーザー名を入力（または空欄でAnonymous）
4. 「登録する」ボタンをクリック
5. 自動的にランキングが表示されます

## 📊 データ構造

### Firestore Collection: `rankings`

```javascript
{
  username: "PlayerName",  // string (最大20文字)
  score: 12345,            // number (整数)
  timestamp: Timestamp     // Firebase Timestamp
}
```

### クエリ
```javascript
query(
  collection(db, 'rankings'),
  orderBy('score', 'desc'),
  limit(50)
)
```

## 🎨 UI コンポーネント

### ランキングボタン
- 位置: 画面左上（固定）
- アイコン: 👑 (Crown)
- 機能: ランキングモーダルを開く

### ランキングモーダル
- トップ50スコアを表示
- 1位〜3位に特別なスタイリング
- ユーザー名、スコア、日時を表示

### ユーザー名入力モーダル
- スコア送信フォーム
- ユーザー名入力（オプション）
- 送信後、自動でランキング表示

詳細は [UI_LAYOUT.md](./UI_LAYOUT.md) を参照してください。

## 🔒 セキュリティ

### 現在の設定（開発用）
- 読み取り: 全員許可
- 書き込み: 全員許可

### 本番環境での推奨設定

1. **認証の追加**: Firebase Authentication を統合
2. **レート制限**: Cloud Functions でスコア送信を制限
3. **バリデーション**: スコアの妥当性を検証
4. **セキュリティルール**: より厳格な制御

詳細は [IMPLEMENTATION_SUMMARY.md](./IMPLEMENTATION_SUMMARY.md) のセキュリティセクションを参照してください。

## 🛠️ トラブルシューティング

### Firebase初期化エラー
- `firebaseConfig` の値が正しいか確認
- Firebase Console でプロジェクトが作成されているか確認

### スコア送信/取得エラー
- Firestore Database が有効になっているか確認
- セキュリティルールが正しく設定されているか確認
- ブラウザのコンソールでエラー詳細を確認

### ランキングが表示されない
- ネットワーク接続を確認
- Firebase Console でデータが保存されているか確認
- ブラウザのキャッシュをクリア

## 📝 技術スタック

- **Frontend**: React (Hooks), Tailwind CSS
- **Backend**: Firebase Firestore
- **Icons**: Lucide React
- **Language**: JavaScript (ES6+)

## 🔄 今後の改善案

- [ ] Firebase Authentication の統合
- [ ] 期間別ランキング（日次、週次、月次）
- [ ] ユーザープロフィール機能
- [ ] スコア共有機能
- [ ] リアルタイム更新（WebSocket）
- [ ] パフォーマンス最適化（ページネーション）
- [ ] 不適切なユーザー名のフィルタリング

## 📄 ライセンス

© Tadashi Kotegawa

## 🤝 コントリビューション

バグ報告や機能提案は GitHub Issues でお願いします。

---

**Created by**: GitHub Copilot  
**Date**: 2026-02-14  
**Version**: 1.0.0
