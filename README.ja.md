# CinePost — 映画マイクロブログプラットフォーム

[English](README.md) | **日本語**

映画好きのためのマイクロブログプラットフォームです。映画の短いレビューを投稿したり、ほかのユーザーをフォローしたり、自分と好みが合う人を見つけたりできます。

詳しい手順は、[バックエンド API のドキュメント](backend/README.ja.md)と[フロントエンドのドキュメント](frontend/README.ja.md)を参照してください。

## 技術構成

| レイヤー | 技術 |
|---|---|
| バックエンド | Node.js + Express |
| データベース | better-sqlite3 を使用した SQLite |
| パスワードのハッシュ化 | bcrypt |
| フロントエンド | React + Vite |

## 必要な環境

- Node.js v20 以降
- npm

## ローカルでの実行方法

### 1. リポジトリをクローンする

```bash
git clone https://github.com/KoseiHoriJP/Movie-Microblogging-Platform.git
cd Movie-Microblogging-Platform
```

### 2. バックエンドを起動する

```bash
cd backend
npm install
cp .env.example .env
```

`.env` を開き、`JWT_SECRET` に十分に長いランダムな文字列を設定します。その後、次のコマンドを実行してください。

```bash
node server.js
```

API は `http://localhost:3000` で起動します。初回起動時に `db/schema.sql` から SQLite データベースが自動作成され、映画 20 件とデモアカウント 4 件の初期データが登録されます。

### 3. フロントエンドを起動する

別のターミナルを開きます。

```bash
cd frontend
npm install
npm run dev
```

アプリには `http://localhost:5173` からアクセスできます。

## デモアカウント

すべてのデモアカウントのパスワードは `password123` です。

| ユーザー名 | 表示名 |
|---|---|
| nolan_fan | Nolan Fan |
| arthouse_ava | Arthouse Ava |
| sci_fi_sam | Sci-Fi Sam |
| blockbuster_bea | Blockbuster Bea |

## プロジェクト構成

```text
/
├── backend/
│   ├── server.js           # エントリーポイント：ミドルウェアとルートの設定
│   ├── routes/             # 認証、ユーザー、投稿、映画のルート
│   ├── middleware/         # JWT 認証、入力検証
│   ├── db/
│   │   ├── schema.sql      # テーブル定義 — このファイルから DB を再作成
│   │   └── app.db          # 実行時に生成（Git の追跡対象外）
│   ├── .env.example        # 環境変数のテンプレート
│   └── README.md           # API の詳細なドキュメント
├── frontend/
│   ├── src/
│   │   ├── components/     # Feed、Login、MovieCard、MovieDetail、MovieSearch、UserProfile、UserSearch
│   │   └── App.jsx         # ルーティングと上部ナビゲーション
│   └── README.md           # フロントエンドのセットアップ手順
└── design-document.md
```
