# DB 勉強

## 3値論理
- SQL の真偽値は TRUE, FALSE に加えて UNKNOWN という値が存在する。比較演算子を使用して値を比較する際に値ではない NULL を比較対象とした場合に判定結果が UNKNOWN となる。
- TRUE, FALSE, UNKNOWN を AND や OR で組み合わせた場合の優劣は以下。
  - AND の場合 FALSE > UNKNOWN > TRUE
  - OR の場合　TRUE > UNKNOWN > FALSE

## 検索結果の加工
- 重複行を排除する場合は DISTINCT を使用。
- 複数のテーブルに対しての検索結果を組み合わせる場合は用途によって以下を使用する。
  - 和集合を求める -> UNION(オプションで ALL を指定した場合は重複を許容する)
  - 差集合を求める -> EXCEPT/MINUS ※ MINUS は oracle で使用
  - 積集合を求める -> INTERSECT

## 式と関数
- CASE WHEN 演算子で場合わけの処理が可能。
- SQLには関数が定義されていて特定の処理をSQLで行わせることが可能。しかしSQLの種類によって処理内容が異なるので使用する場合はリファレンスを確認する。
- COALESCE(コアレス)関数は NULL の値を書き換える際に使用。

## 集計とグループ化
- SUM や MAX で検索結果を集計する場合、COUNT で * を使用しない限りは NULL の行は集計対象とならない。NULL 集計対象とするのならば COALESCE 関数を使用して NULL を他の値に書き換える。
- GROUP BY は検索結果をグループ化する場合に使用するが、グループ化 -> 集計という順番で処理が行われるので、集計関数を使用したグルーピングを行うことは不可能である。グルーピングした集合に対して絞り込みを行いたい場合は、HAVING を使用する。
- 大量のデータを取り扱う場合、予め集計結果を格納するテーブルを用意しておくケースが存在する。これを集計テーブルと呼ばれる。

## 複数テーブルの結合
- 結合クエリの基本形は SELECT ~ FROM (左テーブル) JOIN (右テーブル) ON (結合条件)
- 基本的には右テーブルに存在しない識別子を左テーブルのレコードが参照している場合は、結合時にはそのレコードは表示されない(右外部結合 right outer join)。左テーブルの各レコードをもれなく結合したい場合は左外部結合(left outer join)を使用する。その場合は JOIN 句の前に LEFT を付与する。完全外部結合という結合方法も存在し、その場合は JOIN の前に FULL を付与する。
- 条件にマッチしないレコードが存在した場合に結合結果に含めたくない場合は INNER JOIN を使用し、含めたい場合は OUTER JOIN を使用する。さらに基準となるテーブルの全レコードを結合する場合は LEFT OUTER JOIN, 外部参照するテーブルの全レコードに対して結合を行いたい場合は RIGHT OUTER JOIN を使用する。
  - https://zenn.dev/naoki_mochizuki/articles/60603b2cdc273cd51c59

## トランザクション
- DB では自動コミットモードと呼ばれる、1つのSQL文が実行される場合に裏でコミットが働くモードが存在する。これは設定で解除が可能(MySQL では SET AUTOCOMMIT=0 という SQL を実行する)。
- DBMS に対して複数の利用者が同時に処理を行うことで発生する副作用は以下が存在する。
  - ダーティーリード: ある利用者がトランザクションを張って処理を行なっているとき、その途中のDBの状態を他の利用者から読み取られてしまう副作用。
  - 反復不能読み取り: あるテーブルに対して SELECT 文を実行したのちに UPDATE 文が実行され、その次の SELECT 文で1回目の SELECT 文と異なる検索結果となり整合性がなくなってしまう副作用。
  - ファントムリード: 2回の SELECT 文の実行の間に INSERT 文が実行され、検索結果の行数が変わってしまうという副作用。
- 副作用に対処するために、DBMSでは複数のトランザクション分離レベルが定義されている。用途に合わせて適切なレベルを設定する。

