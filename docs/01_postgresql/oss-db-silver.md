---
layout: default
title: oss-db-silver
nav_order: 1
parent: postgresql
---

# oss-db-silver

## 1.オープンソースデータベースの一般的特徴

## 2.データベースの基礎知識

- データモデルの区別
  - 概念データモデル: 対象世界を単にモデル化したもの
  - 論理データモデル: DB構造に応じて、実装可能なデータモデル
- 論理データモデルで利用される3モデル
  - 関係モデル: いわゆる **RELATION** モデル。RDB(Relational Database)にも書かれるように、複数の表を組み合わせてデータ構造を表現する。一つ一つの表を関係（リレーション）という。
  - 階層モデル: Entityデータをレコードとして表現し、それらのレコードに親子関係を作ってデータ構造を表現する。
  - ネットワークモデル: 階層モデルの多:多バージョン。
- 関係モデルにおける用語定義
  - 候補キー: 列の組み合わせのうち、その列のみで一意に行を特定でき、かつそれ以上列を削ると一意に特定できなくなるものをいう。列`A,B,C`で一意に特定できる状態を`UNI(A,B,C)`と書くならば、「`(A1,A2,...,An)`が候補キーであるとは、 `UNI(A1,A2,...,An) ∧ (∀i=1,2,...,n ￢UNI(A1,...,Ai-1,Ai+1,...,An))`」と表現できる。
  - 主キー: 候補キーの内、一意制約と非NULL制約を満たす中でリレーションごとに任意に選んだ列の組み合わせを指す。
  - 非キー属性: いずれの候補キーにも含まれない列のこと
  - 関数従属: テーブルの列の組`X=(x1,...,xn)`と`Y=(y1,...,ym)`について、「YはXに関数従属する」とは、「Xの各列の値を決定すれば、それに対する全てのレコードのYの列が一意に決まること」をいう。
- 正規形
  - 第1正規形は「列挙がない」（単純で繰り返し項目がない）
  - 第2正規形は「部分従属がない」（非キー属性が候補キーの完全関数従属である）
  - 第3正規形は「推移従属がない」（非キー属性が、推移従属でない）
  - ボイスコット正規形は「候補キー同士の1:1関係がない」（どっち主キーでも良いんだけど、どっちか片方にしろよがない）

## 4. 標準付属ツール

- `initdb`
  - `-D,--pgdata=<ディレクトリ名>`: データベースクラスタのディレクトリパス。デフォルトは`$PGDATA`
  - `-E,--encoding`: エンコーディング。デフォルトはOSから取得
  - `--locale,--no-locale`: ロケール(無し)。デフォルトはOSから取得
  - `-U,--username`: DB作成時のデータベースユーザのユーザ名。デフォルトはinitdb実行ユーザ名になる
- `pg_ctl start`
  - `-t`: 最大待ち時間（デフォルト60秒）　超過してもコマンドが戻るだけで処理は継続
- `pg_ctl stop`
  - `-W`: 停止完了を待たない
  - `-t`: 最大待ち時間（デフォルト60秒）　超過してもコマンドが戻るだけで処理は継続
  - `-m`: シャットダウンモード
    - `s[mart]`: スマートシャットダウン。新規接続は受けないが、既存接続の完了を待つ。
    - `f[ast]`: 高速シャットダウン（デフォルト）。実行中のトランザクションはロールバックされ、クライアント接続を強制切断。
    - `i[mmediate]`: 即時シャットダウン。クラッシュと同等で、再起動時に復旧が必要。障害試験などで利用。
- `pg_ctl restart`
  - `-t`,`-m`: stopと同様
- `pg_ctl reload`: 設定ファイル反映。postgresql.confとpg_hba.conf。
- `pg_ctl status`: 起動状態確認
- `pg_ctl kill <シグナル> <PID>`: linuxのkillコマンドと同様に、プロセスIDへシグナルを送る。
  - シグナルは、TERM→スマートシャットダウン、INT→高速シャットダウン、QUIT→即時シャットダウン、HUP→設定ファイルreload、等々
