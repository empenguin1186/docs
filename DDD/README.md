# 現場で役立つシステム設計の原則

## データベース設計について (IPhone p2807 ~)

データベース設計において守るべき原則は以下の3点
- Not Null 制約
- 一意性  
- 外部キー制約

また、データベースは「状態」ではなく「コト」を記録する。

### 同一の処理で異なるタイミングで記録されるデータについて

同一の処理で異なるタイミングで記録されるデータについては、別々のテーブルで記録した方が良い。なぜなら、同じテーブルで記録する場合には、最初にデータが記録された時と次にデータが記録された時の間でNULLが存在することになるからである。

### 記録の更新には UPDATE ではなくDELETEとINSERTを使う
「コト」の記録をUPDATEで更新するのは、過去の事象を改ざんすることに他ならないので望ましくない。データを更新するという「コト」を記録してさらに更新先のレコードを追加する。そうするとデータベースに存在するデータは以下の3つとなる。

- 元データ
- 更新したという記録
- 新データ

## 「コト」の記録と「状態」の記録

前述した通りの設計でデータベースを設計すると、小さなテーブルが複数作成される。それらを駆使してクライアントが必要とするデータを作成するが、処理コストの影響で、「状態」そのもののデータをダイレクトに取得したいというニーズが存在する。そういった場合にはどうしたら良いか。

### 「状態」を表すテーブル
複雑な処理なしで状態を参照したい場合は、「コト」のテーブルとは別に「状態」のテーブルを二次的に再構築可能なテーブルとして記録する。「コト」のテーブルに更新があったら、それに基づいて「状態」のテーブルも更新されるように設計を行う。例えば、銀行口座で「残高」を「状態」のテーブルとして扱う場合には、「入金」や「出金」テーブルに更新があったらそれを元に「残高」テーブルの更新を行うといった具合である。

### UPDATE は使わない
UPDATE を使用すると、データの不整合が起きやすくなる。既存にあるレコードを削除し、新しく更新するデータを追加してデータの更新を実現する。また、UPDATEは新規追加の処理を行う場合にはINSERTを使用するという場合分けが必要になるが、INSERT と　DELETEを使用したやり方はそういった条件分岐が存在しないというメリットが存在する。

### 「コト」と「状態」をトランザクションで処理する必要はない
「状態」テーブルは二次的に再構築可能なデータなので、「コト」の記録と「状態」の更新を同じトランザクションで行う必要はない（何かしらのエラーを感知する仕組みが必要ではあるが）。状態の更新は非同期でメッセージングを使って行うことも一つの手である。

### 「状態」の更新は複数箇所で行なって良い
例えば、ある顧客がある商品Aを購入した場合に、顧客管理サーバのその顧客の残高のレコードの更新と、商品管理サーバの商品A売り上げのレコードの更新を同時に行っても良い。この場合には非同期メッセージングを使用すると良い。こうすることで、複数のシステムが疎結合な作りを保つことができる。

### ドメインオブジェクトとテーブル設計の関係

「コト」の記録を徹底し動的に状態を導出するテーブル設計とドメインオブジェクトの設計と言うのは基本的に一対一になります。  

しかし、ドメインオブジェクトとテーブル設計の指針と言うものは大きく異なります。それぞれの関心でいうとドメインオブジェクトは導出や加算、データを使用した判断ロジックが関心事になるのに対して、テーブルはデータの記録と言うものが関心事になるからです。  

またドメインオブジェクトの設計の指針については複数のドメインオブジェクトを組み合わせて上位の概念を表現するボトムアップ型のアプローチに対してテーブルは必要なデータを最初に用意しておいてそれらから動的に必要なデータを導出していくトップダウン型のアプローチとなります。  

したがって基本的にはドメインオブジェクトとテーブルが別々に分けて設計するべきです。  

ではその設計の異なるドメインオブジェクトとテーブルをどうやってマッピングすればいいのでしょうか？  

ドメインオブジェクトとテーブルのマッピングにはしばしばフレームワークが使用されますがフレームワークはドメインオブジェクトに様々な制約を課すことが多いです。例えば、独自の命名規則が存在したり、あるメソッドを実装しなければならなかったり、マッピングの情報をアノテーションに埋め込まなければならないといった制約が挙げられます。これらの制約がドメインオブジェクトに課されるとドメインオブジェクトに技術的な関心ごとが含まれることになります。これは、業務のロジックやデータに関心をおくドメインオブジェクトにおいて好ましくない事象です。したがって制約のないフレームワークの選定が必要となります。そこで一つの選択肢となるのが MyBatis です。MyBatis はオブジェクトとテーブルを独立して考える設計思想のもと開発されたツールであり、マッピングに関して制約を極限まで減らすことができます。  

