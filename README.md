# Rails API

- Ruby 3.2.2
- Rails 7.0.0
- MySQL 8.0

## M1 Mac のみ設定

compose.yml に追記

```bash
db:
  platform: linux/x86_64
```

Dockerfile に追記 (nokogiri 対策)

```bash
Run bundle config set --global force_ruby_platform true
```

## 初期設定

Rails プロジェクト作成

```bash
$ docker-compose run --no-deps app rails new . --force --api --database=mysql -T --skip-bundle
```

- --no-deps リンクしたサービスを起動しない
- --force 既存の Gemfile を上書きするための設定
- --api API に必要なファイルのみ生成
- --database=mysql データベースを MySQL に指定
- -T minitest の生成をしない（RSpec を使用したいため）
- --skip-bundle bundle install をスキップする（後で docker-compose build するため）

database.yml の設定変更(config/database.yml)

```bash
password: password
host: db
```

ビルドして起動

```bash
$ docker-compose up -d --build
```

データベース作成

```bash
$ docker-compose exec app rails db:create
```