- `createuser <ユーザ名>`
  - `-P`: パスワード設定
  - `-s(-S)`: スーパーユーザ権限を設定（しない）
  - `-d(-D)`: データベース作成権限を設定（しない）
  - `-r(-R)`: ロール作成権限を設定（しない）
  - `-l(-L)`: ログイン権限を設定（しない）
  - `--interactive`: 対話的に設定
  - `-e,--echo`: SQLを出力
- `dropuser <ユーザ名>`
  - `-i,--interactive`: 削除して良いか確認
- `createdb <データベース名>`
  - `-E,--encoding`: エンコーディング
  - `-l,--locale`: ロケール
  - `-O,--owner`: 所有者となるユーザ (スーパーユーザ権限を持つ場合のみ利用可能)
  - `-T,--template`: DBテンプレート
- `dropdb <データベース名>`
  - `-i,--interactive`: 削除して良いか確認
- `psql`: いろいろ接続
  - スーパーユーザ権限だと、ターミナルが`=#`になる。一般ユーザだと、`=>`
  - コマンドの2行目以降は、`=`が`-`になる。（`-#`とか`->`とか。）
- psqlメタコマンド：
  - `\q`: 終了
  - `\l`: DB一覧
  - `\d <パターン>`: 対象のテーブル・インデックス・ビュー・シーケンスの情報取得。「*」や「?」のワイルドカード利用可能。
  - `\du`: ユーザ一覧
  - `\copy`: クライアント側のコピーコマンド
  - `\password <ユーザ名>`: パスワード変更(デフォルトは現在のユーザの変更)
  - `\c <データベース名>`: DB切り替え
  - `\?`: メタコマンド一覧
  - `\h <SQLコマンド>`: SQLコマンドのヘルプ
- `pg_controldata`: データベースクラスタの制御情報を表示
- `pg_resetwal`: WALを消去して制御情報を初期化。PostgreSQL停止時に実行すること。
- `pg_isready`: 稼働状況確認
- `pg_config`: インストール設定情報を調べる
- `vacuumdb`: VACUUMやANALYZEを実行するためのコマンド
  - `-a(--all)`: 全てのDBに対して実行
  - `-d(--dbname)`: 実行するDB名
  - `-e(--echo)`: SQLを標準出力に
  - `-f(--full)`: テーブル全体に対して完全なVACUUMを実行。
  - `-q(--quiet)`: 出力を最小限に抑える
  - `-t(--table)`: テーブル指定。複数指定可。
  - `-v(--verbose)`: 詳細を出力
  - `-z(--analyze)`: analyzeも実行する
  - `-Z(--analyze-only)`: anlyzeだけを実行する

## 5. 設定ファイル

- postgresql.conf: `<パラメータ名> = <設定値>`で設定値を列挙する。
  - 1行に1パラメータ。`=`は省略して空白で代替できる。パラメータ名は大文字小文字を区別しない。
  - 設定値は基本的にそのまま書く。カンマや空白を含める場合、`'`でエスケープする。
  - 単位は、メモリ系で`kB,MB,GB`、時間系で`ms,s,min,h,d`。
  - 反映は厳しい順に、 変更不可 > 再起動必要 > reload必要 > superuserのみSET文 > 誰でもSET文 の5種類。
- listen_addresses: 指定したIPアドレス（PostgreSQLサーバ側）のみ監視する。その他のIPへの接続は無視する。
  - 空だと一切接続を受け付けないことになる。構築初期は`'*'`が指定されていた（全接続を監視）。