## 画面表示のロジックと業務ロジックに関して

### 画面表示処理の難しさについて
アプリケーションは一つの画面に対してユーザの様々な関心(名前の入力、購入数の設定 etc...)が存在しているので、画面表示の処理は複雑になりやすいです。

### 表示ロジックと業務ロジックは混在させない
画面表示のロジックと業務ロジックが混在しているソースコードは、見通しが悪くなったり、同じような処理を様々な箇所で実装していたりして、変更容易性という観点で考えると好ましくないでしょう。

### どうすれば改修が容易な設計にできるか
上記の通り画面表示に関連する処理の実装には2つの問題点があります。

- ユーザの複数の関心から生まれる画面そのものの複雑性
- 画面表示ロジックと業務ロジックの混在

これらの問題点を解決するには以下のような方針をとります。

- 一つの汎用的な画面を設計するのではなく、一つのユーザの関心に対応した画面設計を行う
- 画面表示のロジックと業務ロジックを分離する

### 実践的な設計

ユーザの関心に基づいて小さな単位で分割し、それぞれをドメインオブジェクトで表現します。また、画面に関しても同じです。ショッピングのアプリでいうと、名前入力や配送先の選定といった単位で画面を設計します。これをタスクベースのユーザインターフェースと呼びます。タスクベースのユーザインターフェースの利点としては小分けにした画面を複数用意することで、柔軟性が上昇し、ユーザがやりたいことを単純に行うことができる点です。これが汎用的な画面になると、例えば配送先を変更しない場合にも配送先が表示され編集可能となっていてユーザにとっては複雑な画面となってしまいます。


# 問題集

## タスクベースのユーザインターフェースに関してドメインオブジェクト(DO)で表示処理を実装する方法とドメインオブジェクトとは別に画面表示用のオブジェクトを作成すする方法のどちらが良いか。そしてその理由は何か。

DOで実装した方がいい。なぜなら、DOとユーザの関心は対応していて、改修する場合の一つの指針となるから。あるユーザの関心に対するDOが存在しなかったら実装する必要があるし、逆もまた然りである。(p3173)

## ビューとモデルにおける処理の分離において気をつけるべきことを3点答えよ。

論理的な情報構造はDOで表現する。  
場合ごとの表示の出しわけはDOで実装する。  
HTMLのクラス属性はDOで出力させる。(p3184)  

## ビューにおいて物理的な情報構造と論理的な情報構造の違いは何か。

物理的な情報構造は、画面を実際に表示する技術方式に依存した情報構造のことを指す。例えば、何か文章を表示する場合に段落を表現する場合はHTMLだと<p>タグが使用されるが、「<p>タグを文章に付与する」というのは物理的な構造である。論理的な構造とは、表示処理とは関係なくその情報の特性を表現したものである。文章で例えた場合、この文章の構造は、段落を配列として持っていたり、文字の総数を持っていたりといったことが挙げられる。(p3208)

## アプリケーションで使用する項目において画面設計と業務ロジックを担当するドメインオブジェクトの間で統一しておくべき方針とは何か？

改修者が直感的に理解しやすくするために、項目の順序を統一する。(p3272)

## 画面設計におけるグルーピングの原則を4つ述べよ。

近接… 意味の似ている項目は近づけ、似ていない項目は離す。
整列… インデントなどで意味の異なりを表現する。
対比… 重要な箇所は強調する。DOの観点では重要な項目はクラスの上部で宣言し、重要ではないものは別のクラスにまとめて隠蔽する。パッケージ構成でも同じ。
反復… 反復で表現されている情報は同じクラスでまとめる。
(p3329)

# Web API開発において気をつけるべきこと

## HTTPメソッドのひとつであるPUTの問題点を挙げよ。またその解決策を述べよ。

PUTはクライアントに返すべきステータスコードや内容が複数選択できるため、クライアント側はその実装を知った上で実装を行わなければならないため密結合になりやすい点。解決策としては、データの更新処理もPOSTで行う。具体的には、GETメソッドで更新するデータが存在するかどうかを確認し、存在すればPOSTで更新処理を行う。GETはデータが存在する場合は200 OK, しなければ 404 No Content を返却すると定義されている。(p3533)

