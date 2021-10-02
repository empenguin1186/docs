# 最初に
[SQLアンチパターン](https://www.oreilly.co.jp/books/9784873115894/) を買って1章のジェイウォークについて読んだので、内容を自分用にまとめる。

# 背景
以下のようなクエリで作成される `Products` (製品)テーブルを考える。
```sql
CREATE TABLE Products (
  product_id SERIAL PRIMARY KEY,
  product_name VARCHAR(1000),
  account_id BIGINT UNSIGNED,
  FOREIN KEY (account_id) REFERENCES Accounts(account_id)
)
```
`account_id` は外部の `Accounts` (連絡先)テーブルを参照している。今は1つの製品に対して1つの連絡先IDが紐づく設計となっており、データ型を数値型としている。<br>
しかし後に1つの製品に対して複数の連絡先を紐付けることが可能となるように仕様が変更され、それに伴いテーブルの設計を見直す必要がある。

# アンチパターン
テーブル構造の修正を最小限に抑えるために `account_id` のデータ型を数値型から文字列型に変更して複数の連絡先を紐付けられるようにした。テーブルにレコードを追加する場合は以下のようなクエリを実行することになる。ここでは連絡先IDをカンマ区切りで複数登録するようにしている。
```sql
INSERT INTO Products VALUES (DEFAULT, 'ProductA', '1,2')
```

# アンチパターンによるデメリット
上記に関しては確かにテーブル構造の修正は最小限に抑えられるが、その代わり様々なデメリットが存在する。具体的な内容としては以下。<br>
- 検索や集約処理を行う場合にパターンマッチや区切り文字の除去を行わなければならなくなり、メンテナンス性、パフォーマンスなどが悪化する。
- 文字列型にすることにより数値ではなく `hoge` といった `account_id` として不正な入力を許容してしまう。
- 数値ではなく文字列を区切り文字を使用して1つのレコードに登録する場合、区切り文字なのか文字列の一部なのかが不明瞭になる可能性がある。
- 登録される文字列の長さによって、各製品に紐づけることができる `account_id` の個数の上限が変動する。例えば VARCHER(30) の場合、文字数2の連絡先IDは10個まで登録でき(カンマ含めて1ID3文字使用)、文字数6の連絡先IDは4個まで登録できる。

# 解決策
解決策としては交差テーブルを作成する方法が存在する。以下のようなクエリでテーブルを作成する。<br>
```sql
CREATE TABLE Contacts (
  product_id BIGINT UNSIGNED NOT NULL,
  account_id BIGINT UNSIGNED NOT NULL,
  PRIMARY KEY (product_id, account_id),
  FOREIGN KEY (product_id) REFERENCES Products(product_id),
  FOREIGN KEY (account_id) REFERENCES Accounts(account_id)
)
```
`Contacts` テーブルは `Products`、`Accounts` テーブルを参照する外部キーを持つので交差テーブルとなる。交差テーブルは参照している2つのテーブルに多対多の関連をもたらす。これによってアンチパターンによるデメリットを解消することができる。<br>

|  デメリット  |  説明  |
|:--:|:---|
|  メンテナンス性/パフォーマンスの悪化  |  `Contacts` テーブルを結合することでクエリをシンプルにすることができる。またインデックスを有効活用することができるのでパフォーマンスも向上する。  |
|  不正な入力の許容  |  `Contacts` の `account_id` は文字列型にする必要がなくなったので、不正な入力を許容しなくなる。 |
|  区切り文字  |  (produt_id, account_id) を個別のレコードに登録するようになったので、区切り文字が不要となる。 |
|  登録できるエントリの上限の変動  |  `Contacts` テーブルを用意したことで、個々の製品で紐付けられる連絡先の制限が変動するということはなくなる。ただしテーブル自体には物理的に格納できるレコードの上限は存在する。  |
