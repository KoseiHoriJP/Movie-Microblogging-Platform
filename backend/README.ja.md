# バックエンド API

[English](README.md) | **日本語**

CinePost 映画マイクロブログプラットフォームの Express + SQLite バックエンドです。

## セットアップ

```bash
cd backend
npm install
cp .env.example .env   # set a real JWT_SECRET
node server.js
```

上のコピーコマンドの後、`.env` の `JWT_SECRET` に実際に使用する署名用の秘密文字列を設定してからサーバーを起動してください。

初回起動時に `db/app.db` に SQLite データベースが自動作成されます。`db/schema.sql` のスキーマが適用され、映画 20 件とデモアカウント 4 件（パスワード：`password123`）の初期データが登録されます。

## 環境変数

| 変数 | 説明 | デフォルト値 |
|---|---|---|
| `PORT` | サーバーが待ち受けるポート | `3000` |
| `JWT_SECRET` | JWT トークンの署名に使用する秘密文字列 | なし — 設定必須 |
| `ALLOWED_ORIGIN` | CORS で許可するフロントエンドのオリジン | `http://localhost:5173` |

## 認証

認証が必要なルートには `Authorization: Bearer <token>` を送信してください。トークンはアカウント登録時またはログイン時に発行され、有効期限は 1 時間です。

## エンドポイント

| メソッド | パス | 認証 | 説明 |
|---|---|---|---|
| POST | `/api/users/register` | 不要 | アカウントを作成し、トークンを返す |
| POST | `/api/users/login` | 不要 | ログインし、トークンを返す |
| GET | `/api/users` | 任意 | ユーザー一覧を取得。`?search=` でユーザー名または表示名を絞り込む |
| GET | `/api/users/:id` | 任意 | ユーザーの公開プロフィールを取得 |
| POST | `/api/users/:id/follow` | 必須 | ユーザーをフォロー |
| DELETE | `/api/users/:id/follow` | 必須 | ユーザーのフォローを解除 |
| GET | `/api/profile` | 必須 | 自分のプロフィールを取得 |
| POST | `/api/posts` | 必須 | 投稿・レビューを作成（最大 280 文字）。`movie_id` と `reaction`（upvote/downvote）は任意 |
| GET | `/api/posts/feed` | 必須 | フォロー中のユーザーの投稿を新しい順に表示するフィードを取得 |
| GET | `/api/posts/:id` | 任意 | 返信と「いいね」・返信の件数を含む投稿を取得 |
| PUT | `/api/posts/:id` | 必須（投稿者本人） | 自分の投稿を編集 |
| DELETE | `/api/posts/:id` | 必須（投稿者本人） | 自分の投稿を削除 |
| POST | `/api/posts/:id/like` | 必須 | 投稿に「いいね」を付ける |
| DELETE | `/api/posts/:id/like` | 必須 | 投稿の「いいね」を解除 |
| POST | `/api/posts/:id/replies` | 必須 | 投稿に返信（最大 280 文字） |
| GET | `/api/movies` | 不要 | 映画一覧を取得。`?search=` でタイトルを絞り込む |
| GET | `/api/movies/:id` | 不要 | すべてのレビュー（投稿）を含む映画の詳細を取得 |
| GET | `/api/health` | 不要 | ヘルスチェック |

## プロジェクト構成

```text
backend/
├── server.js           # エントリーポイント：セキュリティ用ミドルウェアとルートの設定
├── db/
│   ├── schema.sql      # テーブル定義（users、movies、posts、follows、likes、replies）
│   ├── database.js     # SQLite 接続を開き、スキーマを適用
│   └── app.db          # 実行時に生成（Git の追跡対象外）
├── middleware/
│   ├── auth.js         # authenticateToken と optionalAuth ミドルウェア
│   └── validate.js     # express-validator のエラーハンドラー
└── routes/
    ├── auth.js         # POST /register、POST /login
    ├── users.js        # プロフィール、ユーザー検索、フォロー・フォロー解除
    ├── posts.js        # 投稿、フィード、「いいね」、返信
    └── movies.js       # 映画の一覧と詳細
```
