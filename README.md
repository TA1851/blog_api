# FastAPI Docker プロジェクト - Blog API

## 概要
このプロジェクトはDockerを使用したFastAPI製のブログAPIアプリケーションです。ユーザー認証、記事管理、メール認証機能を備えています。

## プロジェクト構成
```
.
├── blog-api-main/
│   ├── main.py              # FastAPIアプリケーションのエントリーポイント
│   ├── database.py          # データベース設定
│   ├── models.py            # SQLAlchemyモデル
│   ├── schemas.py           # Pydanticスキーマ
│   ├── oauth2.py            # OAuth2認証処理
│   ├── hashing.py           # パスワードハッシュ化
│   ├── custom_token.py      # カスタムトークン生成
│   ├── exceptions.py        # カスタム例外
│   ├── requirements.txt     # Python依存関係
│   ├── routers/
│   │   ├── article.py       # 記事関連エンドポイント
│   │   ├── auth.py          # 認証エンドポイント
│   │   └── user.py          # ユーザー管理エンドポイント
│   ├── logger/
│   │   └── custom_logger.py # カスタムロガー
│   └── utils/
│       ├── email_sender.py      # メール送信機能
│       └── email_validator.py   # メールバリデーション
├── docker/
│   ├── Dockerfile           # Dockerイメージ定義
│   └── docker-compose.yml   # Docker Compose設定
└── README.md
```

## セットアップと起動

### 1. 環境変数の設定
必要に応じて `docker/docker-compose.yml` の環境変数を編集してください：
- `SECRET_KEY`: JWT署名用の秘密鍵（本番環境では必ず変更）
- `ALGORITHM`: JWT署名アルゴリズム（デフォルト: HS256）
- `CORS_ORIGINS`: 許可するオリジン

### 2. Dockerコンテナのビルドと起動
```bash
cd docker
docker-compose up --build
```

### 3. バックグラウンドで起動する場合
```bash
docker-compose up -d
```

### 4. コンテナの停止
```bash
docker-compose down
```

## アクセス

アプリケーションが起動したら、以下のURLにアクセスできます：

- **API**: http://localhost:8000
- **API ドキュメント (Swagger UI)**: http://localhost:8000/docs
- **API ドキュメント (ReDoc)**: http://localhost:8000/redoc

## 主要なエンドポイント

### 認証関連 (`/api/v1`)
- `POST /login` - ユーザーログイン
- `POST /change-password` - パスワード変更
- `POST /password-reset-request` - パスワードリセット要求
- `GET /verify-token/{token}` - トークン検証

### ユーザー管理 (`/api/v1`)
- `POST /user` - ユーザー作成
- `GET /verify-email` - メールアドレス確認
- `GET /user/{id}` - ユーザー情報取得
- `POST /resend-verification-email` - 確認メール再送信
- `DELETE /user/{id}` - ユーザー削除

### 記事管理 (`/api/v1`)
- `GET /articles` - 記事一覧取得（ログインユーザーの記事のみ）
- `GET /article/{id}` - 特定記事取得
- `POST /articles` - 記事作成
- `PUT /article/{id}` - 記事更新
- `DELETE /articles` - 記事削除
- `GET /articles/search` - 記事検索
- `GET /articles/tag/{tag}` - タグ別記事取得
- `GET /articles/stats/user` - ユーザー統計取得

## 開発

ホットリロードが有効になっているため、`blog-api-main/` ディレクトリ内のファイルを編集すると自動的に反映されます。

## ログの確認
```bash
docker-compose logs -f fastapi
```

## 主な機能

- **認証**: JWT トークンベースの認証
- **ユーザー管理**: メール認証付きユーザー登録
- **記事管理**: CRUD操作、検索、タグ管理
- **CORS設定**: 柔軟なオリジン設定
- **バリデーション**: メールアドレス、入力データの詳細なバリデーション
- **ログ**: カスタムロガーによる詳細なログ記録

## 技術スタック

- **FastAPI**: 0.115.12
- **Python**: 3.11.14
- **Pydantic**: 2.11.1
- **SQLAlchemy**: データベースORM
- **Alembic**: データベースマイグレーション
- **Passlib + Bcrypt**: パスワードハッシュ化
- **pytest**: テスティングフレームワーク