## クライアントとサーバ間のエラーレスポンス時の約束事を述べよ。

クライアントに返すエラーメッセージは、エラーにどう対応すればいいのかという手がかりに関する情報を含める。内部の詳細な内容は不要でありセキュリティの観点でも含めるべきではない。しかしサーバ側で出力するメッセージは詳細なエラーメッセージを出力しておく。(p3566)

## 開発性に富んだAPIの開発のために気をつけるべきことを2点述べよ。

更新と参照の処理を一括に行うのではなく、処理ごとに分離する。例えば予約を行うAPIでは、POSTリクエストで成功したら予約内容を返却するのではなく、予約番号を返却してそれを用いてGETリクエストで予約内容を取得するという方針をとる。
リソースの単位を細かく分割する。例えば、登録内容という大きなリソースの単位で更新を行うのではなく、出店者、販売期間といった細かい単位で更新を行うことによって、無駄な更新を省く(p3701)。

## API には複合的な機能を提供するAPIと基本機能を提供するAPIの2種類が存在するが、APIを利用する側と提供する側のどちらが実装するべきか？

複合的な機能を提供するAPIは利用する側、基本機能を提供するAPIは提供する側が実装するべきである。なぜなら複合的な機能までAPIで実装することになると、結合度を高めてしまうから。APIに実装する機能は基本的なものにとどめておき、それらを使用して特定の処理を実現したい場合は利用する側で実装する。(p3770)

## APIレスポンスをドメインオブジェクトにしてはいけないのはなぜか。それを踏まえてレスポンスはどのように実装すればよいか。

APIを使用する側の関心は返ってくるデータであり、そのデータの構造は業務ロジックに関心をおくドメインオブジェクトのそれと必ずしも一致しないから。対策としてはレスポンス用のオブジェクトを用意し、データを返す時はそのオブジェクトに変換してから返却する。(p3773)

## APIのレスポンスクラスはプレゼンテーション層とドメインモデルのどちらで実装すべきか

プレゼンテーション層で実装した方がよい。データの構造を変更するだけであって業務ロジックではないから。

## 加工や計算のロジックはAPIを提供する側で実装するべきか？

APIを提供する側は業務ロジックとまではいかない単純な計算は実装し、ある業務に依存している処理は書かない。

## 連携先が複数になった場合のAPIの複雑な連携において実装するとよい基本API, 拡張API, 個別対応API の役割を答えよ。

基本API... 文字通り基本的なAPI。
拡張API… 基本的なAPIを組み合わせて作成されるAPI
個別対応API… あるサービスの処理に特化したAPI
(p3861)

## 基本API、拡張API、個別対応APIのグルーピングを改善させる方針を述べよ。

連携先が増えたりAPIが成長するにつれてAPIを移動させる。例えば、今までは個別対応APIであった機能を利用者の増加に伴い拡張APIに移動させる等(p3867)。

## マイクロサービスの利点、欠点、実際に実現するにはどういった方針がよいのか述べよ。

一つのサービスを複数のサービスにわけ、それぞれの独立性を高めるのはオブジェクト指向とシナジーがある。しかし、オブジェクト指向の設計は改善を繰り返していくものなので、異なる技術で実装されている複数のサービスの修正にはコストがかかる点が課題として存在する。実務では一つのアプリケーションの設計を見直し、関心ごとに分けてマイクロサービスに落とし込んでいくのがよい(p3894)。

## データ構造の複雑性によってどのようなデータ形式を採用すればいいか述べよ。

複雑なデータ構造を表現する場合はXML、単純なデータ構造を表現するにはJSONを採用する(p3927)。

## 非同期メッセージングの利点を3点説明せよ。

メッセージング基盤を介して行われるので、連携先のアプリケーションの状態を気にせずリクエストを行うことができる点。
メッセージング基盤に共通の加工処理を組み込むことで、それを利用するアプリケーション側は自身の業務ロジックに専念できる点。
非同期メッセージングはメールのような機能であるから人間の実業務を模倣した設計になり、理解しやすい点(p3950)。

# オブジェクト指向の開発プロセス

## 従来のソフトウェア開発の流れを説明せよ。また、その問題点を説明せよ。

いわゆるV字モデルという手法で、要件定義、基本設計、詳細設計、実装、単体テスト、システムテスト、受入テストというフェーズに分かれていて、それぞれが独立している。問題点としては、この開発手法は段階的であるため、実装フェーズに入るまでに時間がかかってしまうこと。現代のソフトウェア開発で必要となる短期間での修正や拡張には向かないという点。また、この開発手法では設計フェーズと開発フェーズで担当者が異なることがある。そうなると、業務の関心がそれぞれで異なるためにロジックに不整合が生じてしまう点。(p4018)

