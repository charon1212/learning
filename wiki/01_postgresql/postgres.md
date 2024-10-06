# postgresql

## Get Started

- docker run/stop

```sh
docker run --name postgres -e POSTGRES_PASSWORD=postgres -p 15432:5432 -d postgres
docker ps -a
docker stop postgres
```

- login(windows)

```sh
set PGPASSWORD=postgres && psql -U postgres -d postgres -p 15432
```

## Meta Command

基本的なメタコマンド

| command | 意味 | 概要 | リンク |
| -- | -- | -- | -- |
| \l(\list) | list | データベース一覧 | <https://www.postgresql.jp/document/16/html/app-psql.html#APP-PSQL-META-COMMAND-LIST> |
| \c(\connect) {database} | connect database | データベース切り替え | <https://www.postgresql.jp/document/16/html/app-psql.html#APP-PSQL-META-COMMAND-C-LC> |
| \dt | describe tables | テーブル一覧 | <https://www.postgresql.jp/document/16/html/app-psql.html#APP-PSQL-META-COMMAND-D> |
| \d {テーブル名} | describe | テーブルの詳細情報を表示 | <https://www.postgresql.jp/document/16/html/app-psql.html#APP-PSQL-META-COMMAND-D> |
| \du | describe users | ユーザ一覧を表示 | <https://www.postgresql.jp/document/16/html/app-psql.html#APP-PSQL-META-COMMAND-DU> |
| \i(\include) {filename} | include file | 指定したSQLファイルを実行 | <https://www.postgresql.jp/document/16/html/app-psql.html#APP-PSQL-META-COMMAND-C-LC> |