## 様々な支援機能
- インデックスを作成して処理が高速になるケースとしては以下。
  - WHERE による検索処理
  - ORDER BY による並び替え処理
  - JOIN による結合処理
    - 内部で並び替えが行われるため
- インデックスを作成する際のデメリットは以下。
  - インデックスはディスク容量を消費する。
  - INSERT/UPDATE/DELETE 文が発行されるとそれに伴いインデックスも更新されるので、その分処理に時間がかかる。
- 同じSQL文を何回も実行するのを避けるため予めそのSQL文の実行結果をビューとして定義しておき、参照する場合はそのビューを参照するようにできる機能が存在する。ただビューはテーブルと違い一定の条件を満たさないと更新処理が行えないのであくまで仮想的なものに過ぎない。
- DBのバックアップについては以下の2種類が存在する
  - データベースの内容のバックアップ: データベースの内容のバックアップを行う。頻度としては日時、週次、月次などの間隔で行う。それなりにバックアップをするのに時間がかかる。
  - ログファイルのバックアップ: ログファイルにはこれまで実行されたSQL文の内容が記録されている。データを復旧する際にはデータベース自体のバックアップの最新版をベースに最後にデータベースをバックアップした時間帯から実行されたSQL文をログから特定してそれを実行することで復旧する。

# SQL アンチパターン

## 8.
- 同じテーブルに存在するデータを分割して管理したいケースが存在する。目的としては年度毎のデータを管理したいといったような、とある属性別にデータを管理したいという目的と、分割することによりクエリ実行のパフォーマンスを向上させる目的が考えられる。どちらに関しても SQL のパーティション機能を使用すると実現できる。パーティションには水平パーティションと垂直パーティションが存在し、水平パーティションはテーブル定義の際にどういった条件でどれくらいまでデータを分割するかという設定を行うことができる機能のことを指す。垂直パーティションは VARCHAR などの可変長のカラムを外部のテーブルで管理することによりパフォーマンスを向上させるテクニックのことである。SELECT * などで全カラム指定でデータを取得する場合に、VARCHAR のような可変長のカラムは取得するのに時間がかかるのでこういったテクニックが使用される。またパッケージインストーラなどのバイナリファイルは大きいサイズの可変長カラムで格納することになるので、同じテーブルで管理するとパフォーマンスが低下してしまう。したがってそういった場合は積極的に垂直パーティションを使用するのが望ましい。

## 9. FLOAT ではなく、NUMERIC や DECIMAL を使用する
- FLOAT を使用すると丸め誤差が発生してしまうので会計関連などの正確な計算には向かない。代わりに NUMERIC や DECIMAL を使用する。どちらも指定した精度の小数を格納することができる。NUMERICは(9,2)といったように引数を指定して使用する。最初の引数は精度を表し、何桁の数値を格納するかの設定を表す。9の場合は 123456789 は格納できるが 1234567890 は格納できない。2番目の引数はスケールと呼ばれ小数点以下に格納できる桁数を表す。2の場合は 1234567.89 は格納できるが 123456.789 は格納できない。なおスケールの桁数は精度に含まれる。

## 10. サーティワンフレーバー

- MySQL には ENUM という決まった値を格納できるデータ型が存在する。
```sql
mysql> use sample;
Database changed
mysql> create table Bugs (
    -> status ENUM('NEW', 'IN PROGRESS', 'FIXED')
    -> );
Query OK, 0 rows affected (0.13 sec)

mysql> INSERT INTO Bugs VALUES('NEW');
Query OK, 1 row affected (0.08 sec)

mysql> select * from Bugs;
+--------+
| status |
+--------+
| NEW    |
+--------+
1 row in set (0.01 sec)
```