## ドメインモデルを中心にしたシステム開発において、システム開発の当事者間で共有するドキュメント、開発しているシステムを利用する他の開発者に提供するドキュメント、上層部や外部の人間がそのプロジェクトを把握するのに必要なドキュメントは何か？

システム開発の当事者間で共有するドキュメント … ソースコード。
開発しているシステムを利用する他の開発者に提供するドキュメント… I/F設計書や画面設計図、利用者ガイド、カラム名など。
上層部や外部の人間がそのプロジェクトを把握するのに必要なドキュメント… 企画書、プレスリリース、利用者ガイドの導入など(p4141)

## 非機能要件や環境構築に関するドキュメントはどういったアウトプットで表現するのがよいか？

ソースコードで表現する(p4178)。

## 現場でオブジェクト指向のプログラミングを会得するためにできることを2つのべよ。

業務で扱っているソースコードをオブジェクト指向に基づいたリファクタリングを行うこと。
ThoughtWorks アンソロジー第5章で紹介されている9つのルールを用いてコーディングを行う。(p4319)

## 既存のコードを改善しながらオブジェクト指向設計を学ぶために最適な対象を3つ挙げよ。そしてその改善方法について述べよ。

1 .重複したコード  
重複したコードは一箇所にまとめる。

2 .長いメソッド  
大抵長いメソッドはネストが深いので、一番深くなっている箇所から別のメソッドに切り出していく。その過程で重複している箇所に気づいたら随時まとめていく。

3 .巨大なクラス  

巨大なクラスが存在する場合は以下のような箇所を改修対象とする。  
(a) インスタンス変数が多い  
クラス内でどのインスタンス変数をどのメソッドで使用しているのかを特定し、それらをグルーピングして別の新しいクラスで実装し、元のクラスはそのクラスを呼び出すように改修する。  

(b) 巨大なデータクラスを使用している  
巨大なデータクラスを使用している場合は使用しているメソッドをデータクラス内に移動させる。メソッドとデータは同じクラスにまとめる。  

(c) メソッドの引数が多い  
全ての引数を持つデータクラスを作成し、ロジックもそのクラスで実装する。
p4329

## リファクタリングを行ったのち、再利用しやすいような部品に昇華していくために気をつけるべきことを3点述べよ。

1 . クラスやメソッドの名前は適切かどうか  
リファクタリングを行い生成されたクラスはある一つの巨大なクラスのロジックやデータから切り出されたものである。したがって、そのクラス名やメソッド名は切り出し元のクラスに影響を受けている可能性がある。複数のクラスで切り出されたクラスやメソッドを使用する場合は命名が適切かどうかを再考する。

2 . 切り出されたクラスにロジックを追加することはできないか  
あるクラスから切り出されたクラスが持つデータは切り出し元のクラス以外のクラスで使用されていることがある。それを見つけたら積極的に切り出したクラスにロジックを追加していく。

3 . 複数のクラスをまとめるクラスを作成できないか  
リファクタリングを続けていくと、小さなクラスが複数作成されていく。そのクラスを組み合わせてロジックを実装していく時に、同じ組み合わせで実装している箇所を見つけることがある。その時は組み合わせるクラス群をまとめるクラスを作成する。こうすることで、ロジックを実装するクラスはそのクラスのみを知るだけで良くなる。

p4414

# 第１章

## プログラムの変更を楽にする実装を4点答えよ

変数やメソッドにわかりやすい名前を使用する。

処理ごとに段落に分ける。

目的別に変数を宣言する。破壊的代入を行わない。これを行う理由としては一つの変数を使いまわしていると、修正による影響範囲が広がるから。  

メソッドとして独立させる。  
複数の共通の処理を見つけたら、そのクラス間で参照関係が存在した  ら、呼び出し先のクラスにロジックを集約し、参照関係が存在しない  場合は、新しくクラスを作成し、そこにロジックを実装する。  
(p607)

## データを扱う場合に実装すべき方針を3点挙げよ。
業務ロジックに基づいて値の範囲を制限する  
  - 例えば数量を扱う場合、int で宣言すると、マイナスの値が混入する可能性がある。
データオブジェクトは Immutable にする
  - データを宣言する場合は再代入は行うべきではない。加工や計算を行う場合は新しいインスタンスとして定義する。