- ログ
  - まず、`log_destination`で標準エラー出力・syslog(linux)・eventlog(windows)へログ出力ができる。
  - 続いて、`logging_collector`,`log_directory`,`log_filename`で、標準エラー出力をファイルへ出力できる。
  - `log_min_message`: 最低ログレベル
  - `log_line_prefix`: ログの接頭辞。デフォルトは`%m[%p]`で、タイムスタンプとPID。`%u`:ユーザ名、`%d`:データベース名、`%t`:タイムスタンプ、`%%`:`%`のエスケープ
- SET文でセッション内のみパラメータを変更できる。トランザクション開始後に「SET LOCAL」を使えば、トランザクション内に限定できる。
  - `SET <param> TO DEFAULT`でデフォルトに戻すことができる。（postgresql.confにある場合はその値に、ない場合はPostgreSQLのデフォルトへ戻る）

## 6. バックアップとリストア

- バックアップコマンド
  - DBクラスタ全体のバックアップ: `pg_dumpall`
    - 平文形式のみ。
    - `pg_dumpall -f <ファイル名>`や、`pg_dumpall > <ファイル名>`で実行。
  - 個別のデータベースをバックアップ: `pg_dump`
    - 平文・カスタム・tar形式を指定可能。（`-Fp (Format Plain)`,`-Fc (Format Custom)`,`-Ft (Format Tar)`）
    - `pg_dump mydatabase1 -Fp -f <ファイル名>`等で実行。`-f`のファイル名指定か、リダイレクトで送るか。
- リストアコマンド
  - 平文形式である、`pg_dumpall`や`pg_dump -Fp`は、`psql`コマンドでリストアする。 `psql -U user -P -f <バックアップファイル名>`
  - それ以外は、`pg_restore`で復元する。 `pg_restore -d restoredb <バックアップファイル名>`
- PITR(Point in Time Recover): ある時点のバックアップとそこからのWAL+WALアーカイブで復旧する方法。
  - 事前にpg_basebackupや低レベルAPI(cpコマンドとか)でベースバックアップを取得する。
  - 障害発生時は以下の手順を行う。
    1. ベースバックアップをリストア
    1. postgresql.confのrestore_commandパラメータにWALアーカイブからWALファイルをコピーするcpコマンドを設定
    1. recovery.signalというからファイルを作成
    1. PostgreSQLを起動
  - cpコマンド等でベースバックアップを取得する場合は、`pg_start_backup`関数でバックアップを作成し、cpコマンドで退避後、`pg_stop_backup`関数を実行する。スーパーユーザ権限が必要。

## 7. 基本的な運用管理

- `VACUUM <テーブル名>`: 不要領域を回収するコマンド。テーブル名を指定しないと、全てのテーブルに対して実行する。
  - FULL: VACUUM FULLは、テーブルの内容を新しいファイルに書き換えて、物理的に不要領域を削除する。コストがかかるほか、排他ロックも必要で、定期実行するものではない。VACUUMのみであれば、ロックは不要でコストも比較的低い。
- `ANALYZE <テーブル名>`: 統計情報を更新するコマンド。テーブル名を指定しないと、全てのテーブルに対して実行する。
- 自動バキューム: 設定値`autovacuum`がONになっている場合、VACUUMとANALYZEが自動的に実行される。定期実行ではなく、データベース内の不要領域の割合が多くなったことを検知したタイミングで実施する。
- `GRANT`: ユーザー権限を設定する。
  - `GRANT SELECT ON TABLE items TO user1,user2`: user1,user2がitemsテーブルをSELECTできる
  - `GRANT ALL ON TABLE catalog TO PUBLIC`: 誰でもcatalogテーブルを操作できる

## 8. SQLとオブジェクト

- リテラル表現はシングルクォート`'`。ダブルクォート`"`は、大文字小文字を区別する変数名に利用する。
- 3種類のJOIN:
  - INNER JOIN: 主のテーブルを元に従のテーブルを探してくっつける。複数見つかる場合は、その分だけ出力。見つからない場合は出力しない。
  - OUTER JOIN: 主のテーブルを元に従のテーブルを探してくっつける。複数見つかる場合は、その分だけ出力。見つからない場合はNULL値としてくっつける。
  - CROSS JOIN: 掛け算（3×4）