- 将来格納する値のバリエーションが増えることがないのならば ENUM を使用しても良いが、増えることが考えられるのならば ENUM の使用は避けた方が良い。理由の一つとしては値の一覧を取得する際には以下のように複雑なクエリを実行する必要がある。さらに値のみ取得できないのもデメリットの一つ。また Enum をソートする場合はアルファベット順などではなく登録順にソートされるので意図しない挙動になる可能性がある。
```sql
mysql> SELECT column_type FROM information_schema.columns WHERE table_schema = 'sample' AND table_name = 'Bugs' AND column_name = 'status';
+-----------------------------------+
| COLUMN_TYPE                       |
+-----------------------------------+
| enum('NEW','IN PROGRESS','FIXED') |
+-----------------------------------+
1 row in set (0.09 sec)
```

- 決まった値を管理する場合は外部テーブルで定義すると柔軟な設計になる。
```sql
-- 決まった値を管理する BugStatus テーブルを作成する
mysql> CREATE TABLE BugStatus (
    -> status VARCHAR(20) PRIMARY KEY
    -> );
Query OK, 0 rows affected (0.65 sec)

mysql> INSERT INTO BugStatus (status) VALUES ('NEW'), ('IN PROGRESS'), ('FIXED');
Query OK, 3 rows affected (0.18 sec)
Records: 3  Duplicates: 0  Warnings: 0


-- Bug テーブルを別途作成し、status は BugStatus の値を参照するようにする。また `ON UPDATE CASCADE` で BugStatus の更新を Bug テーブルに反映するようにする。
mysql> CREATE TABLE Bugs (
    -> status VARCHAR(20),
    -> FOREIGN KEY (status) REFERENCES BugStatus(status) ON UPDATE CASCADE
    -> );
Query OK, 0 rows affected (0.71 sec)

-- BugStatus に格納されているデータは Bug テーブルにも格納可能
mysql> INSERT INTO Bugs (status) VALUES ('NEW'), ('IN PROGRESS'), ('FIXED');
Query OK, 3 rows affected (0.04 sec)
Records: 3  Duplicates: 0  Warnings: 0

-- BugStatus に格納されていないデータは Bug テーブルにも格納できない
mysql> INSERT INTO Bugs (status) VALUES ('OLD');
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`sample`.`Bugs`, CONSTRAINT `Bugs_ibfk_1` FOREIGN KEY (`status`) REFERENCES `BugStatus` (`status`) ON UPDATE CASCADE)

-- BugStatus が UPDATE されると Bug もそれに追従
mysql> select * from BugStatus;
+-------------+
| status      |
+-------------+
| FIXED       |
| IN PROGRESS |
| NEW         |
+-------------+
3 rows in set (0.00 sec)

mysql> INSERT INTO Bugs (status) VALUES ('FIXED');
Query OK, 1 row affected (0.07 sec)

mysql> UPDATE BugStatus SET status = 'DONE' WHERE status = 'FIXED';
Query OK, 1 row affected (0.09 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from BugStatus;
+-------------+
| status      |
+-------------+
| DONE        |
| IN PROGRESS |
| NEW         |
+-------------+
3 rows in set (0.01 sec)
```

## 11. ファントムファイル
画像のような大容量のメディアファイルをデータベースで扱う場合、設計方法としては大まかに分けて以下の2種類が存在する。
1. 画像ファイルそのものはファイルシステムで管理し、データベースには画像ファイルのパスを格納する。
2. 画像ファイルそのものを BLOB などのデータ型で格納する。

1.に関しては以下のようなデメリットが存在する。
- 画像を削除したい場合、レコードの削除に加えて画像ファイルそのものもさ駆除しなければならない。
- 削除や更新処理でトランザクションを張っている場合は、画像ファイルの変更はCOMMITする前に反映されてしまう。
- ロールバックを行うことができない。
- バックアップ時にもデータベースの状態とファイルの内容で不整合が発生する可能性がある。
- 外部ファイルには SQL におけるアクセス権限を付与することができない。
- ファイルのパスを格納する場合データ型を文字列で指定すると、それが正しいパスなのかを検証する術がない

