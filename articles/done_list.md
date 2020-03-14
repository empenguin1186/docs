
<!-- TOC -->

- [この半期行ったことのまとめ](#この半期行ったことのまとめ)
  - [Golang](#golang)
    - [go.mod を使用するためには](#gomod-を使用するためには)
    - [seelog について](#seelog-について)
    - [ginについて](#ginについて)
    - [Go のテスト機構](#go-のテスト機構)
    - [データレース検出コマンド](#データレース検出コマンド)
    - [タグによるビルド対象制御](#タグによるビルド対象制御)
    - [ioutil.ReadFile について](#ioutilreadfile-について)
    - [便利なコマンド集](#便利なコマンド集)
    - [メインプロセスを停止させる処理](#メインプロセスを停止させる処理)
    - [参照型とポインタ型を明確に区別する](#参照型とポインタ型を明確に区別する)
    - [HTTPS 通信の方法](#https-通信の方法)
    - [sync.RWMutexについて](#syncrwmutexについて)
    - [xerrors と errors はどっちを使用する？](#xerrors-と-errors-はどっちを使用する)
    - [variadic parameter について](#variadic-parameter-について)
    - [可変長引数について](#可変長引数について)
    - [Client.Do() メソッドがタイムアウトしたかどうかを判定する処理](#clientdo-メソッドがタイムアウトしたかどうかを判定する処理)
    - [GoでCLIを作成する際のコマンドラインオプションの定義方法](#goでcliを作成する際のコマンドラインオプションの定義方法)
    - [time.Durationについて](#timedurationについて)
    - [json.Marshal() の仕様](#jsonmarshal-の仕様)
  - [Spring framework](#spring-framework)
    - [profile の切り替え方法](#profile-の切り替え方法)
    - [(Springとは関係ない) Jackson で Enum のフィールドの値をJSONに出力する](#springとは関係ない-jackson-で-enum-のフィールドの値をjsonに出力する)
    - [Spring によるトランザクション管理](#spring-によるトランザクション管理)
      - [PlatformTransactionManagerの役割について](#platformtransactionmanagerの役割について)
      - [@Transactional アノテーションは同一クラスでは機能しない](#transactional-アノテーションは同一クラスでは機能しない)
      - [トランザクション実装方法](#トランザクション実装方法)
      - [明示的トランザクションについて](#明示的トランザクションについて)
      - [@Transactional を使用する場合の制約](#transactional-を使用する場合の制約)
      - [Springのトランザクションの伝搬属性について](#springのトランザクションの伝搬属性について)
    - [テストでDIコンテナに登録されているBeanを置き換えたい時どうするか](#テストでdiコンテナに登録されているbeanを置き換えたい時どうするか)
    - [Runwith(SpringTest::class) は何のために付与する？](#runwithspringtestclass-は何のために付与する)
    - [Spring AOP について](#spring-aop-について)
      - [Join Point について](#join-point-について)
    - [DataIntegrityViolationException](#dataintegrityviolationexception)
    - [@ConditionalOnPropertyアノテーションについて](#conditionalonpropertyアノテーションについて)
    - [Spring のキャッシュ機能について](#spring-のキャッシュ機能について)
    - [BaseTypeHandler について](#basetypehandler-について)
    - [Kotlin + Spring AOP の仕様について](#kotlin--spring-aop-の仕様について)
    - [MyBatis について](#mybatis-について)
      - [MyBatis 定義した SQL で kotlin or Java のメソッドを使用する記法](#mybatis-定義した-sql-で-kotlin-or-java-のメソッドを使用する記法)
      - [mybatis の insert について](#mybatis-の-insert-について)
      - [mapper クラスで動的SQLを使用する時の注意事項](#mapper-クラスで動的sqlを使用する時の注意事項)
      - [Mybatis-config.xml の plugins ディレクティブについて](#mybatis-configxml-の-plugins-ディレクティブについて)
      - [ORACLE では null をセットしようとすると実行時例外が発生する](#oracle-では-null-をセットしようとすると実行時例外が発生する)
    - [AbstractRoutingDataSource について](#abstractroutingdatasource-について)
    - [@ExtendWith(SpringExtension.class)](#extendwithspringextensionclass)
    - [Spring におけるロガーのDIのさせ方](#spring-におけるロガーのdiのさせ方)
    - [maven BOM とは](#maven-bom-とは)
  - [データベース](#データベース)
    - [ORACLE の一意性制約について](#oracle-の一意性制約について)
    - [DDL について](#ddl-について)
    - [楽観ロックと悲観ロックの違い](#楽観ロックと悲観ロックの違い)
  - [fluentd](#fluentd)
  - [サーバ全般](#サーバ全般)
    - [systemd について](#systemd-について)
      - [systemd コマンド一覧](#systemd-コマンド一覧)
      - [systemd のファシリティについて](#systemd-のファシリティについて)
      - [systemd 各種コマンド](#systemd-各種コマンド)
    - [logrotate.d について](#logrotated-について)
    - [syslog](#syslog)
      - [syslog が使用しているポート](#syslog-が使用しているポート)
      - [syslog で ファイルに出力されたログの内容を syslog で扱えるようなストリームに変換するには？](#syslog-で-ファイルに出力されたログの内容を-syslog-で扱えるようなストリームに変換するには)
    - [rsyslog について](#rsyslog-について)
      - [rsyslog におけるモジュールについて](#rsyslog-におけるモジュールについて)
      - [rsyslog の ログローテートの設定](#rsyslog-の-ログローテートの設定)
      - [rsyslog による ログローテート の設定ファイルと設定確認方法](#rsyslog-による-ログローテート-の設定ファイルと設定確認方法)
      - [rsyslog.conf について](#rsyslogconf-について)
    - [rsyslog の設定を有効にするには](#rsyslog-の設定を有効にするには)
    - [/etc ディレクトリについて](#etc-ディレクトリについて)
    - [gmake について](#gmake-について)
    - [OpenSSL について](#openssl-について)
    - [zlib-devel について](#zlib-devel-について)
    - [pcre-devel について](#pcre-devel-について)
    - [登録されているサービス一覧を取得するコマンド](#登録されているサービス一覧を取得するコマンド)
    - [rpm コマンド](#rpm-コマンド)
      - [インストールしているパッケージを表示](#インストールしているパッケージを表示)
      - [特定のパッケージの詳細を確認する](#特定のパッケージの詳細を確認する)
      - [パッケージに含まれるファイルの一覧を表示](#パッケージに含まれるファイルの一覧を表示)
      - [参考文献](#参考文献)
    - [yum コマンド](#yum-コマンド)
      - [レポジトリの一覧を確認したい時](#レポジトリの一覧を確認したい時)
      - [yum update で指定したレポジトリを更新したい or したくない時](#yum-update-で指定したレポジトリを更新したい-or-したくない時)
    - [yum パッケージの公開鍵における検証](#yum-パッケージの公開鍵における検証)
    - [SIGSEGV について](#sigsegv-について)
    - [journald](#journald)
    - [UNIX ソケット](#unix-ソケット)
    - [kill -HUP <プロセス>](#kill--hup-プロセス)
  - [シェル](#シェル)
    - [シェルスクリプトで繰り返し処理を実装するときのテンプレート](#シェルスクリプトで繰り返し処理を実装するときのテンプレート)
    - [深さを1と限定して hoge. という名前がつくファイルを結果を標準出力に表示せずに削除したい場合](#深さを1と限定して-hoge-という名前がつくファイルを結果を標準出力に表示せずに削除したい場合)
    - [HTTPS通信を試す](#https通信を試す)
    - [シェルスクリプトでファイルの存在確認について](#シェルスクリプトでファイルの存在確認について)
    - [basename コマンド](#basename-コマンド)
    - [make について](#make-について)
      - [参考文献](#参考文献-1)
    - [curl でレスポンスタイムを計測したい場合](#curl-でレスポンスタイムを計測したい場合)
    - [どのプロセスがどのポートを使用しているかを確認する](#どのプロセスがどのポートを使用しているかを確認する)
    - [tcpdump で所定のポートにリクエストが飛んでいるかどうかを確認する。](#tcpdump-で所定のポートにリクエストが飛んでいるかどうかを確認する)
    - [grep で　ignore case で引っ掛ける](#grep-で　ignore-case-で引っ掛ける)
  - [git](#git)
    - [git add したファイルのHEADとの差分を確認するコマンド](#git-add-したファイルのheadとの差分を確認するコマンド)
    - [git merge したが元に戻したいときに使うコマンド](#git-merge-したが元に戻したいときに使うコマンド)
    - [別のブランチの commit を反映させる](#別のブランチの-commit-を反映させる)
    - [git でファイル検索を行うコマンド](#git-でファイル検索を行うコマンド)
    - [git のdevelopとmasterを除くマージ済みブランチを一括削除](#git-のdevelopとmasterを除くマージ済みブランチを一括削除)
    - [git grep した結果を特定の文字に変換する](#git-grep-した結果を特定の文字に変換する)
    - [git reflog](#git-reflog)
  - [Kotlin](#kotlin)
    - [Mockkについて](#mockkについて)
      - [Mockk の relaxed について](#mockk-の-relaxed-について)
      - [Static なクラスをモック化する](#static-なクラスをモック化する)
    - [Kotlin における internal について](#kotlin-における-internal-について)
    - [Kotlin の Triple について](#kotlin-の-triple-について)
    - [Kotlin の @Throws アノテーションについて](#kotlin-の-throws-アノテーションについて)
    - [KotlinのTODO関数について](#kotlinのtodo関数について)
    - [Kotlin の runCatching について](#kotlin-の-runcatching-について)
    - [companion object について](#companion-object-について)
  - [Java](#java)
    - [JMX について](#jmx-について)
  - [プログラミング全般](#プログラミング全般)
    - [テストカバレッジにおける C0 と C1 について](#テストカバレッジにおける-c0-と-c1-について)
    - [UUID かどうかを判定する正規表現](#uuid-かどうかを判定する正規表現)
    - [特定の文字以降の部分を抜き出す正規表現](#特定の文字以降の部分を抜き出す正規表現)
    - [循環的(サイクロマティック)複雑度](#循環的サイクロマティック複雑度)
    - [データレース(データ競合)について](#データレースデータ競合について)
    - [MIT ライセンスはどのようなライセンスか。](#mit-ライセンスはどのようなライセンスか)
  - [ネットワーク関連](#ネットワーク関連)
    - [ステータスコード 308 Permanent Redirect](#ステータスコード-308-permanent-redirect)
    - [MAC アドレスと IP アドレスの違い](#mac-アドレスと-ip-アドレスの違い)
    - [ネットワークスイッチ](#ネットワークスイッチ)
      - [コアスイッチ](#コアスイッチ)
    - [ロードバランサ再整理](#ロードバランサ再整理)
    - [デジタル署名について](#デジタル署名について)
    - [デジタル証明書について](#デジタル証明書について)
    - [TCP について](#tcp-について)
    - [CNAMEについて](#cnameについて)
    - [127.0.0.1 アドレスってなんなの？](#127001-アドレスってなんなの)
    - [ロードバランサーを経由したリクエストの送信元 IP アドレスはどこに格納される?](#ロードバランサーを経由したリクエストの送信元-ip-アドレスはどこに格納される)
  - [時事ネタ](#時事ネタ)
    - [ITP(Intelligent Tracking Prevention)](#itpintelligent-tracking-prevention)
    - [GDPR(General Data Protection Regulation)](#gdprgeneral-data-protection-regulation)
- [どんな構成にするか](#どんな構成にするか)
  - [Golang](#golang-1)
    - [Go の基礎知識](#go-の基礎知識)
    - [ツール](#ツール)
    - [Webフレームワークの紹介](#webフレームワークの紹介)
    - [テスト方法](#テスト方法)
    - [詰まった点](#詰まった点)
  - [Spring FrameWork](#spring-framework)
    - [基礎的な項目](#基礎的な項目)
    - [AOP](#aop)
    - [データベースアクセス](#データベースアクセス)
  - [データベース](#データベース-1)
    - [ORACLE の仕様について勉強になったこと](#oracle-の仕様について勉強になったこと)
  - [サーバ全般](#サーバ全般-1)
    - [systemd, rsyslog, jounald 周り](#systemd-rsyslog-jounald-周り)
      - [概要](#概要)
    - [rpm コマンド](#rpm-コマンド-1)
      - [基本コマンド一覧](#基本コマンド一覧)
      - [rpm パッケージの作成方法](#rpm-パッケージの作成方法)
    - [yum コマンド](#yum-コマンド-1)
      - [使用したコマンド](#使用したコマンド)
    - [運用で使用するコマンド](#運用で使用するコマンド)
    - [サーバ関連の基礎知識](#サーバ関連の基礎知識)
  - [シェル](#シェル-1)
    - [使用したコマンド](#使用したコマンド-1)
  - [git](#git-1)
    - [使用したコマンド](#使用したコマンド-2)
  - [Kotlin](#kotlin-1)
    - [Kotlin in Action のまとめ](#kotlin-in-action-のまとめ)
    - [Tips](#tips)
    - [Mockk の使用方法](#mockk-の使用方法)
  - [プログラミング全般](#プログラミング全般-1)
    - [正規表現](#正規表現)
    - [基礎知識](#基礎知識)
  - [ネットワーク関連](#ネットワーク関連-1)
    - [Tips](#tips-1)
  - [時事ネタ](#時事ネタ-1)
    - [Tips](#tips-2)

<!-- /TOC -->
# この半期行ったことのまとめ

## Golang

### go.mod を使用するためには
  - go.sum について

### seelog について
- seelog.xml の項目
- ロガーのテストについて

### ginについて
- *gin.Context.ShouldBindHeaderはグローバルな構造体にしか有効でない
- テスト用の Context を用意する方法
  - context.createTestContext
  - c.Request, _ := http.NewRequest("POST", "/", nil)

### Go のテスト機構
- 基本的な実行コマンド
  - カバレッジ作成方法
    - go tool cover -html=cover.out
    - パッケージごとのカバレッジファイルを作成するには？
- TestMain について
- reflect.DeepEqualについて
- fmt.Printlnのテスト方法
- テストのキャッシュを削除する方法
- サードパーティライブラリの testify について
  - mock の使用方法
- テストメソッドの命名規則

### データレース検出コマンド

### タグによるビルド対象制御

### ioutil.ReadFile について

### 便利なコマンド集
- peco
- ghq

### メインプロセスを停止させる処理
- os.Exit(1)

### 参照型とポインタ型を明確に区別する
### HTTPS 通信の方法
- https://louliz.com/ja/programming/go/create-https-server-with-golang

### sync.RWMutexについて
- http://tsujitaku50.hatenablog.com/entry/2017/08/20/211559

### xerrors と errors はどっちを使用する？

### variadic parameter について

### 可変長引数について
- https://blog.teapla.net/2015/09/5552/

### Client.Do() メソッドがタイムアウトしたかどうかを判定する処理
- https://blog.golang.org/error-handling-and-go

### GoでCLIを作成する際のコマンドラインオプションの定義方法
```go
var hoge string
flag.StringVar(&hoge, "default", "usage")
flag.Parse()
```

### time.Durationについて
- time.Duration の掛け算に int64 を使用してはならない。する場合は一旦Durationに変換する。

### json.Marshal() の仕様
- json.Marshal(hoge)を呼ぶときに内部では MarshalJSON() が呼ばれる。Marshalの方法を変更したいときはこのメソッドをオーバーライドする。
- (参考文献) https://qiita.com/taroshin/items/be00bea3371ade705a2d

## Spring framework

### profile の切り替え方法
  - -Dspring.profiles.active=XXX
  - https://reasonable-code.com/intellij-spring-profiles-active/
### (Springとは関係ない) Jackson で Enum のフィールドの値をJSONに出力する

### Spring によるトランザクション管理

#### PlatformTransactionManagerの役割について
- トランザクションを管理しているオブジェクト。デフォルトでDIされている。

#### @Transactional アノテーションは同一クラスでは機能しない
- https://www.qoosky.io/techs/400f6b6f09

#### トランザクション実装方法
- https://docs.spring.io/spring/docs/3.2.18.RELEASE/spring-framework-reference/html/transaction.html#transaction-declarative-annotations
- https://backpaper0.github.io/2018/02/22/spring_proxy.html

#### 明示的トランザクションについて
- TransactionTemplate を TransactionManager で初期化し、 execute メソッドを用いて実現する。execute メソッドはラムダを渡すが、このラムダの処理が一つのトランザクションの単位となる。
- mybatis-spring – MyBatis-Spring | トランザクション
  - http://mybatis.org/spring/ja/transactions.html

#### @Transactional を使用する場合の制約
- Spring の AOP の機能を使用する経緯でデフォルトでは同じクラスから public メソッドを呼び出す場合は機能しない(設定で変更できる)。
- AOP の機能を利用して実現されるため、実際に処理を行うクラスは Proxy クラスとなる。Proxy クラスとは、元のクラスの処理に加えてアドバイスの処理が実装された拡張クラスのことをさす。
- @Transactional は裏でJDK DynamicProxy というプロキシが使用されており、これはインターフェースを定義していないとエラーが発生する。
- 参考文献
  - https://qiita.com/eri-twin/items/9641f0cdc11122f57e59
  - https://terasolunaorg.github.io/guideline/1.0.3.RELEASE/ja/ImplementationAtEachLayer/DomainLayer.html
  - https://yo1000.gitbooks.io/self-study-spring/content/chapter7.html
  - https://docs.spring.io/spring/docs/current/spring-framework-reference/data-access.html#transaction

#### Springのトランザクションの伝搬属性について
- REQUIRED -> トランザクションだけでなく、データソースも単一のデータソースを使い回す(データソースの解決が逐一行われない)。したがって、別のDBの更新しようとすると、エラーが発生する
- 異なるデータソースを使うようにするには、propagation 属性の REQUIRES_NEW, NOT_SUPPORTED を使用する。こうすることで、データソースの解決処理が逐一走るようになるが、以下の副作用が存在する
  - REQUIRES_NEW : 新たにトランザクションを作成されてしまう。
  - NOT_SUPPORTED : そもそもこの設定がなされているメソッドではトランザクションが張られない。

### テストでDIコンテナに登録されているBeanを置き換えたい時どうするか
- @MockBean アノテーションを使用する
  - https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-testing  
- Spring BootでAutowiredされるクラスをMockitoでモックする - Qiita  
  - https://qiita.com/Plemling138/items/c48b3805cafa9dec7675

### Runwith(SpringTest::class) は何のために付与する？
- AutowiredなどのDIの機能をテストで使用したい場合に付与する。
  - https://qiita.com/Plemling138/items/c48b3805cafa9dec7675

### Spring AOP について
- Spring AOP Pointcutについて - Qiita  
  - https://qiita.com/mtsu724/items/6f5bb5c4a70af432df31  
- Spring AOP ポイントカット指定子の書き方について - Qiita  
  - https://qiita.com/rubytomato@github/items/de1019aeaaab51c8784d

#### Join Point について
- 定義方法
  ```java
  @Before("execution(* jp.co.penguin.*.*(..)))
  ```
  - これは全てのアクセス修飾子および戻り値の型、そして jp.co.penguin.(任意のクラス).(任意のメソッド名(引数の組み合わせは問わない))に該当するメソッドの前に実行するという意味である。
- 注意事項
  - AOPのジョインポイントで特定のパッケージ配下のメソッドに対して処理を挟み込む場合、ちゃんと存在するパッケージを指定してやらないと、コンパイル時にエラーが発生する。

### DataIntegrityViolationException
- データベースの更新や挿入を行う際に、一意性制約に違反した場合に投げられる例外

### @ConditionalOnPropertyアノテーションについて
- jar 実行時に実行されるRunnerを切り替えるために使用する。  例えば、以下のように設定されたクラスを実行したい場合は、`java -jar ~.jar --type hoge`とすれば実行される。
  ```java
  @ConditionalOnProperty(value = ["type"], havingValue = "hoge")
  class HogeRunner implements ApplicationRunner {
  }
  ```
- https://qiita.com/ksby/items/ef21b717e75b7c5b9988

### Spring のキャッシュ機能について
- Spring Bootでキャッシュの機能を有効にするには@EnableCachingアノテーションをアプリケーションクラスに付与する。@Cacheableアノテーションをつけることによりキャッシングを行うことが可能。オプションでキャッシュの名前とキーを定義する。@CacheEvictでキャッシュの削除を行う。
- https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/cache/CacheManager.html

### BaseTypeHandler について
- DBのデータ型と、Java(Kotlin)のデータ型のマッピングは、TypeHandlerによって行われている。以下が実装されているTypeHandlerの一覧である。
  - https://mybatis.org/mybatis-3/ja/configuration.html
- データのマッピングは自分でも定義することができる。この場合BaseTypeHandlerクラスを継承して各種メソッドを実装する必要がある。
- ResultSet.wasNull について
  - 上記の話にも関連してくることだが、DBから取得したレコードにnullが入っていた場合には、データ型がIntだと0に変換される。レコードの内容がnullだったかどうかを確認するには、ResultSet.wasNull()を使用する。
    - http://kuniku.hatenadiary.jp/entry/20130308/1362833246

### Kotlin + Spring AOP の仕様について
- Spring AOP を使うと、@Throws つけていない例外は UndeclaredThrowableException に型変換されてしまい、予期しない動作になってしまう

### MyBatis について
#### MyBatis 定義した SQL で kotlin or Java のメソッドを使用する記法
- 例) @jp.co.class@method
- MyBatisの条件分岐(<if>など)で文字列リテラルを利用する際の注意点 - Qiita
  - https://qiita.com/kazuki43zoo/items/dbdaad338f2f1ae5b256
- MyBatis – MyBatis 3 | 動的 SQL
  - https://mybatis.org/mybatis-3/ja/dynamic-sql.html

#### mybatis の insert について
- 引数が1つだけなら #{entity.property} の形にせず、#{property}で指定できる。
  - https://qiita.com/kazuki43zoo/items/ea79e206d7c2e990e478
- 引数が2個以上になると、#{entity.property}にしないと、変数の特定ができない。

#### mapper クラスで動的SQLを使用する時の注意事項
- <script>タグでSQLを括らないと、作用しない。
  - https://mybatis.org/mybatis-3/ja/dynamic-sql.html

#### Mybatis-config.xml の plugins ディレクティブについて
- mapper に処理をインターセプトさせることができる。
  - MyBatis – MyBatis 3 | 設定
    - https://mybatis.org/mybatis-3/ja/configuration.html#plugins

#### ORACLE では null をセットしようとすると実行時例外が発生する

- これは JdbcType が関係している。 JdbcType とは、Java オブジェクトとDBのデータのマッピングを行う際に指定することができるデータ型を定義しているクラスである。mybatis ではカラムにNULLをセットする場合には JdbcType.OTHER が指定されるが、ORACLEではJdbcType.OTHERをサポートしていないので、実行時に例外が発生する。これを解消するには、 application.yaml に以下の設定を行うと null をセットするときに JdbcType.NULL 型が指定されるようになる。

```yaml
mybatis:
  configuration:
    jdbcTypeForNull: 'NULL'
```
- しかし、yaml の設定だけでは対応できないカラムも存在する。その場合は Mapper インターフェースで jdbcType を適切な値に設定してあげる必要がある。
- 参考文献
  - https://docs.oracle.com/javase/jp/1.4/guide/jdbc/getstart/mapping.html
  - https://qiita.com/clomie/items/9949345ff3622715fd33
  - https://radiochemical.hatenablog.com/entry/2019/05/26/165306

### AbstractRoutingDataSource について
- AbstractRoutingDataSource の継承クラスを定義することで、データベースのシャーディングを行うことができる。シャーディングとは、複数のデータベースへリクエストを分散させ、全体のスループットを向上させる方法である。
- 参考文献
  - [シャーディング - Qiita](https://qiita.com/hharu/items/15627d2058bffe1fadf0)
  - [SpringのAbstractRoutingDataSourceを使ってシャーディングっぽいことをしてみる！ - Qiita](https://qiita.com/kazuki43zoo/items/9d8aec0ecab117a4d5c1)

### @ExtendWith(SpringExtension.class)
- JUnit5　をテストで使用したい場合はMainApplicationTestクラスに以下のアノテーションを付与する
```kt
@ExtendWith(SpringExtension.class)
```

### Spring におけるロガーのDIのさせ方
- https://saiya-moebius.hatenablog.com/entry/2017/11/08/033932

### maven BOM とは
- Project で使用するモジュールのバージョンを統一するために使用するもの。モジュールA は Z のバージョン 1.0 を使用しているが、モジュールB は Z のバージョン 2.0 を使用しているというような場合に BOM でバージョンを固定することでそのバージョンで固定することができる。
  - https://qiita.com/syogi_wap/items/432bbdbe9892eb05e122

## データベース
### ORACLE の一意性制約について
- 単一の列で構成される一意キーの場合は、複数のNULL（行） を持つことが可能。
- 複合一意キーにおいてすべてのキー列に対して NULL を持つ行も同様に複数持つことが可能。
- ただし、1つ以上のキー列に対して NULL を持ち、その他のキー列に対して同じ組合せの値を持つ 2つの行は制約違反となる。(※)
- 一意キーは 1 つの表に 複数定義してもよい。
```
単一列における一意キー制約において NULL が存在して NULL を INSERT してもは制約に違反しない。
複合列における一意キー制約において
(NULL,NULL) ⇔ (NULL,NULL) は制約に違反しないが　
(NULL,1000) ⇔ (NULL,1000) は一意キー制約違反となる。
(1000,NULL,NULL) ⇔ (1000,NULL,NULL) も同様に違反となる。 (※)
(※) 複合列における一意キー制約での、この振る舞いは Oracle 検索メカニズムによるものとマニュアルに書かれている。標準SQL では制約の 「1つのキー列でも NULL を持つ」場合には制約違反にはならない。という定義のようである。(標準SQL の資料が見つからなかったので未確認：制定年によって内容も変化する）
```
- RDBMS によって実装レベルに差があるため要注意である。SQL-Server では単一列でも制約違反となり、PostgreSQL では標準SQL に準拠しているらしい。
- 参考資料
  - https://www.shift-the-oracle.com/constraint/


### DDL について
- Data Definition Language の略。データ構造を定義するときに使用する言語。SQLにおけるCREATE文がこれに当たる。  
- 他にも、権限周りを定義するデータ制御言語(DCL)、データを操作言語(DML)というものが存在する。  
- (参考) https://wa3.i-3-i.info/word15639.html

### 楽観ロックと悲観ロックの違い
- 楽観ロックはデータ取得時のデータのバージョンと、データ更新時のデータのバージョンを突合し、バージョンが一致していた場合に更新処理を行う方法のことをさす。バージョンの確認に使用されるデータはデータベースに定義しておく必要があり、そのデータのことをロックキーと呼ぶ。それに対して悲観ロックとは、データ取得時にそのデータに対するロックを取得して、他のトランザクションから更新されないようにする方法のことをさす。Mybatisには楽観ロックを行う機能は提供されていない。
- 参考
  - https://qiita.com/NagaokaKenichi/items/73040df85b7bd4e9ecfc
  - https://terasolunaorg.github.io/guideline/public_review/ArchitectureInDetail/DataAccessCommon.html

## fluentd

## サーバ全般

### systemd について
#### systemd コマンド一覧
- https://dev.classmethod.jp/cloud/aws/service-control-use-systemd/

#### systemd のファシリティについて
- https://www.atmarkit.co.jp/ait/articles/0209/07/news002_2.html

#### systemd 各種コマンド

```
# 設定値反映
$ systemctl daemon-reload

# ステータス表示
$ systemctl status hoge.service

# service 一覧表示
$ systemctl list-units --type=service

# サービス定義ファイル編集
$ vim /etc/systemd/system/hoge.service
```

### logrotate.d について

### syslog
#### syslog が使用しているポート
- 514 番ポート。ちなみに UDP と TCP どちらも受け付けている。

#### syslog で ファイルに出力されたログの内容を syslog で扱えるようなストリームに変換するには？
`imfile`モジュールを使用する。
```conf
module(load="imfile") # (1)
 
input(type="imfile" 
      File="/var/log/sample/hoge.log" # (2)
      Tag="mua-gated" # (3)
      Facility="local0"# (4)
      Severity="info") # (5)
local0.info      @@<syslog サーバの IP アドレス>:514 # (6)
```

|  項目  |  説明  |
|:--:|:---|
|  (1)  |  ファイルのログを読み込むために必要なモジュール  |
|  (2)  |  取得先のファイルを定義する  |
|  (3)  |  タグを付与する  |
|  (4)  |  Facility を設定する  |
|  (5)  |  Serverity を設定する  |
|  (6)  |  出力先を定義する。「@@」はTCP で送信を意味する  |

Facility と Serverity の値は [ここ](https://knowledge.sakura.ad.jp/8969/) を参照。Facility に関しては　`local0~7`を使用するのが無難か。  
他の設定値は [ここ](http://www.l3jane.net/doc/rsyslog/configuration/modules/imfile.html?highlight=inputfileseverity) を参照。

### rsyslog について
#### rsyslog におけるモジュールについて
- https://knowledge.sakura.ad.jp/8969/
- モジュールには以下のようなものが存在する。
|  モジュール名  |  説明  |
|:--:|:---|
|  imuxsock  |  UNIX　ソケットを経由してログを受け取る。  |
|  imjournal  |  journald からのログを受け取る。  |

#### rsyslog の ログローテートの設定
- https://qiita.com/Esfahan/items/a8058f1eb593170855a1
- https://oxynotes.com/?p=6493

#### rsyslog による ログローテート の設定ファイルと設定確認方法
- `/etc/logrotate.conf` がログローテートの設定で、以下のコマンドを実行すると、設定が正しいかどうかを確認できる。
```sh
logrotate -dv /etc/logrotate.conf
```
-d は dryrun モードで実行することを指す。 -v はコマンドの詳細な内容が表示される。

#### rsyslog.conf について
- https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/7/html/system_administrators_guide/s1-basic_configuration_of_rsyslog
- https://qiita.com/pa_pa_paper/items/e2c886fa65d32a7442a1

### rsyslog の設定を有効にするには
- rsyslog は systemd が受け付けたログに対して動作するので、ちゃんとログが吐き出されるかを確認したい場合は、systemctl restart <サービス名> コマンドを打ち込まないとちゃんと吐き出されない。

### /etc ディレクトリについて
- システムの設定ファイルを格納するためのディレクトリ。

### gmake について

### OpenSSL について
- クライアントとサーバの間で行われる通信を暗号化するプロトコルである SSLおよびTLS のオープンソースのライブラリのことを指す。

### zlib-devel について
> zlibは、データの圧縮および伸張を行うためのフリーのライブラリである

- 【引用】[zlib - Wikipedia](https://ja.wikipedia.org/wiki/Zlib)

### pcre-devel について
- perl の正規表現を利用できるパッケージ？

> Perl Compatible Regular Expressions (PCRE) is a library written in C, which implements a regular expression engine, 

- 【引用】：[Perl互換の正規表現-ウィキペディア](https://en.wikipedia.org/wiki/Perl_Compatible_Regular_Expressions)

### 登録されているサービス一覧を取得するコマンド

```
$ systemctl list-unit-files --type=service
```

### rpm コマンド

#### インストールしているパッケージを表示

```
$ rpm -qa
```
grep と組み合わせるのが鉄板

#### 特定のパッケージの詳細を確認する

```
$ rpm -qi <パッケージ名>
```

#### パッケージに含まれるファイルの一覧を表示

```
$ rpm -ql <パッケージ名>
```

#### 参考文献
[おしえて、rpmコマンド! - Qiita](https://qiita.com/hijili/items/899522b9cd2fd1140ad2)

### yum コマンド

#### レポジトリの一覧を確認したい時
```
$ yum repolist [all|enabled|disabled]
```

#### yum update で指定したレポジトリを更新したい or したくない時
```
$ sudo yum --disablerepo "hoge" --enablerepo "fuga" update -x "piyo"
```
|  オプション  |  内容  |
|:--:|:--:|
|  --disablerepo  |  アップデートを行わないレポジトリを指定。*で一括指定も可能  |
|  --enablerepo  |  アップデートを行うレポジトリを指定。  |
|  -x  |  レポジトリに含まれる指定したパッケージのアップデートを行わない  |

### yum パッケージの公開鍵における検証
- [1.5.2. 署名パッケージの検証 Red Hat Enterprise Linux 6 | Red Hat Customer Portal](https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/6/html/security_guide/sect-security_guide-updating_packages-verifying_signed_packages)  

### SIGSEGV について
- ソフトウェアの実行時にアクセスが許可されていないメモリにアクセスしようとしてセグメンテーション違反が起こった場合にそのプロセスが受け取る信号のことである。不正なメモリにアクセスするケースとしては、システムで仕様しているメモリにアクセスした場合や、Read Only のメモリにアクセスした場合などが考えられる。

### journald
- サービスプロセスの 標準出力、エラー出力、sys ログメッセージなどを収集しているデーモン。sysログメッセージに関しては rsyslog にもログを転送している。

### UNIX ソケット
- プロセス間で通信を行う時に使用できるソケット(通信の接続口)。
- https://qiita.com/nk_yohn3301/items/7aec184e290940052ed2

### kill -HUP <プロセス>
- プロセスに設定の変更や再起動、停止などといった命令はシグナルと呼ばれている。このシグナルは、命令の種類によって決められており、設定の変更をプロセスに教えるシグナルは HUP と呼ばれる。使い方は以下。  
```
$ kill -HUP <proccess>
```
- シグナルは他にも色々な種類が存在する。  
  - https://www.itmedia.co.jp/help/tips/linux/l0689.html 

## シェル

### シェルスクリプトで繰り返し処理を実装するときのテンプレート

```sh
#!/usr/bin/bash
#echo "test"が10回実行される。
for i in `seq 10`
do
echo "test"
done
```

### 深さを1と限定して hoge. という名前がつくファイルを結果を標準出力に表示せずに削除したい場合
```
$ find ./ -maxdepth 1 -name 'hoge.＊' -exec rm -rf {} \; >/dev/null 2>&1
```

### HTTPS通信を試す
```
# サーバ側
$ openssl genrsa 2024 > server.key

$ openssl req -new -key server.key > server.csr

$ openssl x509 -req -days 3650 -signkey server.key < server.csr > server.crt

# クライアント側
$ curl --cacert /path/to/server.crt https:host/
```
- https://weblabo.oscasierra.net/openssl-gencert-1/

### シェルスクリプトでファイルの存在確認について

```sh
if [-d /etc/hoge]; then
    <command>
else
    <command>
fi
```
- 【参考文献】https://qiita.com/nii_yan/items/73f4caacdc2ca6f45135

### basename コマンド

ファイルの名前のみを切り出すコマンド。
```
$ basename /home/kodaira/nginx-1.16.1.tar.gz
nginx-1.16.1.tar.gz
```

### make について
- Makefileに定義されている処理を実行することができる。以下は全てのオブジェクトファイルを削除するルールを定義している。
```Makefile
clean:
    rm -f *.o
```

- 特定のファイルに対して実行する場合は以下のような書き方も可能。
```Makefile
print: *.c
    lpr -p $?
    touch print
```

```Makefile
clean:
    make -C src $@
```
- `-C <dir>` オプションは指定したディレクトリに移動してからルールを実行するという意味。 `$@` はターゲット名を表す変数。

#### 参考文献
- [GNU make 日本語訳(Coop編) - 目次](https://www.ecoop.net/coop/translated/GNUMake3.77/make_toc.jp.html)

### curl でレスポンスタイムを計測したい場合
- http://please-sleep.cou929.nu/curl-write-out-option.html

### どのプロセスがどのポートを使用しているかを確認する

```
$ sudo lsof -nPi
ntpd      958     ntp   16u  IPv4  20596      0t0  UDP *:123
ntpd      958     ntp   17u  IPv6  20597      0t0  UDP *:123
...
```
ちなみに `lsof` は `list open file` の略。プロセスなんかが開いているファイルをリストアップするコマンド。

### tcpdump で所定のポートにリクエストが飛んでいるかどうかを確認する。
```
$ sudo tcpdump src port <任意のポート番号>
```

### grep で　ignore case で引っ掛ける
```
$ grep -i ~
```

## git

### git add したファイルのHEADとの差分を確認するコマンド
```sh
git diff --cached ファイル名
```

### git merge したが元に戻したいときに使うコマンド
- git merge --abort
  - https://qiita.com/chihiro/items/5dd671aa6f1c332986a7

### 別のブランチの commit を反映させる
- git cherry-pick <commit の hash>
- git cherry-pick --abort

### git でファイル検索を行うコマンド
- git ls-files *hoge.kt

### git のdevelopとmasterを除くマージ済みブランチを一括削除
- git branch --merged | grep -v '\*|develop|master' | xargs git branch -d
  - [Gitでマージ済みブランチを一括削除 - Qiita](https://qiita.com/hajimeni/items/73d2155fc59e152630c4)

### git grep した結果を特定の文字に変換する
- https://qiita.com/snaka/items/568586259aa63bc74fec

### git reflog
- `git reset --soft ^HEAD` でコミットの取り消しをしようとした時に、間違って取り消しをする必要のないコミットも取り消してしまった。そういう時は、 `git reflog` で HEAD がどのように変化したかが表示されるので、それを元に任意の状態に戻すことができる。
```
$ git reflog
718654b (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
3dbc7bf HEAD@{1}: reset: moving to HEAD~
22bb5a3 (origin/master) HEAD@{2}: reset: moving to HEAD~
ae73b10 (origin/feature/vr-tutorial, feature/vr-tutorial) HEAD@{3}: checkout: moving from feature/vr-tutorial to master
ae73b10 (origin/feature/vr-tutorial, feature/vr-tutorial) HEAD@{4}: checkout: moving from master to feature/vr-tutorial
ae73b10 (origin/feature/vr-tutorial, feature/vr-tutorial) HEAD@{5}: commit: :+1: VR アプリ作成でつまづいた点を記録
22bb5a3 (origin/master) HEAD@{6}: pull origin mast
```
HEAD@{戻りたい番号} を指定して以下のコマンドを実行する。
```
$ git reset --hard HEAD@{3}
```

## Kotlin
### Mockkについて

#### Mockk の relaxed について
- https://mockk.io/

#### Static なクラスをモック化する
- java のクラスで static イニシャライザを使用しているクラスの static メソッドをモックする場合、以下のように定義した方が安全である。

```java
mockkStatic("jp.co.package.Class")
```

以下のように宣言すると、大抵テストは失敗する。
```java
mockkStatic(Class::class)
```

エラー文
```
every/verify {} block were run several times. Recorded calls count differ between runs
```

### Kotlin における internal について
- 修飾子。Javaにおけるパッケージプライベートがこれに近い。違いとしては、Javaのパッケージプライベートは他のプロジェクトで同じパッケージが存在すると、そこからでもアクセスできてしまうが、Kotlinのinternal はこれを防ぐ。  
  - https://blog.y-yuki.net/entry/2019/05/22/090000

### Kotlin の Triple について
- 一気に値を3つ返すことができる関数  
  - https://qiita.com/sdkei/items/2d5dab51b53975286945

### Kotlin の @Throws アノテーションについて
- Kotlin のコードをJavaで使用するときに、Kotlinは検査例外と非検査例外の区別がついていないために、Javaではcatchすることができない。catchするためには、Kotlin側で@Throwsアノテーションを付与する必要がある。したがって、呼び出し側で例外のハンドリングが必要な場合には@Throwsアノテーションを付与するのが望ましいと考える。    
  - https://qiita.com/noripi/items/0649415c84b08d1de3bb  

### KotlinのTODO関数について
- 未実装のメソッドについては、TODO関数を使うと、Exceptionを投げてくれる。  
  - http://kotlin-rev-solution.herokuapp.com/site/todo/

### Kotlin の runCatching について
- try catch が書きにくいときに使う記法
  - https://qiita.com/sdkei/items/030875adad33f76d1f09

### companion object について
- companion object で プリミティブ型やString型を使用する場合は、const 修飾子をつけたほうがオーバーヘッドは少ない

## Java

### JMX について
> Java Management Extensions (JMX) は、Java アプリケーションをモニタおよび管理するための仕様です。JMX を使用すると、汎用管理システムでアプリケーションをモニタし、注意が必要なときに通知を生成し、アプリケーションの状態を変更して問題を解決できます。

- アプリケーションの状態を記録している Bean である。自作することが可能。jolokia というツールを使用すると、HTTP で MBean の状態を取得することができる。

- 参考：[JMX について](http://otndnld.oracle.co.jp/document/products/wls/docs103/jmxinst/understanding.html)

## プログラミング全般
### テストカバレッジにおける C0 と C1 について
  - https://qiita.com/bremen/items/8b6542467d2a0066e5af

### UUID かどうかを判定する正規表現
```
[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}
```
- uuidのバリデーションチェックをする正規表現(bashシェルスクリプト編) | 瀬戸内の雲のように
  - https://www.setouchino.cloud/blogs/107

### 特定の文字以降の部分を抜き出す正規表現
```
(?<=【指定する文字】).*$
```
- https://boukenki.info/sakuraeditor-shiteimoji-ato-sakujo/

### 循環的(サイクロマティック)複雑度
- https://qiita.com/yut_arrows/items/16749e02313109071338

### データレース(データ競合)について
- あるデータの処理中に別の処理が割り込んでくること。これは回避すべき事象。
- https://qiita.com/yohhoy/items/00c6911aa045ef5729c6

### MIT ライセンスはどのようなライセンスか。
- このソフトウェアを誰でも無償で無制限に扱って良い。ただし、著作権表示および本許諾表示をソフトウェアのすべての複製または重要な部分に記載しなければならない。
- 作者または著作権者は、ソフトウェアに関してなんら責任を負わない。

## ネットワーク関連

### ステータスコード 308 Permanent Redirect
- 要求したリソースが Location ヘッダに記載されている URL に完全に移行したことを示す。したがって Locationヘッダに記載されている URL にリクエストを実行する。
  - 【参考文献】 https://developer.mozilla.org/ja/docs/Web/HTTP/Status/308

### MAC アドレスと IP アドレスの違い
- MAC アドレスは、個々のデバイスを一意に特定するためのアドレスを指す。IP アドレスはデータの送信先を特定するためのアドレスのことをさす。MAC アドレスは変更できないが、IPアドレスは変更することができる。
- https://qiita.com/cironaga/items/6a0c909e31f986c9f825

### ネットワークスイッチ
- 流れてきたデータの情報を元に送信元を特定して送信する機器。プロトコル階層によって種類が異なり、IP アドレスを元に送信先を特定するスイッチは L3 スイッチと呼ばれる。
- http://e-words.jp/w/%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF%E3%82%B9%E3%82%A4%E3%83%83%E3%83%81.html

#### コアスイッチ
- 主要拠点間をつなぐコアネットワークで用いられるスイッチ製品。

### ロードバランサ再整理
- https://www.slideshare.net/ryuichitakashima3/ss-72343772
- https://wa3.i-3-i.info/word11978.html
- https://wa3.i-3-i.info/word13634.html

### デジタル署名について
- メッセージの送信者はメッセージを送る相手に公開鍵を送る。  
- 続けてメッセージの送信者はメッセージとメッセージのハッシュ値を秘密鍵で暗号化したものを相手に送る。  
- 受信した相手側は暗号化されたハッシュ値を公開鍵で復号して、送られてきたメッセージのハッシュ値を計算し値が一致しているかを確認する。  
デジタル署名の利点としては送信元のみが秘密鍵を保持しているために送信元を一意に特定できること、加えて署名(ハッシュ値)を作成できるのは送信元のみであるから、受信元が署名を捏造するという可能性がなくなるという点である。

### デジタル証明書について
- デジタル署名では送信元を一意に特定できるという利点があったが、それでも送信元が想定していた相手だと証明できない欠点が存在する。その欠点を克服したのがデジタル証明書である。

- 流れとしては以下のようになる。
  - 送信元は認証局へメールアドレスやドメインを含んだ証明書を発行してもらう。この証明書は認証局自身の秘密鍵で暗号化されている。  
  - 送信元は受信元に証明書を送る。  
  - 証明書を受信した相手は認証局から公開鍵を取得し、証明書を復号する。正しく復号できればこれは認証局から発行された証明書であることが証明される。  
  - あとは復号されたデータから送信元本当に通信を行いたい相手なのかどうかを確認(メールアドレスやドメインから判断)し、そうであれば通信を行う。

### TCP について
>　TCP (Transmission Control Protocol) は、IPと同様にインターネットにおいて標準的に利用されている
　プロトコルです。TCPは、IPの上位プロトコルでトランスポート層で動作するプロトコル。ネットワーク
　層のIPとセッション層以上のプロトコル（例：HTTP、FTP、Telnet) の橋渡しをする形で動作しています。  

- 【参考文献】：[TCP/IP - TCPとは](https://www.infraexpert.com/study/tcpip7.html)
- サーバにHTTPでリクエストする場合は以下の手順を踏む。  
  - DNS サーバに問い合わせてホスト名からIPアドレスを取得する  
  - IPアドレスを取得したら、そのIPアドレスに向けてTCPで接続を確立する。 
  - 接続を確立したら、HTTPでリクエストを行う。

### CNAMEについて
[CNAMEレコードとは｜「分かりそう」で「分からない」でも「分かった」気になれるIT用語辞典](https://wa3.i-3-i.info/word12290.html)

### 127.0.0.1 アドレスってなんなの？
- ループバックアドレスと呼ばれ、自身を表すIPアドレス。

### ロードバランサーを経由したリクエストの送信元 IP アドレスはどこに格納される?
- IP アドレスを格納するヘッダには「REMOTE_ADDR」と「X-Forwarded-For」が存在する。ロードバランサーを介したリクエストは「REMOTE_ADDR」ヘッダにロードバランサーのIPアドレスが格納され、「X-Forwarded-For」ヘッダにはクライアントのIPアドレスが格納される。  
- 【参考文献】http://3.1415.jp/kc47fh1k/

## 時事ネタ
### ITP(Intelligent Tracking Prevention)
- ユーザの望まないような広告などを出さないために、Appleが開発したクッキーによるトラッキングを機械学習によって規制する技術

### GDPR(General Data Protection Regulation)

# どんな構成にするか

## Golang
### Go の基礎知識
### ツール
### Webフレームワークの紹介
### テスト方法
### 詰まった点

## Spring FrameWork
### 基礎的な項目
### AOP
### データベースアクセス

## データベース
### ORACLE の仕様について勉強になったこと

## サーバ全般
### systemd, rsyslog, jounald 周り
#### 概要

### rpm コマンド
#### 基本コマンド一覧
#### rpm パッケージの作成方法

### yum コマンド
#### 使用したコマンド

### 運用で使用するコマンド

### サーバ関連の基礎知識

## シェル
### 使用したコマンド

## git
### 使用したコマンド

## Kotlin
### Kotlin in Action のまとめ
### Tips
### Mockk の使用方法

## プログラミング全般
### 正規表現
### 基礎知識

## ネットワーク関連
### Tips

## 時事ネタ
### Tips