- SELECT構文

```sql
-- ASの別名・WHERE条件・LIMIT-OFFSET・IN NOT IN・ANY
SELECT name AS username FROM user_data
  WHERE user_type NOT IN ('type-A', 'type-B')
    AND user_class = ANY(SELECT cl FROM user_class WHERE classorder > 5)
  LIMIT 20 OFFSET 100;
-- GROUP-HAVING  Aクラスの生徒の内、いずれかの科目が50点以上の生徒の、トータルスコア表　（評価順はWHERE => GROUP => HAVING）
SELECT student_name, sum(score) AS total_score FROM student_test_score WHERE student_class='A' GROUP BY student_name HAVING max(score) >= 50;
```

- WHERE句の正規表現
  - `SIMILAR TO`を使うパターンマッチング
    - SQL用の正規表現。標準SQLに準拠。任意の文字列は`%`でマッチし、任意の1文字は`_`でマッチする。
  - パターンマッチング（~, !~, ~*）
    - POSIXと同様の正規表現。
    - 例：`WHERE name !~*'^a'`は、!なので否定、*なので大小文字を区別しないため、「aまたはAで始まらないレコード」を取得する。

- 数値型：smallint, integer・int, bigint, decimal・numeric, real, double precision
- 文字列型： char(n)・character(n)  空白埋めの固定長、 varchar(n)・character varying(n)  上限指定の可変長、 text  任意長の文字列
  - ※char = char(1), varchar = varchar(∞) = text (※上限1GB)
- バイト型：bytea
- 日付/時刻型： timestamp(日付+時刻)、date(日付)、time(時刻)、interval(期間)
  - リテラル例：`SELECT timestamp '2020-01-02 03:04:05' - interval '1 weeks 3.5 day';`  （weekやdayは複数形もOK）
- 論理値：boolean
  - リテラル値：`'true'/'t'/'yes'/'y'/'on'/'1'/TRUE`,`'false'/'f'/'no'/'n'/'off'/'0'/FALSE`
- 連番型：smallserial,serial,bigserial
  - 自動でシーケンスが作成される
- JSON型：json, jsonb
  - `SELECT jsonb_path_query(json_column, '$.name.somaArray[1].someArray[*]') FROM some_json_data;`
- キャスト：`::`を使う。`'123'::integer`
  - `'hoge'::regclass`でOID取得可能

- INDEX作成

```sql
-- 関数INDEX・INDEX種類指定・部分インデックス
CREATE INDEX idx01 ON some_table(col1, upper(col2)) USING [btree/gist/gin/hash] WHERE col1 < 100;

CREATE TRIGGER some_trigger BEFORE UPDATE TRUNCATE ON some_table EXECUTE PROCEDURE some_proc;
```

- レプリケーション: メイン・スタンバイ構成等の冗長構成をするときのデータ同期方法。データ移行はマイグレーション。
  - ストリーミングレプリケーション: WALを全部スタンバイに送って適用する方法。DBクラスタ全体をレプリする。
  - ロジカルレプリケーション：WALをデコードして獲られた変更をスタンバイに転送して適用する方法。論理的な変更内容が分かるため、レプリ単位は自由にできる（DB単位・テーブル単位）
    また、アーキテクチャ・プラットフォームなどが異なっていても適用できるが、TRUNCATEを除くDDLは複製されない。（テーブル作成などはそれぞれで実施しないといけない）

## 9. 組み込み関数と演算子

## 10. トランザクション

- コマンド：BEGIN(START TRANSACTION), COMMIT(END), ROLLBACK(ABORT)
- SAVEPOINT

## 試験用のメモ

- 識別子
  - カッコ有り：version(), current_database(), now()
  - カッコ無し：user, session_user, current_timestamp, current_user, current_date, current_time, localtimestamp, localtime
