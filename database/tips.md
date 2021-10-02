# rust の SQL ライブラリ diesel と MySQL 連携

## 手順
### MySQL コンテナの起動~table作成まで

```
$ docker run -it --name test-mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=mysql -d mysql:latest
$ mysql -u root -p -h 127.0.0.1 -P 3306
```

### diesel との連携

```
$ echo DATABASE_URL=mysql://root:mysql@127.0.0.1/diesel_demo > .env
```

コマンド一覧
https://qiita.com/CyberMergina/items/f889519e6be19c46f5f4

## つまづいた点

### MySQL 関連

#### VARCHAR について

MySQL において VARCHAR 型で文字列の制約をつけないとエラーとなる。
```
mysql> CREATE TABLE posts (id SERIAL PRIMARY KEY, title VARCHAR, body TEXT not null, published BOOLEAN not null DEFAULT 0);
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ', body TEXT not null, published BOOLEAN not null DEFAULT 0)' at line 1
```
https://dev.mysql.com/doc/refman/8.0/ja/string-type-syntax.html

# その他 tips
MySQL の utf8 は 1~3 バイトまで対応している。4バイトまで対応させたい場合は、utf8mb4 を使用する。
https://penpen-dev.com/blog/mysql-utf8-utf8mb4/