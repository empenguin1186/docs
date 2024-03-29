# スッキリわかる SQL 入門

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
