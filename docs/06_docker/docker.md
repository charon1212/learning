---
layout: default
title: docker
nav_order: 6
parent: Home
---

# docker

## docker-compose

### docker-compose.yamlの書き方

```yaml
services:
  some-app: # アプリ名。任意の名前。

    image: hoge/some-app-v2 # イメージURL

    container_name: some-app # コンテナイメージ名

    ports:
      - "9876:80" # <ホスト側ポート>:<コンテナ側ポート>で指定する。この場合、localhost:9876がコンテナの80ポートに転送される。

    volumes:
      - ./log:/some-app/archive/log # <ホスト側の>:<コンテナ側のマウントパス>でボリュームを指定する
      - volume-name:/some-app/data # 左辺は`docker volume create`で作成できるボリュームを登録することもできる

    environment:
      SOMEAPP_ID: user # <key>: <value>形式で環境変数を登録する
      SOMEAPP_PASS: pass

    restart: unless-stopped # 再起動するかどうかの設定(詳細は後述)

    mem_limit: 2g # メモリ上限の設定。version2までの古い指定方法だが簡単に指定でき、2025/04時点では非推奨だけど使える。
    cpus: 2.0 # CPU上限の設定。version2までの古い指定方法だが簡単に指定でき、2025/04時点では非推奨だけど使える。

    depends_on: some-db # コンテナ間の依存関係を定義する。起動順序の制御に使える。

  some-db: # 同時にデプロイする他のアプリも記述可能。
    container_name: some-db
    # ...(以下省略)
```

- `restart`:
  - `no`(default): どのような場合でも、自動的に再起動しない。
  - `always`: コンテナが終了した場合と、Docker daemonが再起動した場合、常に再起動する。コンテナを手動で止めて「Exited」ステータスになっていても、Docker daemonを再起動したら再起動する。
  - `unless-stopped`: コンテナが終了した場合、再起動する。また、Docker daemonを再起動した場合、その前に起動中だった（Exitedステータス以外だった）場合は、連動して再起動する。
  - `on-failure`: コンテナが異常終了（非ゼロステータスコード）の場合、再起動する。Docker daemonを再起動した場合、その前のステータスが「Exited(0)」でない限り、連動して再起動する。

### よく使うコマンド

- `docker-compose up -d`: docker-compose.yamlに従ってサービスを作成して起動する。`-d`はバックグラウンド実行のオプション。
- `docker-compose down  -->  docker-compose up -d`: docker-compose.yamlを変更した場合の変更内容の反映。