ただデータベースの容量を減らしたい場合や、画像の編集やプレビューを容易にしたり一括で画像を修正したい場合などは1.の方針を採用するのも一つの手である。ただそれ以外のケースでは上記のデメリットを解消することができる2.の方針を採用するのが望ましい。

## 12. インデックスショットガン

以下のテーブルを用いてインデックスを作成する際の注意点をまとめる。
```sql
mysql> CREATE TABLE Bugs(
    -> bug_id SERIAL PRIMARY KEY,
    -> data_reported DATE NOT NULL,
    -> summary VARCHAR(80) NOT NULL,
    -> status VARCHAR(10) NOT NULL,
    -> hours NUMERIC(9,2),
    -> INDEX (bug_id), -- 1.
    -> INDEX (summary), -- 2.
    -> INDEX (hours),
    -> INDEX (bug_id, data_reported, status)  -- 3.
    -> );
```
1. 大抵の場合主キーのインデックスは自動的に生成されるので、わざわざ明示的に作成する必要はない。
2. `summary` のような長い文字列のインデックスを作成すると他に比べてサイズが大きくなる。
3. 複合インデックスを作成する場合は、検索条件、ソート条件、結合条件を指定する際には定義した列順通りに指定しなければならない(この場合は bug_id, data_reported, status の順番)。

上記の例からも分かるとおり、闇雲にインデックスを作成するのは得策ではない。したがって作成することによってパフォーマンスが向上するインデックスはどんなものがあるかを分析する必要があるが、その分析方法の指針の一つに `MENTOR` というものがある。MENTOR という文字列はそれぞれのフェーズの頭文字から構成されており、各フェーズの説明を以下に示す。

|  フェーズ  |  説明  |
|:--:|:---|
|  Measure  |  SQL のパフォーマンスを測定する。Oracle では SQL のトレース機能とトレース結果のレポートを出力してくれる TKPROF というツールが存在し、MySQL では指定された閾値よりも長く処理時間がかかったクエリを記録するスロークエリログという機能が存在する。これらを使用してどのクエリが一番時間がかかっているのかを確認する。  |
|  Explain  |  実行時間が長いクエリについて分析を行う。データベースはクエリ実行計画(QEP)という機能によってどのインデックスを使用するかを決定している。その QEP のレポートからそのクエリがどのインデックスを使用しているのかを確認する。  |
|  Nominate  |  QEP を確認したらそのクエリの中でインデックスを使用していない箇所を特定する。一部のデータベースではパフォーマンス向上のためのクエリの修正の提案を行ってくれるツールが存在し、MySQL では MySQL Enterprise Query Analyzer, Oracle では Oracle SQL Tuning Advisor がそれに該当する。 |
|  Test  |  修正したクエリのパフォーマンスをテストする。  |
|  Optimize  |  データベースサーバのキャッシュメモリの最適化を行う。インデックスはコンパクトで使用頻度が高いデータなのでキャッシュに格納されやすい。データベースはキャッシュに割り当てるシステムメモリの量を設定することができるので適切なサイズに設定する。 |
|  Rebuild  |  インデックスのメンテナンスを行う。長期にわたり更新や削除が行われると、インデックスが不均衡になってしまう。基本的にインデックスが均衡が取れている方が効率は良くなるので、定期的にインデックスのメンテナンスを行うことには一定の価値がある。 |

## 13. フィア・オブ・ジ・アンノウン

データに NULL が登録されている場合はクエリ実行時には予期せぬ実行結果を取得してしまうケースが存在する。例として、first_name と last_name を格納している Accounts テーブルに新しくミドルネームのイニシャルを格納する middle_initial 列を追加して、それぞれの値を結合してフルネームを取得するクエリを実行したとする。実行結果として middle_initial 列が NULL の場合は first_name と last_name を結合した文字列が取得できることを期待していたが、実際には以下のように NULL が取得されてしまった。