そのデータ独自のクラスを設計する
  - 例えば引数が int, int の関数の場合に、引数のそれぞれを逆に設定してしまい、予期せぬ挙動となるから。

## コレクションオブジェクトのデータを参照元で変更しないようにするにするための3つの工夫を答えよ。

参照元のコレクションオブジェクトのデータを使用して実現させる処理ロジックをコレクションオブジェクトに移譲する。
コレクションオブジェクトを新しく new して返す。
unmodifiableList (追加と削除ができない)を使用して、参照元には変更不可なコレクションオブジェクトを渡すようにする。また、各要素はSetterを実装していない値オブジェクトにし、変更を不可能にする。

# 第2章

## 条件分岐をコードに加える時にとるべき方針を2点挙げよ。

コードのかたまりをメソッドに切り分ける。
関連するデータやロジックを一つのクラスにまとめる。
(p905)

## 区分されたクラスごとに別々の処理を実装したい時にはどういったコーディングを行えばよいか？

区分されたクラスはすべて同じインターフェースを実装するようにする。(p994)

## 区分ごとのクラスのインスタンスを集合として扱う手法を1つ挙げよ。

Enumで区分ごとのクラス(同じインターフェースを実装している)をインスタンスとして保持しておき、場合によってvalueOf()メソッドを使用して特定のクラスを取得する。(p1090)

## 状態遷移のルール(~の状態は〇〇の状態に遷移できても、××の状態に遷移することはできない etc…)を定義する時に使用される手法を挙げよ。

MapとSetを使用してどの状態に遷移できるかを制御する。
```java
Map<State, Set<State>> allowed;
{
    allowed = new HashMap<>();
    allowed.put(A, Enumset.of(B, C));
    allowed.put((B, Enumset.of(C)));
    allowed.put(C, Enumset.of(A));
}
boolean canTransit(State from, State to) {
    Set<State> allowedStates = allowed.get(from);
    return  allowedStates.contains(to);
}
```
(p1116)

## 場合分けのロジックを整理する手法を2点挙げよ

if 文を使用する場合はコードのかたまりはメソッドに切り分けたり、早期リターンを使用してコードの独立性を向上させる。

そもそもif 文を使用せず、区分オブジェクトを使用することで分岐処理を実装する側が区分されたオブジェクトを気にしないようにする。
2章

# 第3章

## 業務ロジックを重複させないクラスの実装方法を4点述べよ。

業務ロジックをデータクラスに実装する  
必ずメソッドにはインスタンス変数を使用するようにする  
クラスを小さな単位で分ける  
積極的にパッケージを使用してグルーピングを行い、ソースコードの変更の影響をそのパッケージ内に留める。
(p1431)

# 第4章

## ドメインオブジェクトを実装する上でまずはじめに行うことを述べよ。

パッケージ図や業務フロー図などを用いて業務を理解する。

## ドメインオブジェクトの基本である値オブジェクト、コレクションオブジェクト、区分オブジェクト、列挙型の集合操作に加えてさらに4つのドメインモデルを開発するパターンを4つ挙げよ。

口座パターン... 入荷/出荷の履歴/見込みを記録して、契約が妥当かどうかを判断するパターン  
期日パターン... ある期日が存在したとして、現時点で期日を過ぎているのか、過ぎていないとしたら期日までどれくらいあるのか、期日が近づいていることに対しての通知などを実装するパターン
方針パターン... あるルールに基づいて判断するオブジェクトを複数開発しておき、それをListで保持して全て満たしているのか、一つだけ満たしているのかという判断を行うオブジェクトを開発するパターン

p1973

# 5章

## アプリケーション層の設計の方針を述べよ。

アプリケーション層に業務ロジックを実装するのではなく、ドメインオブジェクトを使用して処理を実現する。
5章

## アプリケーション層のサービスクラスの実装方針を規模の観点から述べよ。

まず基本機能を実装したサービスクラスを作成し、必要であればそれらを複合したサービスクラス(シナリオクラス)を作成する。例えば、残高照会を行う BankAccountService と任意の金額を引き出すBankAccountWithdrawServiceにわけるなど。
5章

## アプリケーション層でデータベース操作を行う際の注意点を述べよ。

コードを理解しやすいように、Repositoryインターフェースに業務ロジックに関心をおいたメソッドを実装させる。そして具体的なデータ操作はインターフェースを実装したクラスに実装させる。
5章
