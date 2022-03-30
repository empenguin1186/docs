JUnit5 でテストの初期化処理を統一する

## 背景
- JUnit5 

## 実装

例えばあるテストクラスに実装されているテストメソッドにおいて、各テストを実行する前に共通の処理を入れたい場合は以下のように書くと思います。
```java
@Slf4j
public class SampleTest {

    private static final String userName = "user01";
    
    @BeforeEach
    public void setUp(TestInfo info) {
        log.info("{} run...", info.getDisplayName());
    }

    @Test
    public void test1() {
        Assertions.assertEquals(2, 1 + 1);
    }
}
```
この場合 `@BeforeEach` が付与されているメソッドが各テストの実行前に実行されます。ですが当然この処理は `SampleTest` で実装されているテストメソッドにのみ実行されるので、他のクラスで定義されているメソッドが実行される時にはこの処理は実行されません。では全てのテストクラスで同じ処理をテスト実行前に実行させるためにはどのようにすればいいのでしょうか？
JUnit5には Extension という機能が存在し、テストの機能を拡張することができます。そしてこの機能については自作することができ、テストの初期化処理もこのExtensionを使用することで実現することができます。まずは Extension のクラスを実装することから始めます。例えば先程のようにテスト実行前にメソッド名を表示するような処理を行う Extension は以下のように実装できます。

```java
import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.extension.BeforeEachCallback;
import org.junit.jupiter.api.extension.ExtensionContext;

@Slf4j
public class SampleExtension implements BeforeEachCallback {
    @Override
    public void beforeEach(ExtensionContext context) throws Exception {
        log.info("{} run...", context.getDisplayName());
    }
}
```
`SampleExtension` は `BeforeEachCallback` の実装クラスであり、`beforeEach()`メソッドを実装する必要があります。これによりこの Extension を使用しているテストクラスはテスト実行前に初期化処理が実行されるようになります。なお今回はテスト実行前に初期化処理を行いたかったので `BeforeEachCallback` インターフェースを実装していますが、実行させたいタイミングによって実装するインターフェースは異なるので目的に応じたインターフェースを実装することが必要になります。他にどんなインターフェースがあるかは以下の資料に記載されています。
TODO

次にテストクラス側でこの Extension を使用するような設定を追加してあげる必要があります。実装としては以下になります。
```java
@ExtendWith(SampleExtension.class)
public class SampleTest {

    @Test
    public void test1() {
        Assertions.assertEquals(2, 1 + 1);
    }
}
```
`@ExtendWith()` に `SampleExtension` を指定してあげることで先程実装した初期化処理が実行されることになります。実行結果は以下となります。
TODO

また、テストごとに設定されている変数を使用した初期化処理を実装したい場合はどうすればいいのでしょうか？ 例えば `@BeforeEach` を使用した実装例は以下のようになります。

```java
@Slf4j
public class SampleTest {

    private static final String userName = "user01";

    @BeforeEach
    public void setUp(TestInfo info) {
        log.info("{} run.... user: {}", info.getDisplayName(), userName);
    }

    @Test
    public void test1() {
        Assertions.assertEquals(2, 1 + 1);
    }
}
```
先程のような Extension クラスの実装だとテストクラスごとに設定されている変数の情報は取得できないので一工夫する必要がありそうです。この場合は `@RegisterExtension` を使用すると実現できます。実装例は以下です。

```java
@Slf4j
public class UserNameExtension implements BeforeEachCallback {
    private final String userName;

    public UserNameExtension(String userName) {
        this.userName = userName;
    }

    @Override
    public void beforeEach(ExtensionContext context) throws Exception {
        log.info("{} run... user name: {}", context.getDisplayName(), this.userName);
    }
}
```

```java
@Slf4j
public class SampleTest {
    private static final String userName = "user01";

    @RegisterExtension
    private final UserNameExtension extension = new UserNameExtension(userName);

    @Test
    public void test1() {
        Assertions.assertEquals(2, 1 + 1);
    }
}
```
`@ExtendWith` ではなく `@RegisterExtension` を使用してあげることでテストごとに設定されている変数を使用して初期化処理を実装することができます。実行結果は以下です。

## まとめ