```sql
mysql> CREATE TABLE Accounts(account_id SERIAL PRIMARY KEY, first_name VARCHAR(10) NOT NULL, last_name VARCHAR(10) NOT NULL);
mysql> INSERT INTO Accounts (first_name, last_name) VALUES ('John', 'Smith');
Query OK, 1 row affected (0.13 sec)

mysql> INSERT INTO Accounts (first_name, last_name) VALUES ('Tom', 'Cat');
Query OK, 1 row affected (0.07 sec)

mysql> ALTER TABLE Accounts ADD COLUMN middle_initial CHAR(2);
Query OK, 0 rows affected (0.52 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM Accounts;
+------------+------------+-----------+----------------+
| account_id | first_name | last_name | middle_initial |
+------------+------------+-----------+----------------+
|          1 | John       | Smith     | NULL           |
|          2 | Tom        | Cat       | NULL           |
+------------+------------+-----------+----------------+
2 rows in set (0.77 sec)

mysql> SELECT first_name || ' ' || middle_initial || ' ' || last_name As full_name FROM Accounts;
+-----------+
| full_name |
+-----------+
|      NULL |
|      NULL |
+-----------+
2 rows in set, 8 warnings (0.19 sec)
```

上記の例を踏まえて、NULLのデータを扱う場合は注意点が存在する。ケース別の注意点を以下の表にまとめる。

|  ケース  |  例  |  結果  |  説明  |
|:--:|:---|:---|:---|
|  比較  |  NULL = 0 |  NULL  |  NULL との比較結果は NULL  |
|  加算  |  NULL + 1234  |  NULL  |  NULL に値の加算を行う場合、0として扱われず結果は NULL となる |
|  文字連結  |  NULL + 'hoge'  |  NULL  |  NULL に文字を連結すると NULL になる  |

このように NULL の場合は想定とは異なるクエリの実行結果を取得してしまうので、事前に NULL チェックを行っていく必要がある。NULL チェックには `IS (NOT) NULL`, `IS (NOT) DISTINCT FROM` が使用できる。

```sql
-- IS NULL
mysql> SELECT * FROM Accounts WHERE middle_initial IS NULL;
+------------+------------+-----------+----------------+
| account_id | first_name | last_name | middle_initial |
+------------+------------+-----------+----------------+
|          1 | John       | Smith     | NULL           |
|          2 | Tom        | Cat       | NULL           |
+------------+------------+-----------+----------------+
2 rows in set (0.01 sec)

-- 以下の2つのクエリは等価
mysql> SELECT * FROM Accounts WHERE middle_initial IS NULL OR middle_initial <> 'J.';
+------------+------------+-----------+----------------+
| account_id | first_name | last_name | middle_initial |
+------------+------------+-----------+----------------+
|          1 | John       | Smith     | NULL           |
|          2 | Tom        | Cat       | NULL           |
+------------+------------+-----------+----------------+
2 rows in set (0.07 sec)

mysql> SELECT * FROM Accounts WHERE NOT (middle_initial <=> 'J.'); -- MySQL では IS NOT DISTINCT FROM と等価な演算子 <=> を提供している
+------------+------------+-----------+----------------+
| account_id | first_name | last_name | middle_initial |
+------------+------------+-----------+----------------+
|          1 | John       | Smith     | NULL           |
|          2 | Tom        | Cat       | NULL           |
+------------+------------+-----------+----------------+
2 rows in set (0.41 sec)
```

また、最初に紹介した例のように、middle_initial 列が NULL の場合は first_name と last_name を結合した文字列を取得するためには、COALESCE 関数を使用すると良い。

```sql
-- ' ' || middle_initial || ' ' の値が NULL の場合、' ' が文字連結される
mysql> SELECT first_name || COALESCE(' ' || middle_initial || ' ', ' ') || last_name As full_name FROM Accounts;
+--------------+
| full_name    |
+--------------+
| John Smith   |
| Tom Cat      |
| Hoge J. Fuga |
| Piyo R. Foo  |
+--------------+
4 rows in set (0.06 sec)
```