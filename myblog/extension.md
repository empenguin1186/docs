JUnit5 でテストの初期化処理を統一する

# 背景
JUnit5 で実装するテストにおいて、ユーザデータの初期化などが目的でテスト実行前に共通の処理を実行したい場合の実装方法を調査したので、記事として残しておきます。

# 実装

例えばあるテストクラスに実装されているテストメソッドにおいて、各テストを実行する前に共通の処理を入れたい場合は以下のように書くことが多いと思います。
```java
public class SampleTest {

    @BeforeEach
    public void setUp(TestInfo info) {
        System.out.println("beforeEach() executed before " + info.getDisplayName() + ".");
    }

    @Test
    @DisplayName("テスト1")
    public void test1(TestInfo info) {
        System.out.println(info.getDisplayName() + " executed.");
    }
}
```
この場合 `@BeforeEach` が付与されているメソッドが各テストの実行前に実行されます。ですが当然この処理は `SampleTest` で実装されているテストメソッドにのみ実行されるので、他のクラスで定義されているメソッドが実行される時にはこの処理は実行されません。では全てのテストクラスで同じ処理をテスト実行前に実行させるためにはどのようにすればいいのでしょうか？
JUnit5には Extension という機能が存在し、テストの機能を拡張することができます。そしてこの機能については自作することができ、テストの初期化処理もこの Extension を使用することで実現することができます。
まずは Extension のクラスを実装することから始めます。例えば先程のようにテスト実行前にメソッド名を表示するような処理を行う Extension は以下のように実装できます。

```java
public class SampleExtension implements BeforeEachCallback {

    @Override
    public void beforeEach(ExtensionContext context) throws Exception {
        System.out.println("beforeEach() executed before " + context.getDisplayName() + ".");
    }
}
```
`SampleExtension` は `BeforeEachCallback` の実装クラスであり、`beforeEach()`メソッドを実装する必要があります。これによりこの Extension を使用しているテストクラスはテスト実行前にここで指定した処理が実行されるようになります。なお今回はテスト実行前に初期化処理を行いたかったので `BeforeEachCallback` インターフェースを実装していますが、実行させたいタイミングによって実装するインターフェースは異なるので目的に応じたインターフェースを実装することが必要になります。他にどんなインターフェースがあるかは以下の資料に記載されています。
[JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/#extensions-lifecycle-callbacks)

次にテストクラス側でこの Extension を使用するような設定を追加してあげる必要があります。実装としては以下になります。
```java
@ExtendWith(SampleExtension.class)
public class SampleTest {

    @Test
    @DisplayName("テスト1")
    public void test1(TestInfo info) {
        System.out.println(info.getDisplayName() + " executed.");
    }
}
```
`@ExtendWith()` に `SampleExtension` を指定してあげることで先程実装した初期化処理が実行されることになります。実行結果は以下となります。

```sh
beforeEach() executed before テスト1.
テスト1 executed.
```

また、テストごとに設定されている変数を使用した初期化処理を実装したい場合はどうすればいいのでしょうか？ 例えば `@BeforeEach` を使用した実装例は以下のようになります。

```java
public class SampleTest01 {

    private final String userName = "user01";

    @BeforeEach
    public void setUp(TestInfo info) {
        System.out.println("beforeEach() executed before " + info.getDisplayName() + ". userName=" + this.userName);
    }

    @Test
    @DisplayName("テスト1")
    public void test1(TestInfo info) {
        System.out.println(info.getDisplayName() + " executed.");
    }

    @Test
    @DisplayName("テスト2")
    public void test2(TestInfo info) {
        System.out.println(info.getDisplayName() + " executed.");
    }
}
```
先程のような Extension クラスの実装だとテストクラスごとに設定されている変数(今回だと `userName`)の情報は取得できないので一工夫する必要がありそうです。この場合は `@RegisterExtension` を使用すると実現できます。まずは Extension クラスを実装します。

```java
public class UserNameExtension implements BeforeEachCallback {
    private final String userName;

    public UserNameExtension(String userName) {
        this.userName = userName;
    }

    @Override
    public void beforeEach(ExtensionContext context) throws Exception {
        System.out.println("beforeEach() executed before " + context.getDisplayName() + ". userName=" + this.userName);
    }
}
```

次にテストクラス側で上記の Extension クラスを使用するような設定を追加します。今回はテストクラスを2つ用意しました。
```java:SampleTest01.java
public class SampleTest01 {
    private final String userName = "user01";

    @RegisterExtension
    private final UserNameExtension extension = new UserNameExtension(userName);

    @Test
    @DisplayName("テスト1")
    public void test1(TestInfo info) {
        System.out.println(info.getDisplayName() + " executed.");
    }

    @Test
    @DisplayName("テスト2")
    public void test2(TestInfo info) {
        System.out.println(info.getDisplayName() + " executed.");
    }
}
```

```java:SampleTest02.java
public class SampleTest02 {
    private final String userName = "user02";

    @RegisterExtension
    private final UserNameExtension extension = new UserNameExtension(userName);

    @Test
    @DisplayName("テスト3")
    public void test3(TestInfo info) {
        System.out.println(info.getDisplayName() + " executed.");
    }

    @Test
    @DisplayName("テスト4")
    public void test4(TestInfo info) {
        System.out.println(info.getDisplayName() + " executed.");
    }
}
```

`@ExtendWith` ではなく `@RegisterExtension` を使用してあげることでテストごとに設定されている変数を使用して初期化処理を実装することができます。実行結果は以下です。

```sh
beforeEach() executed before テスト3. userName=user02
テスト3 executed.
beforeEach() executed before テスト4. userName=user02
テスト4 executed.
beforeEach() executed before テスト1. userName=user01
テスト1 executed.
beforeEach() executed before テスト2. userName=user01
テスト2 executed.
```

ちなみに以下のように `@BeforeEach` と `@RegisterExtension` の両方が使用されている場合に実行順序はどうなるのか検証してみました。
```java
public class SampleTest01 {
    private final String userName = "user01";

    @BeforeEach
    public void setUp(TestInfo info) {
        System.out.println("setUp() executed before " + info.getDisplayName() + ".");
    }

    @RegisterExtension
    private final UserNameExtension extension = new UserNameExtension(userName);

    @Test
    @DisplayName("テスト1")
    public void test1(TestInfo info) {
        System.out.println(info.getDisplayName() + " executed.");
    }

    @Test
    @DisplayName("テスト2")
    public void test2(TestInfo info) {
        System.out.println(info.getDisplayName() + " executed.");
    }
}
```
実行結果は以下のようになり、Extension クラスの処理が先に実行されるようでした。
```sh
beforeEach() executed before テスト1. userName=user01
setUp() executed before テスト1.
テスト1 executed.
beforeEach() executed before テスト2. userName=user01
setUp() executed before テスト2.
テスト2 executed.
```

# 参考文献
- [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)