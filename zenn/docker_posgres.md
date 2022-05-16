# 任意のバージョンの pg_dump を実行する

# 概要
PostgreSQL のデータをダンプする際に pg_dump コマンドを使用しているのですが、CLI のバージョンと接続先の PostgreSQL のバージョンに差異があると以下のようなエラーが発生してダンプに失敗します。

```sh
$ pg_dump -h ${POSTGRES_HOST} -p ${POSTGRES_PORT} -U ${POSTGRES_USER} -d ${POSTGRES_DB} -v -a -f database.dump -W
パスワード:
pg_dump: サーババージョン: 12.9、pg_dump バージョン: 9.2.24
pg_dump: サーババージョンの不整合のため処理を中断しています
```
対応としては CLI のバージョンを接続先のバージョンに合わせることが挙げられますが、ネットで検索すると様々な記事で対応方法が紹介されています。

- [既存環境に影響を与えずに pg_dump をインストールする](https://zenn.dev/dai0916/articles/a593ccd9ab3773b2b04f)
- [ソースファイルからPostgreSQLをインストールする手順（前編） | アシスト](https://www.ashisuto.co.jp/db_blog/article/20150918_pg_install_1.html)

自分も上記の記事などを参考にダンプを行なっていましたが、ダンプを実行するために `configure` をはじめとした複数のコマンドを実行しなければならなかったり、依存ライブラリがインストールされていないためにエラーが発生したりしていたので、もっと容易にダンプを実行できないかどうか考えていました。結果 docker コマンドを使用してダンプを実行することにしました。

# コマンド&動作確認
実際に実行したコマンドは以下になります。`POSTGRES_VERSION` には接続先の PostgreSQL のバージョンを指定します。実行後ダンプファイルが作成されていることが確認できます。
```sh
$ docker run -v "$(pwd):/dump" -it --rm --network host postgres:${POSTGRES_VERSION} pg_dump -h ${POSTGRES_HOST} -p ${POSTGRES_PORT} -U ${POSTGRES_USER} -d ${POSTGRES_DB} -v -a -f /dump/database.dump -W 
$ ls database.dump
database.dump
```

また `POSTGRES_VERSION` が接続先の PostgreSQL のバージョンではない場合には以下のようなエラーが発生します。
```sh
$ docker run -v "$(pwd):/dump" -it --rm --network host postgres:${POSTGRES_VERSION} pg_dump -h ${POSTGRES_HOST} -p ${POSTGRES_PORT} -U ${POSTGRES_USER} -d ${POSTGRES_DB} -v -a -f /dump/database.dump -W 
Password: 
pg_dump: server version: 12.10 (Debian 12.10-1.pgdg110+1); pg_dump version: 10.20 (Debian 10.20-1.pgdg90+1)
pg_dump: aborting because of server version mismatch
```
