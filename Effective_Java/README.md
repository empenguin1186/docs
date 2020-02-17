<!-- TOC -->

- [Effective Java まとめ](#effective-java-まとめ)
- [項目1 コンストラクタの代わりに static ファクトリメソッドを検討する](#項目1-コンストラクタの代わりに-static-ファクトリメソッドを検討する)
  - [問題](#問題)
- [項目2 多くのコンストラクタパラメータに直面したときにはビルダーを検討する](#項目2-多くのコンストラクタパラメータに直面したときにはビルダーを検討する)
- [項目3 privateのコンストラクタかenum型でシングルトン特性を強制する](#項目3-privateのコンストラクタかenum型でシングルトン特性を強制する)
- [項目4 private のコンストラクタでインスタンス化不可能を矯正する](#項目4-private-のコンストラクタでインスタンス化不可能を矯正する)
  - [問題](#問題-1)
- [項目5 資源を直接結びつけるよりも依存性注入を選ぶ](#項目5-資源を直接結びつけるよりも依存性注入を選ぶ)
  - [問題](#問題-2)
- [項目6 不必要なオブジェクトの生成を避ける](#項目6-不必要なオブジェクトの生成を避ける)
- [項目7 使われなくなったオブジェクト参照を取り除く](#項目7-使われなくなったオブジェクト参照を取り除く)
- [項目9 try-finally よりも try-with-resources を選ぶ](#項目9-try-finally-よりも-try-with-resources-を選ぶ)
  - [問題](#問題-3)
- [項目12 toString を常にオーバーライドする](#項目12-tostring-を常にオーバーライドする)
  - [問題](#問題-4)
- [項目17 可変性を最小限にする](#項目17-可変性を最小限にする)
  - [問題](#問題-5)
- [項目18 継承よりもコンポジションを選ぶ](#項目18-継承よりもコンポジションを選ぶ)
- [項目20 抽象クラスよりもインターフェースを選ぶ](#項目20-抽象クラスよりもインターフェースを選ぶ)
  - [問題](#問題-6)
- [項目21 将来のためにインターフェースを設計する](#項目21-将来のためにインターフェースを設計する)
- [項目22 型を定義するためだけにインターフェースを使う](#項目22-型を定義するためだけにインターフェースを使う)
  - [問題1](#問題1)
  - [問題2](#問題2)
- [項目23 タグ付きクラスよりもクラス階層を選ぶ](#項目23-タグ付きクラスよりもクラス階層を選ぶ)
  - [問題](#問題-7)
- [項目24 非 static のメンバークラスよりも static のメンバークラスを選ぶ](#項目24-非-static-のメンバークラスよりも-static-のメンバークラスを選ぶ)
  - [問題](#問題-8)
- [項目25 ソースファイルを単一のトップレベルのクラスに限定する](#項目25-ソースファイルを単一のトップレベルのクラスに限定する)
- [項目27 無検査警告を取り除く](#項目27-無検査警告を取り除く)
- [項目28 配列よりもリストを選ぶ](#項目28-配列よりもリストを選ぶ)
- [項目35 序数の代わりにインスタンスフィールドを使う](#項目35-序数の代わりにインスタンスフィールドを使う)
  - [問題](#問題-9)
- [項目36 ビットフィールドの代わりに EnumSet を使う](#項目36-ビットフィールドの代わりに-enumset-を使う)
  - [問題1](#問題1-1)
  - [問題2](#問題2-1)
- [項目40 常に Override アノテーションを使う](#項目40-常に-override-アノテーションを使う)
  - [問題](#問題-10)
- [項目41 型を定義するためにマーカーインターフェースを使う](#項目41-型を定義するためにマーカーインターフェースを使う)
  - [問題](#問題-11)
- [項目42 無名クラスよりもラムダを選ぶ](#項目42-無名クラスよりもラムダを選ぶ)
  - [問題1](#問題1-2)
  - [問題2](#問題2-2)
  - [問題3](#問題3)
- [項目43 ラムダよりもメソッド参照を選ぶ](#項目43-ラムダよりもメソッド参照を選ぶ)
  - [問題](#問題-12)
- [項目44 関数型インターフェースを使う](#項目44-関数型インターフェースを使う)
  - [問題](#問題-13)
- [項目51 メソッドのシグニチャを注意深く設計する](#項目51-メソッドのシグニチャを注意深く設計する)
  - [問題](#問題-14)
- [項目54 null ではなく、空コレクションか空配列を返す。](#項目54-null-ではなく空コレクションか空配列を返す)
  - [問題](#問題-15)
- [項目58 従来のforループよりもfor-each　ループを選ぶ](#項目58-従来のforループよりもfor-each　ループを選ぶ)
  - [問題](#問題-16)
- [項目57 ローカル変数のスコープを最小限にする](#項目57-ローカル変数のスコープを最小限にする)
  - [問題](#問題-17)
- [項目59 ライブラリを知り、ライブラリを使う](#項目59-ライブラリを知りライブラリを使う)
  - [問題](#問題-18)
- [項目60 正確な答えが必要ならば、float と double を避ける](#項目60-正確な答えが必要ならばfloat-と-double-を避ける)
  - [問題](#問題-19)
- [項目61 ボクシングされた基本データよりも基本データ型を選ぶ](#項目61-ボクシングされた基本データよりも基本データ型を選ぶ)
  - [問題](#問題-20)
- [項目62 他の型が適切な場所では、文字列を避ける](#項目62-他の型が適切な場所では文字列を避ける)
- [項目63 文字列結合のパフォーマンスに用心する](#項目63-文字列結合のパフォーマンスに用心する)
  - [問題](#問題-21)
- [項目64 インターフェースでオブジェクトを参照する](#項目64-インターフェースでオブジェクトを参照する)
  - [問題](#問題-22)
- [項目65 リフレクションよりもインターフェースを選ぶ](#項目65-リフレクションよりもインターフェースを選ぶ)
  - [問題](#問題-23)
- [項目66 ネイティブメソッドを注意して使う](#項目66-ネイティブメソッドを注意して使う)
  - [問題](#問題-24)
- [項目71 チェックされる例外を不必要に使うのを避ける](#項目71-チェックされる例外を不必要に使うのを避ける)
  - [問題1](#問題1-3)
  - [問題2](#問題2-3)
- [項目72 標準的な例外を使う](#項目72-標準的な例外を使う)
- [項目74 各メソッドがスローするすべての例外を文書化する](#項目74-各メソッドがスローするすべての例外を文書化する)
  - [問題](#問題-25)
- [項目75 詳細メッセージにエラー記録情報を含める](#項目75-詳細メッセージにエラー記録情報を含める)
  - [問題](#問題-26)
- [項目76 エラーアトミック性に努める](#項目76-エラーアトミック性に努める)
  - [問題1](#問題1-4)
  - [問題2](#問題2-4)
- [項目77 例外を無視しない](#項目77-例外を無視しない)
- [項目78 共有された可変データへのアクセスを同期する](#項目78-共有された可変データへのアクセスを同期する)
  - [問題1](#問題1-5)
  - [問題2](#問題2-5)
- [項目83 遅延初期化を注意して使う](#項目83-遅延初期化を注意して使う)
  - [問題1](#問題1-6)
  - [問題2](#問題2-6)
  - [問題3](#問題3-1)
- [項目84 スレッドスケジューラに依存しない](#項目84-スレッドスケジューラに依存しない)
  - [問題](#問題-27)
- [項目85 Java のシリアライズよりも代替手段を選ぶ](#項目85-java-のシリアライズよりも代替手段を選ぶ)
  - [問題](#問題-28)

<!-- /TOC -->
# Effective Java まとめ

# 項目1 コンストラクタの代わりに static ファクトリメソッドを検討する
インスタンスを生成する方法には static ファクトリメソッドを使用する方法とコンストラクタを使用する方法の 2種類存在する。 static ファクトリメソッドを使用するメリットとしては以下の通り。  

1. コンストラクタと違い、メソッド名を持つので、重複を避けることができる。  
2.　呼び出しごとに新しいインスタンスを生成する必要がない。  
3. メソッドの戻り値の型を任意のサブタイプにすることが可能である。  
4. 戻り値の型は入力パラメータの値に応じて呼び出しごとに変えられる。  
5. 戻り値の型はファクトリメソッドの実装時点で存在する必要はなく、後から実装可能。  

しかし static ファクトリメソッドにも欠点が存在する。以下が欠点である。  
1. piublic あるいは protected のコンストラクタが実装されていないクラスのサブクラスを作成することができない。  
2. プログラマが static ファクトリメソッドを見つけるのが困難である。

## 問題
以下の文章は static ファクトリメソッドについて述べた文章である。括弧に当てはまる文章を答えよ。  

インスタンスを生成する方法には static ファクトリメソッドを使用する方法とコンストラクタを使用する方法の 2種類存在する。 static ファクトリメソッドを使用するメリットとしては以下の通り。  

1. コンストラクタと違い、メソッド名を持つので、(1)を避けることができる。  
2.　呼び出しごとに(2)を生成する必要がない。  
3. メソッドの戻り値の型を任意の(3)にすることが可能である。  
4. 戻り値の型は(4)の値に応じて呼び出しごとに変えられる。  
5. 戻り値の型はファクトリメソッドの(5)で存在する必要はなく、後から実装可能。  

しかし static ファクトリメソッドにも欠点が存在する。以下が欠点である。  
1. (6) あるいは (7) のコンストラクタが実装されていないクラスのサブクラスを作成することができない。  
2. プログラマが static ファクトリメソッドを見つけるのが(8)である。

<details>
<summary>解答</summary>

1. 重複  
2. インスタンス  
3. サブクラス  
4. パラメータ  
5. 実装時点  
6. public  
7. protected  
8. 困難

</details>

# 項目2 多くのコンストラクタパラメータに直面したときにはビルダーを検討する

フィールド数が多いクラスでファクトリメソッドやコンストラクタを使用すると、引数の記述が多くなり、さらに同じ型の引数が連続するような場合にはヒューマンエラーが起こり、正常なオブジェクトが生成されない場合がある。それを避けるため、フィールド数が多いオブジェクトはビルダーを使用すると良い。また、ビルダーを使用して子クラスのインスタンスを生成する場合は以下のようなコードを実装する必要がある。  

```java
abstract class Pizza {
    public enum Topping {HAM, MASHROOM, ONION, PEPPER, SAUSAGE}
    final Set<Topping> toppings;

    abstract static class Builder <T extends Builder<T>> {
        EnumSet<Topping> toppings = EnumSet.noneOf(Topping.class);
        public T addTopping(Topping topping) {
            toppings.add(Objects.requireNonNull(topping));
            return self();
        }

        abstract Pizza build();

        protected abstract T self();
    }

    Pizza(Builder<?> builder) {
        this.toppings = builder.toppings.clone();
    }
}

class NyPizza extends Pizza {
    public enum Size {SMALL, MEDIUM, LARGE}
    private final Size size;

    public static class Builder extends Pizza.Builder<Builder> {
        private final Size size;

        public Builder(Size size) {
            this.size = Objects.requireNonNull(size);
        }

        @Override
        public NyPizza build() {
            return new NyPizza(this);
        }

        @Override
        protected Builder self() {
            return this;
        }
    }

    private NyPizza(Builder builder) {
        super(builder);
        this.size = builder.size;
    }
}
```
親クラスのビルダーでジェネリクスを使用して、呼ばれた時点での型を返すようにすると、子クラスと親クラスでビルダーメソッドがまたがっても実装することができる。

# 項目3 privateのコンストラクタかenum型でシングルトン特性を強制する

シングルトンパターンにおいて有効な実装パターンを2点紹介する。  
まず１つ目はファクトリメソッドを使用するパターンである。
```java
public class Elvis {
    private static final Elvis INSTANCE = new Elvis();
    private Elvis() {...}
    public static Elvis getInstance() { return INSTANCE; }
    public void ...
}
```
この実装のメリットは以下の3点。  
1. シングルトンのスコープを変更できる。例えば、このパターンで実装すると、スレッド単位で唯一のインスタンスを保持するような実装に変更したい場合も対応することができる。  
2. ジェネリックのシングルトンファクトリを定義することが可能。  
3. メソッド参照が可能。  

2つ目はEnumを定義することである。
```java
public enum Elvis {
    INSTANCE;
    public void ...
}
```
この実装のメリットは以下の2点。  
1. １つ目のパターンと異なり、シリアライズの機能が備わっている。  
2. シリアライズやリフレクションによる複数のインスタンスの生成を防ぐことができる。

# 項目4 private のコンストラクタでインスタンス化不可能を矯正する
時折、static なメソッドのみからなるクラスを実装したい時がある。その場合、インスタンスが生成されるのを防ぐ必要があるが、public なコンストラクタを定義した時点でインスタンス生成を許してしまうし、かといって何もコンストラクタを定義しないとデフォルトコンストラクタが定義されてしまい、結局コンストラクタが生成される羽目になる。これを避けるため、private なコンストラクタを定義する。これをユーティリティクラスと呼ぶ。

```java
public UtilityClass {
    private UtilityClass() {
        throw new AssertionError();
    }
    // 以下 static メソッドを定義
    ...
}
```
ユーティリティクラスは副作用として継承することもできないため、サブクラスがインスタンスを生成してしまうという事態も回避することができる。  

## 問題
ユーティリティクラスとは何か。また、その実装方法を述べよ。

<details>
<summary>解答</summary>

static なメソッドのみで構成される、インスタンス化を不可能にしたクラスのこと。private のコンストラクタを定義して実装する。

</details>

# 項目5 資源を直接結びつけるよりも依存性注入を選ぶ

以下のようなスペルチェッカーを実装したユーティリティクラスがあったとする。

```java
public class SpellChecker {
    private static final Lexicon dictionary = ...

    private SpellChecker() {}

    public static boolean isValid(String word) {
        ...
    }

    public static List<String> suggestions(String typo) {
        ...
    }
}
```
この実装はフィールドである fictionary が固定されているという点で実装としては柔軟ではない。もしあらゆる辞書を使用してテストを実行したくなった場合、このクラスは扱いにくいものとなる。したがって dictionary を外部から注入する依存性注入という方式を用いて柔軟な設計とする。

```java
public class SpellChecker {
    private static final Lexicon dictionary = ...

    private SpellChecker(Lexicon dictionary) {
        this.dictionary = Objects.requireNonNull(dictionary);
    }

    public static boolean isValid(String word) {
        ...
    }

    public static List<String> suggestions(String typo) {
        ...
    }
}
```
また、このように手動で依存性を注入しなくとも、Spring といった依存性注入のフレームワークを使用して複雑なインスタンスの生成も容易に行うことが可能となる。

## 問題
以下のようなスペルチェッカーの実装があったとする。

```java
public class SpellChecker {
    private static final Lexicon dictionary = ...

    private SpellChecker() {}

    public static boolean isValid(String word) {
        ...
    }

    public static List<String> suggestions(String typo) {
        ...
    }
}
```

以下の文章はこの実装の欠点について述べたものである。括弧に当てはまる単語を答えよ。

この実装はフィールドである fictionary が(1)されているという点で実装としては(2)ではない。もしあらゆる辞書を使用して(3)を実行したくなった場合、このクラスは扱いにくいものとなる。したがって dictionary を外部から注入する(4)という方式を用いて柔軟な設計にした方が良い。

<details>
<summary>解答</summary>

1. 固定  
2. 柔軟  
3. テスト  
4. 依存性注入

</details>

# 項目6 不必要なオブジェクトの生成を避ける

不必要なオブジェクトの生成を避ける。例えば、以下のようなケースである。

```java
String s = new String("hoge")
```
このような記述をすると、String インスタンスが毎回生成されてしまう。String 型に関しては以下のように記述して同じ文字列の場合はインスタンスの生成を避けるべきである。

```java
String s = "hoge"
```
他にも、正規表現のコンパイル処理もインスタンスごとに行わずに、クラスの初期化時にキャッシュとして保持しておいたほうが、パフォーマンスは向上する。

```java
public class RomanMatcher {
    private static final Pattern ROMAN = Pattern.compile("...")
}
```
他にも、うっかりfor文でLongのようなボクシングされた基本データ型を使用すると、パフォーマンスが低下する。
```java
Long sum = 0L;
for(long i=0, i <= Integer.MAX_VALUE; i++) {
    sum += i;
}
```
この場合sumが加算されるたびに新しいLongインスタンスが生成されるので、大量のメモリを消費することとなる。したがって、sum は基本データ型であるlongを使用する。  
また、不必要なインスタンスを生成しないためには、各所で同じインスタンスを使いまわすことが必要となるが、インスタンスの共有が好ましくないときは、無理に共有しない。

# 項目7 使われなくなったオブジェクト参照を取り除く

プログラマが意識してオブジェクトの参照を考慮しなければならないときは、メモリリークが発生しないかどうかを検討する必要がある。使われなくなったオブジェクトへの参照の代わりにnullを参照するように仕向ければ、ガベージコレクションの対象となり、メモリを解放することができる。  
メモリリーク発生の原因でよくあるものは、キャッシュとコールバックである。キャッシュによるオブジェクト参照はそのキャッシュが使われなくなったとしても残りがちだが、この問題に関してはキャッシュに弱い参照であるWeakHashMapを使用すると、キャッシュの外部のオブジェクトの全てがそのキャッシュのキーの参照を持たなくなった時点で自動的にメモリから取り除かれる。コールバックに関しても、実行されずに登録され続けているコールバックが増えてメモリリークとなるケースが考えられるが、これに関してもコールバックをWeakHashMapのキーとして登録しておけば、ガベージコレクションの対象となる。

# 項目9 try-finally よりも try-with-resources を選ぶ

クローズしなければならない資源を扱う場合は、try-finally よりも try-with-resources 文を使用する。  
なぜなら、try-finally の finally 句でクローズ処理を記述しても、そのクローズ処理も例外を投げる可能性がある。  
そして、その投げられた例外は、スタックトレースの特性上、try 句で投げられた例外を隠蔽してしまうため、原因の特定を行うのが難しくなる。  
したがってそれを避けるために try-with-resources 文を使用する。try-with-resources 文を使用できるクラスは、 AutoCloseable インターフェースを実装しているものに限定され、AutoCloseable インターフェースは唯一 close メソッドをもつ。例外が発生した場合にはこの close メソッドが呼ばれるイメージである。  
現在多くのクラスがこの AutoCloseable インターフェースを実装している。  
また、try-with-resources 文で例外が発生すると、try　句の例外が優先的に記録される。後から発生した例外については　Java 7 で新たに　Throwable に追加された　GetSuppressed メソッドで確認することができる。

悪い例
```java
static String firtLineOfFile(String path) throws IOException {
    BufferedReader br = new BufferedReader(new FileReader(path));
    try {
        return br.readLine();
    } finally {
        br.close();
    }
}
```

いい例
```java
static String firtLineOfFileWithResource(String path) throws IOException {
    try(BufferedReader br = new BufferedReader(new FileReader(path))) {
        return br.readLine();
    }
}
```

## 問題
以下のコードの問題点をあげよ。そしてどのように改善すべきか述べよ。

```java
static String firtLineOfFile(String path) throws IOException {
    BufferedReader br = new BufferedReader(new FileReader(path));
    try {
        return br.readLine();
    } finally {
        br.close();
    }
}
```

<details>
<summary>解答</summary>
try-finally 文を使用している点。finally句でも例外を投げる可能性があり、それがtry　句の例外を隠蔽する可能性があるので、使用は避けたほうが良い。try-with-resources　を使用することでこの問題を回避することができる。  
Effective Java 項目9 より

```java
static String firtLineOfFileWithResource(String path) throws IOException {
    try(BufferedReader br = new BufferedReader(new FileReader(path))) {
        return br.readLine();
    }
}
```
</details>

# 項目12 toString を常にオーバーライドする

`toString` メソッドはプログラマが意図しないところで使用されたりするので、そのクラスにあった仕様でオーバライドしておくと、デバッグの時に役立つので、オーバーライドしておいた方がよい。

例えば、電話番号を表す`PhoneNumber`クラスがあったとして、そのクラスを標準出力で表示してみる。
```java
System.out.println("Failed to connect: " + phoneNumber);
```
toString() がオーバーライドされていないと、以下のような出力がされる。
```
Failed to connect: PhoneNumber@aabb
```
一見すると何を表しているのかがわからないので、電話番号を返すように toString() をオーバーライドすると良い。その場合は具体的に何の値を返すのかという仕様をメソッドに記述しておくこと。

## 問題
toString() メソッドをオーバライドする動機を述べよ。

<details>
<summary>解答</summary>

デフォルトの toString() メソッドではそのクラスが何を表しているかがわかりにくい。そこでそのクラスを端的に表す文字列を返すことで、デバッグしやすくするため。

</details>


# 項目17 可変性を最小限にする

クラスを不変にするための原則は以下の通り。  
1. オブジェクトの状態を変更するメソッドを提供しない  
2. クラスを拡張できないようにする(サブクラスの生成を防ぐ)  
3. 全てのフィールドを private にする  
4. 全てのフィールドを final にする  
5. 可変コンポーネントの参照をそのままクライアントに渡してはならない  

## 問題
以下の文章はJavaにおいてクラスを不変にするための原則を述べたものである。括弧に当てはまる単語を答えよ。

1. (1)を変更するメソッドを提供しない  
2. クラスを(2)できないようにする(サブクラスの生成を防ぐ)  
3. 全てのフィールドを(3)にする  
4. 全てのフィールドを(4)にする  
5. (5)の参照をそのままクライアントに渡してはならない  

<details>
<summary>回答</summary>

(1) オブジェクトの状態  
(2) 拡張  
(3) final  
(4) private  
(5) 可変コンポーネント  

</details>

# 項目18 継承よりもコンポジションを選ぶ
- 継承を使用すると、サブクラスでオーバーライドしたメソッドがスーパークラスの実装に依存する場合がある
  - 例) HashSet の addAll は内部で add が呼ばれている
- 継承よりも移譲を用いる方が良い。移譲とは、あるインターフェースを実装する場合に、private フィールドにメソッドの実装を任せることである。
- 継承は、例えばクラスAとBの間に A is a part of B の関係が存在する場合に使用する

# 項目20 抽象クラスよりもインターフェースを選ぶ
クラスを拡張する場合、Javaではクラスを一つしか継承できないので、インターフェースを使用するのが望ましい。また、Java8 からはインターフェースはデフォルトメソッドを実装することができるようになったが、それでもインターフェースでは toString() や hashCode() メソッドの実装を行うことができない。しかし、TemplateMethod パターンを使用した骨格実装という方法を用いると、この欠点を解消することができる。  

```java
public abstract class AbstractMapentry<K, V> implements Map.Entry<K, V> {     
    @Override
    public V setValue(V value) {
        throw new UnsupportedOperationException();
    }

    @Override
    public boolean equals(Object obj) {
        if (obj == this) {
            return true;
        }
        if (!(obj instanceof Map.Entry)) {
            return false;
        }
        Map.Entry<?,?> e = (Map.Entry) obj;
        return Objects.equals(e.getKey(), getKey()) && Objects.equals(e.getKey(), getValue());
    }

    @Override
    public int hashCode() {
        return Objects.hashCode(getKey()) ^ Objects.hashCode(getValue());
    }

    @Override
    public String toString() {
        return getKey() + "=" + getValue();
    }
}
```

## 問題

以下の文章は抽象クラスとインターフェースの使用について述べたものである。括弧に当てはまる単語を答えよ。  

クラスを拡張する場合、Javaではクラスを一つしか継承できないので、(1)を使用するのが望ましい。また、Java8 からはインターフェースは(2)を実装することができるようになったが、それでもインターフェースでは (3) や (4) メソッドの実装を行うことができない。しかし、(5) パターンを使用した(6)という方法を用いると、この欠点を解消することができる。

<details>
<summary>解答</summary>

1. インターフェース  
2. デフォルトメソッド  
3. toString()  
4. hashCode()  
5. Template Method  
6. 骨格実装  

Effective Java 項目20 より

</details>

# 項目21 将来のためにインターフェースを設計する

既存のインターフェースに新たにメソッドを追加しなければならなくなった場合、そのインターフェースを実装している全てのクラスはその追加されたメソッドを実装しなければならない。このコストを軽減するために、インターフェースのデフォルト実装を用いる方法があるが、これは好ましくない。なぜなら、そのデフォルト実装は実装クラスの知識を持っておらず、実装クラスでそのデフォルト実装のメソッドを呼び出したときに例外が発生する場合があるからである。例えば、デフォルト実装が並列処理を前提としない設計であった場合、実装クラスで並列処理を行うことを想定している場合には、例外が発生する。したがってインターフェースの設計は注意深く行うべきである。

# 項目22 型を定義するためだけにインターフェースを使う
インターフェースを利用するモチベーションとしては、複数の型を同一にみなして処理を行うために共通のインターフェースを実装させるという点があげられる。逆に言えば、それ以外の用途でインターフェースを定義するのは望ましくない。  
例えば、定数のみを定義した定数インターフェースを考えると、その定数を使用するためには必ずそのインターフェースを実装しなければならない。その場合、改修によってその定数を使用しなくなった場合でも互換性を考慮してそのインターフェースの実装を継続しなければならず、開発者は「何でこのインターフェースを実装しているのか」と混乱してしまう。  
これを避けるためには、ユーティリティクラスで定数を宣言し、利用側はそのクラスを使用するようにすれば、必要なくなった場合でも柔軟な対応が可能となる。ちなみに static フィールドを参照したい場合は、static インポートを用いることで、定数の頭にクラス名をつけなくてよくなる。

```java
import static com.effectivejava.science.PhysicalConstants.*;

public class Test {
    double atoms(double mols) {
        return AVOGADROS_NUMBER * mols;
    }
}
```

## 問題1
以下の文章はインターフェースの用途に関して述べた文章である。括弧に当てはまる単語を答えよ。  

インターフェースを利用するモチベーションとしては、複数の型を(1)にみなして処理を行うために共通のインターフェースを実装させるという点があげられる。逆に言えば、それ以外の用途でインターフェースを定義するのは望ましくない。  
例えば、定数のみを定義した(2)を考えると、その定数を使用するためには必ずそのインターフェースを実装しなければならない。その場合、改修によってその定数を使用しなくなった場合でも(3)を考慮してそのインターフェースの実装を継続しなければならず、開発者は「何でこのインターフェースを実装しているのか」と混乱してしまう。   
これを避けるためには、(4)クラスで定数を宣言し、利用側はそのクラスを使用するようにすれば、必要なくなった場合でも柔軟な対応が可能となる。

<details>
<summary>解答1</summary>

1. 同一   
2. 定数インターフェース   
3. 互換性  
4. ユーティリティ
Effective Java 項目22 より

</details>

## 問題2

static インポートのやり方を述べよ。

<details>
<summary>解答2</summary>

```java
static import hoge.Fuga.*
```

Effective Java 項目22 より

</details>

# 項目23 タグ付きクラスよりもクラス階層を選ぶ

以下はオブジェクトの属性をタグで表現する実装である。
```java
class Figure {
    enum Shape {RECTANGLE, CIRCLE};

    // タグフィールド
    final Shape shape;

    // shape が RECTANGLE である場合にだけこれらのフィールドが使用される
    double length;
    double width;

    // shape が CIRCLE である場合にだけこれらのフィールドが使用される
    double radius;

    // 円のコンストラクタ
    Figure(double radius) {
        shape = Shape.CIRCLE;
        this.radius = radius;
    }

    // 長方形のコンストラクタ
    Figure(double length, double width) {
        shape = Shape.RECTANGLE;
        this.length = length;
        this.width = width;
    }

    double area() {
        switch (shape) {
            case RECTANGLE:
                return length*width;
            case CIRCLE:
                return Math.PI * (radius*radius);
            default:
                throw new AssertionError(shape);
        }
    }
}
```
このクラスの欠点としては、以下のような事項があげられる。  
1. 長方形と円の概念が混在している。 
2. 長方形の場合、`radius` フィールドは使用されないので、メモリの無駄使いとなる。また、その逆も然り。  
3. タグによって無関係なフィールドが存在するので、データの不整合が起こる可能性がある。  

したがって、以下のようにクラスを分割する。
```java
abstract class Figure {
    abstract double area();
}

class Rectangle extends Figure {
    final double length;
    final double width;

    public Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }

    @Override
    double area() {
        return length * width;
    }
}

class Circle extends Figure {
    final double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    double area() {
        return Math.PI * (radius * radius);
    }
}
```

## 問題
タグ付きクラスの例として、以下のような実装が存在する。
```java
class Figure {
    enum Shape {RECTANGLE, CIRCLE};

    // タグフィールド
    final Shape shape;

    // shape が RECTANGLE である場合にだけこれらのフィールドが使用される
    double length;
    double width;

    // shape が CIRCLE である場合にだけこれらのフィールドが使用される
    double radius;

    // 円のコンストラクタ
    Figure(double radius) {
        shape = Shape.CIRCLE;
        this.radius = radius;
    }

    // 長方形のコンストラクタ
    Figure(double length, double width) {
        shape = Shape.RECTANGLE;
        this.length = length;
        this.width = width;
    }

    double area() {
        switch (shape) {
            case RECTANGLE:
                return length*width;
            case CIRCLE:
                return Math.PI * (radius*radius);
            default:
                throw new AssertionError(shape);
        }
    }
}
```
また、以下の文章はこの実装の欠点を述べたものである。括弧に当てはまる単語を答えよ。

1. RECTANGLEとCIRCLEの概念が(1)している。 
2. 長方形の場合、`radius`フィールドは使用されないので、(2)の無駄使いとなる。また、その逆も然りである。  
3. タグによって無関係なフィールドが存在するので、(3)が起こる可能性がある。  

<details>
<summary>解答</summary>

1. 混在   
2. メモリ   
3. データの不整合   
Effective Java 項目23 より

</details>

# 項目24 非 static のメンバークラスよりも static のメンバークラスを選ぶ

Java ではネストしたクラスを定義することができるが、ネストしたクラスは 4つのタイプが存在する。  

|  種類  |  説明  |
|:--:|:---|
|  static のメンバークラス  |  あるクラス内で static 宣言されたクラス  |
|  非 static のメンバークラス  |  あるクラス内で static 宣言されていないクラス  |
|  無名クラス  |  名前を持たないメソッド内で宣言されるその場限りのクラス  |
|  ローカルクラス  |  メソッド内で定義された名前を持ったクラス  |

static のメンバークラスと非 static のメンバークラスの違いはエンクロージングクラスのとの関連にある。static のメンバークラスは、エンクロージングクラスとの関連は比較的薄いが、非 static のメンバークラスはインスタンス生成時には必ずエンクロージングクラスのインスタンスを介して行われるので、関連は強い。したがってエンクロージングインスタンスへの関連が必要ない、すなわち関連を辿ってエンクロージングインスタンスにアクセスする必要がない場合はメンバークラスは static なものにした方が良い。なぜなら、メンバークラスがエンクロージングインスタンスへの参照を保持するのにコストがかかり、なおかつ参照が消えないためにガベージコレクションの対象にならず、メモリリークの原因となってしまうから。

## 問題
以下はJava におけるネストしたクラスについての文章である。括弧に当てはまる単語を答えよ。  

ネストしたクラスには4つの種類が存在し、それは (1), (2), (3), (4) である。ここで (1) と (2) の違いは(5)のとの関連にある。(1) は、(5)との関連は比較的薄いが、(2)はインスタンス生成時には必ず(5)のインスタンスを介して行われるので、関連は強い。したがって(6)への関連が必要ない、すなわち関連を辿って(6)にアクセスする必要がない場合は(1)にした方が良い。なぜなら、メンバークラスが(6)への参照を保持するのにコストがかかり、なおかつ参照が消えないために(7)の対象にならず、(8)の原因となってしまうからである。


<details>
<summary>解答</summary>

1. static のメンバークラス  
2. 非 static のメンバークラス  
3. 無名クラス  
4. ローカルクラス  
5. エンクロージングクラス  
6. エンクロージングインスタンス  
7. ガベージコレクション  
8. メモリリーク  

Effective Java 項目24 より
</details>

# 項目25 ソースファイルを単一のトップレベルのクラスに限定する

一つのソースファイルに複数のトップレベルのクラスを定義するのは、ややこしくなったりコンパイル時に挙動がおかしくなる可能性があるので、避ける。もしどうしても複数のトップレベルのクラスを定義する場合は static のメンバークラスとして定義する。

# 項目27 無検査警告を取り除く

ジェネリクスを用いてコーディングを行う場合、無検査警告を無視せず、できる限り全てを取り除くようにする。警告を取り除くことはClassCastExceptionを発生しないことを意味するからである。もし警告を取り除くことができないのであれば、型安全であることが保証できる箇所にのみ@SuppressWarningsアノテーションを付与して警告を抑制する。この場合変数宣言に対してアノテーションを付与することになるが、できる限り付与するスコープは小さくする。  
以下、無検査警告が発生する例。
```java
Set<Lark> exaltation = new HashSet();
```
HashSet<>() とすれば取り除くことが可能。

# 項目28 配列よりもリストを選ぶ
- 配列は共変であるのに対し、リストは不変であるため、リストを使用した方が良い。共変とは、例えばクラス A がクラス B のサブタイプだとすると、A[] も B[]のサブタイプとなる特性のことをさす。したがって、以下のコードはコンパイル時にはエラーとならず、実行時に例外が発生する。
```
Object[] objectArray = new Long[1];
objectArray[0] = "I don't fit in.";
```
- また、配列はジェネリクスを使用すると、未検査キャスト警告を出すが、リストは警告を出さないため安全である。

# 項目35 序数の代わりにインスタンスフィールドを使う

以下のような enum を定義したとする。
```java
public enum Ensemble {
    SOLO, DUET, TRIO, QUARTET, QUINTET,
    SEXTET, SEPTET, OCTET, NONET, DECTET;
    public int numberOfMusicians() { return ordinal() + 1;}
}
```
この enum クラスには以下の欠点が存在する。  
1. 順序を変更すると、各 `nuberOfMusicians()` メソッドの戻り値も変更されてしまう。  
2. 序数は順番に割り当てられていくために、飛び飛びの値をもつ enum を作成することが難しい。  
解決策として、インスタンスフィールドに序数を保持するようにする。

```java
public enum Ensemble {
    SOLO(1), DUET(2), TRIO(3), QUARTET(4), QUINTET(5),
    SEXTET(6), SEPTET(7), OCTET(8), NONET(9), DECTET(10);

    private int numberOfMusicians;

    Ensemble(int numberOfMusicians) {
        this.numberOfMusicians = numberOfMusicians;
    }

    public int numberOfMusicians() {
        return numberOfMusicians;
    }
}
```

## 問題
以下のような enum の欠点を述べよ。またその解決方法を述べよ。
```java
public enum Ensemble {
    SOLO, DUET, TRIO, QUARTET, QUINTET,
    SEXTET, SEPTET, OCTET, NONET, DECTET;
    public int numberOfMusicians() { return ordinal() + 1;}
}
```

<details>
<summary>解答</summary>

1. 順序を変更すると、各 `nuberOfMusicians()` メソッドの戻り値も変更されてしまう。  
2. 序数は順番に割り当てられていくために、飛び飛びの値をもつ enum を作成することが難しい。  
解決策として、インスタンスフィールドに序数を保持するようにする。

```java
public enum Ensemble {
    SOLO(1), DUET(2), TRIO(3), QUARTET(4), QUINTET(5),
    SEXTET(6), SEPTET(7), OCTET(8), NONET(9), DECTET(10);

    private int numberOfMusicians;

    Ensemble(int numberOfMusicians) {
        this.numberOfMusicians = numberOfMusicians;
    }

    public int numberOfMusicians() {
        return numberOfMusicians;
    }
}
```
Effective Java 項目35 より
</details>

# 項目36 ビットフィールドの代わりに EnumSet を使う

何かの要素の集合を用いて何かしらの処理を行いたい場合、選択肢として以下のような実装が考えられる。
```java
public class Text {
    public static final int STYLE_BOLD = 1 << 0;
    public static final int STYLE_ITALIC = 1 << 1;
    public static final int STYLE_UNDERLINE = 1 << 2;
    public static final int STYLE_STRIKETHROUGH = 1 << 3;

    public void applyStyles(int styles) {
        ...
    }
}
```
この場合、`text.applyStyles(STYLE_BOLD | STYLE_ITALIC)` のように呼び出せば、集合を用いた処理を実現することができる。このような表現をビットフィールド表現と呼ぶ。しかし、この実装には以下の欠点が存在する。  
1. 必要とするビット数によって型を int か long かを選択しなければならない。  
2. イテレーションを伴う処理を実装する場合には、使い勝手が悪くなる。  
以上のような欠点を解消するために、EnumSet を使用した実装を行うとよい。

```java
public class Text {
    public enum Style { BOLD, ITALIC, UNDERLINE, STRIKETHROUGH}

    public void applyStyles(Set<Style> styles) { ... }
}
```
この場合、`text.applyStyles(EnumSet.of(Style.BOLD, Style.ITALIC));` といった呼び出しが可能となる。ちなみに `applyStyles()`メソッドの引数は実装型ではなく、インターフェースなので柔軟な設計となっている。

## 問題1

以下の文章はビットフィールド表現による集合を使用した処理の欠点を述べたものである。括弧に当てはまる単語を述べよ。  
1. 必要とするビット数によって型を (1)か(2) かを選択しなければならない。  
2. (3)を伴う処理を実装する場合には、使い勝手が悪くなる。  

<details>
<summary>解答1</summary>

1. int  
2. long  
3. イテレーション

Effective Java 項目36 より

</details>

## 問題2

以下の実装はビットフィールド表現を用いた集合を使用した処理を実装したものである。これを EnumSet を使用した実装に書き換えよ。

```java
public class Text {
    public static final int STYLE_BOLD = 1 << 0;
    public static final int STYLE_ITALIC = 1 << 1;
    public static final int STYLE_UNDERLINE = 1 << 2;
    public static final int STYLE_STRIKETHROUGH = 1 << 3;

    public void applyStyles(int styles) {
        ...
    }
}
```

<details>
<summary>解答2</summary>

```java
public class Text {
    public enum Style { BOLD, ITALIC, UNDERLINE, STRIKETHROUGH}

    public void applyStyles(Set<Style> styles) { ... }
}
```

Effective Java 項目36 より

</details>

# 項目40 常に Override アノテーションを使う

toString() メソッドや hashCode() メソッドをオーバーライドするときは、Override アノテーションを使用するともし間違った実装を行ってしまったときに気づくことができる。例えば、以下のように equals() メソッドを定義したとする。  
```java
public class Hoge {
    @Override
    public equals(Hoge hoge) {
        ...
    }
}
```
この場合コンパイルエラーが発生する。なぜなら、equals メソッドの引数は `Object`クラスであるからである。したがって誤った実装を行った場合にはコンパイル時に気づくことができるので、Override アノテーションを付与した方が良い。  
しかし、例外も存在する。それは抽象クラスで定義されている抽象メソッドをオーバーライドするときはoOverride アノテーションをつけなくても構わない。しかし、ある抽象クラスで定義されているメソッドについて注意を引きたい場合はアノテーションをつけても構わない。

## 問題

以下は Override アノテーションに関して述べた文章である。括弧に当てはまる単語を答えよ。  
toString() や equals() などのメソッドをオーバーライドするときは Override アノテーションを付与した方が良い。なぜなら、(1)が間違っていた場合に(2)が発生し、誤った実装であることに気づくことができるからである。しかし、(3)クラスで定義されている(3)メソッドに関してはアノテーションを付与しなくても構わない。  

<details>
<summary>解答</summary>

1. シグニチャ  
2. コンパイルエラー  
3. 抽象  
Effective Java 項目40 より

</details>

# 項目41 型を定義するためにマーカーインターフェースを使う
マーカーインターフェースがマーカーアノテーションに機能的に勝っている点としては、共通の型として定義できるために、コンパイル時にエラーが発生する可能性がある箇所を検知できる点である。例えば、Serializable インターフェースはマーカーインターフェースであり、これを実装しているクラスはシリアライズ可能であるということを宣言している。実際に `OutputStream.writeObject`メソッドで OutputStream へ書き込むことができるが、残念ながらこのメソッドは　Serializable インターフェースを実装していないクラスでも指定することができてしまう。これは`writeObject`メソッドの引数が `Serializable` であればコンパイル時にエラーを検知できたはずである。 

## 問題
以下の文章はマーカーアノテーション/インターフェースに関して述べた文章である。括弧に当てはまる単語を答えよ。  

マーカーインターフェースとは(1)を含んでいないインターフェースであり、クラスが何らかの(2)を持っていることを表すものである。似たような機能としてマーカーアノテーションが存在するが、マーカーインターフェースがマーカーアノテーションに機能的に勝っている点としては、クラスを(3)として定義できるために、(4)が発生する点である。例えば、(5) インターフェースはマーカーインターフェースであり、これを実装しているクラスは(6)可能であるということを宣言している。実際に `OutputStream.writeObject`メソッドで OutputStream へ書き込むことができるが、残念ながらこのメソッドは　(4) インターフェースを実装していないクラスでも指定することができてしまう。これは`writeObject`メソッドの引数が `(4)` であればコンパイル時にエラーを検知できたはずである。 

<details>
<summary>解答</summary>

1. メソッド宣言  
2. 特性  
3. 共通の型  
4. コンパイルエラー  
5. Serializable  
6. シリアライズ  
Effective Java 項目41 より

</details>


# 項目42 無名クラスよりもラムダを選ぶ

ある文字列の配列の要素をソートしたい時、以下のようなコードを実装したとする。

```java
Collections.sort(list, new Comparator<String>() {
    @Override
    public int compare(String s1, String s2) {
        return Integer.compare(s1.length(), s2.length());
    }
});
```
少しこれは冗長な感じがする。ラムダを使用すると以下のように実装することができる。

```java
Collections.sort(list, (s1, s2) -> Integer.compare(s1.length(), s2.length()));
```
これでスッキリしたが、さらに以下のように実装することでコードの量を減らすことができる。

```java

// さらに簡略化
Collections.sort(list, Comparator.comparingInt(String::length));

// さらにさらに簡略化
list.sort(Comparator.comparingInt(String::length));
```
このように、ラムダを使用した方が冗長なコーディングを避けることができる。ちなみに、`Comparator` インターフェースは単一の抽象メソッドをもつことから関数型インターフェースと呼ばれている。`Collections.sort()`メソッドの第二引数や`List.sort()`メソッドの第一引数はこの関数型インターフェースを指定しており、これによってソートの方法を決定する。  
ラムダの実用的な使い方は他にも存在する。ある enum クラスを考えた時、それぞれの enum で違う処理を実装したい場合は以下のような実装が考えらえる。

```java
public enum Operation {
    PLUS("+") {
        @Override
        public double apply(double x, double y) {
            return x + y;
        }
    },
    TIMES("*") {
        @Override
        public double apply(double x, double y) {
            return x * y;
        }
    },
    DIVIDE("/") {
        @Override
        public double apply(double x, double y) {
            return x / y;
        }
    };

    private final String symbol;

    Operation(String symbol) {
        this.symbol = symbol;
    }

    @Override
    public String toString() {
        return super.toString();
    }

    public abstract double apply(double x, double y);
}
```
これに対してラムダを使用すると以下のように簡略化できる。

```java
public enum Operation {
    PLUS("+", (x, y) -> x + y),
    TIMES("*", (x, y) -> x * y),
    DIVIDE("/", (x, y) -> x / y);

    private final String symbol;
    private final DoubleBinaryOperator op;

    Operation(String symbol, DoubleBinaryOperator op) {
        this.symbol = symbol;
        this.op = op;
    }

    @Override
    public String toString() {
        return super.toString();
    }

    public double apply(double x, double y) {
        return op.applyAsDouble(x, y);
    }
}
```
このように、フィールドに`DoubleBinaryOperator`という関数型インターフェースを追加し、それに`apply`メソッドの処理を移譲させるという構成にしている。ちなみに、enum で定義した処理は、インスタンスフィールドすることができないので注意。  
以上より、無名クラスよりラムダを使用した方が、全体的にコードはスッキリするが、このような処理ができるのは関数型インターフェースだからである。複数の抽象メソッドをもつインターフェースに関してはこういった変換はできないので、その時は無名クラスを使用すること。

## 問題1
関数型インターフェースとは何か答えよ。

<details>
<summary>解答1</summary>
抽象メソッドを一つだけもつインターフェースのこと。  
Effective Java 項目43 より
</details>

## 問題2

以下のコードをラムダを使用してシンプルな実装に変換せよ。
```java
Collections.sort(list, new Comparator<String>() {
    @Override
    public int compare(String s1, String s2) {
        return Integer.compare(s1.length(), s2.length());
    }
});
```

<details>
<summary>解答2</summary>
list.sort(Comparator.comparingInt(String::length));  
Effective Java 項目43 より
</details>

## 問題3

以下の実装を関数型インターフェースを用いてシンプルにせよ。
```java
public enum Operation {
    PLUS("+") {
        @Override
        public double apply(double x, double y) {
            return x + y;
        }
    },
    TIMES("*") {
        @Override
        public double apply(double x, double y) {
            return x * y;
        }
    },
    DIVIDE("/") {
        @Override
        public double apply(double x, double y) {
            return x / y;
        }
    };

    private final String symbol;

    Operation(String symbol) {
        this.symbol = symbol;
    }

    @Override
    public String toString() {
        return super.toString();
    }

    public abstract double apply(double x, double y);
}
```

<details>
<summary>解答3</summary>

```java
public enum Operation {
    PLUS("+", (x, y) -> x + y),
    TIMES("*", (x, y) -> x * y),
    DIVIDE("/", (x, y) -> x / y);

    private final String symbol;
    private final DoubleBinaryOperator op;

    Operation(String symbol, DoubleBinaryOperator op) {
        this.symbol = symbol;
        this.op = op;
    }

    @Override
    public String toString() {
        return super.toString();
    }

    public double apply(double x, double y) {
        return op.applyAsDouble(x, y);
    }
}
```
Effective Java 項目43 より
</details>

# 項目43 ラムダよりもメソッド参照を選ぶ

ラムダよりもメソッド参照の方がシンプルに書けるので、メソッド参照で書ける時はメソッド参照を使用する。  

ラムダを使用した例
```java
map.merge(key, 1, (count, incr) -> count + incr);
```

メソッド参照を使用した例
```java
map.merge(key, 1, Integer::sum);
```

## 問題

以下のコードをメソッド参照を使用した実装に変換せよ。
```java
map.merge(key, 1, (count, incr) -> count + incr);
```

<details>
<summary>解答</summary>

```java
map.merge(key, 1, Integer::sum);
```

Effective Java 項目43 より
</details>

# 項目44 関数型インターフェースを使う
極力関数型インターフェースは自作せずに、標準のもので条件を満たせるのであれば、それを使用することで関数型インターフェースに対する理解を深めることができる。しかし、他の関数でより標準なものが存在する場合や、標準の関数型インターフェースで賄えるものが存在しない場合は使用は見送る。よく使われる関数型インターフェースは以下の通り。  

|  インターフェース  |  関数  |  例  |
|:---|:---|:---|
|  UnaryOperator<T>  |  T apply(T t)  |  String::toLowerCase  |
|  BinaryOperator<T>  |  T apply(T t1, T t2)  |  BigInteger::add  |
|  Predicate<T>  |  boolean test(T t)  |  Collection::isEmpty  |
|  Function<T,R>  |  R apply(T t)  |  Arrays::asList  |
|  Supplier<T>  |  T get()  |  Instant::now  |
|  Consumer<T>  |  void accept(T t)  |  System.out::println  |


## 問題

以下の文章は関数型インターフェースについて述べた文章である。括弧に当てはまる単語を述べよ。  
極力関数型インターフェースは(1)せずに、標準のもので条件を満たせるのであれば、それを使用することで関数型インターフェースに対する理解を深めることができる。しかし、他の関数でより(2)なものが存在する場合や、(2)の関数型インターフェースで賄えるものが(3)しない場合は使用は見送る。よく使われる関数型インターフェースは以下の通り。   

UnaryOperator<T>  T apply(T t)  
例) (4)  
BinaryOperator<T>  T apply(T t1, T t2)  
例) (5)  
Predicate<T>  boolean test(T t)  
例) (6)  
Function<T,R>  R apply(T t)  
例) (7)  
Supplier<T>  T get()  
例) (8)  
Consumer<T>  void accept(T t)  
例) (9)  

<details>
<summary>解答</summary>

1. 自作  
2. 標準  
3. 存在  
4. String::toLowerCase  
5. BigInteger::add  
6. Collection::isEmpty  
7. Arrays::asList  
8. Instant::now  
9. System.out::println  
Effective Java 項目44 より  

</details>

# 項目51 メソッドのシグニチャを注意深く設計する
1. メソッド名を注意深く選ぶ  
【参考】 https://qiita.com/Ted-HM/items/7dde25dcffae4cdc7923  
2. 便利なメソッドを提供しすぎない  
メソッドを増やすとテストや捕手が大変になるので極力避ける  
3. 長いパラメータのリストは避ける  
4個以下を目標にする。また同一のパラメータが続くのは誤用の危険性が増すのでやめる。どうすればいいのかは以下を参照    
- 複数のメソッドに分割し、パラメータを減らす  
- パラメータの集まりを保持するヘルパークラスを用意する  
- パラメータの一部がオプションである場合は、Builder パターンを採用し、必要なパラメータがセットされた時点で execute() メソッドを実行して状態検証を行い、処理を実行する。  
4. 変更容易性のためパラメータに関しては、クラスよりもインターフェースを選ぶ  
5. 変更容易性のため boolean パラメータよりは、2つの要素を持つ enum を使用する  

## 問題

以下はメソッドのシグニチャについて述べた文章である。括弧に当てはまる単語を述べよ。  

1. (1)を注意深く選ぶ  
2. 便利な(2)を提供しすぎない  
3. 長い(3)のリストは避ける。方法は以下。  
- 複数の(2)に分割し、パラメータを減らす  
- パラメータの集まりを保持する(4)を用意する  
- パラメータの一部がオプションである場合は、(5) パターンを採用し、必要なパラメータがセットされた時点で execute() メソッドを実行して状態検証を行い、処理を実行する。  
4. 変更容易性のためパラメータに関しては、クラスよりも(6)を選ぶ  
5. 変更容易性のため boolean パラメータよりは、2つの要素を持つ (7) を使用する  

<details>
<summary>解答</summary>

(1) メソッド名  
(2) メソッド  
(3) パラメータ  
(4) ヘルパークラス  
(5) Builder  
(6) インターフェース  
(7) Enum

Effective Java 項目51 より
</details>

# 項目54 null ではなく、空コレクションか空配列を返す。
コレクションや配列を返すメソッドで処理の結果 null を返す可能性のあるメソッドを実装すると、呼び出し側で null チェックを行う必要があり、メソッドが扱いづらくなる。したがって空のコレクションや空の配列を返すのが望ましい。

悪い例
```java
public List<Cheese> getCheeses() {
    return cheessInStock.isEmpty() ? null : new ArrayList<>(cheesesInStock)
}
```

いい例
```java
public List<Cheese> getCheeses() {
    return new ArrayList<>(cheesesInStock)
}
```

## 問題
コレクションや配列を返すメソッドを実装するときに、null を返してはいけない理由を述べよ。

<details>
<summary>解答</summary>

メソッドを使用する側で null チェックを行う必要があり、メソッドが扱いづらいものとなるから。  
Effective Java 項目54 より

</details>

# 項目58 従来のforループよりもfor-each　ループを選ぶ

例えば6×6通りの出力をしたい時、以下のようなコードを実装したとする
```java
enum Face {ONE, TWO, THREE, FOUR, FIVE, SIX}

public static void main(String[] args) throws InterruptedException {
    Collection<Face> faces = EnumSet.allOf(Face.class);
    for (Iterator<Face> i = faces.iterator(); i.hasNext();)
        for (Iterator<Face> j = faces.iterator(); j.hasNext();)
            System.out.println(i.next() + " " + j.next());
}
```
この出力結果は以下の通りである。
```
ONE ONE
TWO TWO
THREE THREE
FOUR FOUR
FIVE FIVE
SIX SIX
```
内部のループで外部のCollection の　next　が呼ばれているため、36通りの出力はされず、６通りで打ち止めとなる。for-each　を使用するとそういった自体を回避することができる。

```java
enum Face {ONE, TWO, THREE, FOUR, FIVE, SIX}

public static void main(String[] args) throws InterruptedException {
    Collection<Face> faces = EnumSet.allOf(Face.class);
    for (Face face : faces) {
        for (Face face1 : faces) {
            System.out.println(face + " " + face1);
        }
    }
}
```

## 問題
6×6通りの出力をしたいため、以下のようなコードを実装したが、６通りの出力しかされなかった。その理由と改善方法を述べよ。

```java
enum Face {ONE, TWO, THREE, FOUR, FIVE, SIX}

public static void main(String[] args) throws InterruptedException {
    Collection<Face> faces = EnumSet.allOf(Face.class);
    for (Iterator<Face> i = faces.iterator(); i.hasNext();)
        for (Iterator<Face> j = faces.iterator(); j.hasNext();)
            System.out.println(i.next() + " " + j.next());
}
```

<details>
<summary>解答</summary>
外部のイテレータを内部で呼び出しているから６通りしか出力されなかった。改善案としてはfor-each　を使用する。  
Effective Java 項目59 より
</details>

# 項目57 ローカル変数のスコープを最小限にする
コードの可読性と保守性が向上させ、誤りの可能性を減らすために、ローカル変数は必要になるタイミングで宣言することが望ましい。  
また、ループ変数を使用する場合は、 while ループを使用するよりも for ループを使用した方が、誤りを減らすことができる。  

while を使用したループ
```java
Iterator<Element> i = c.iterator();
while(i.hasNext()) {
    ...
}
```

for を使用したループ
```java
for (Iterator<Element> i = c.iterator(); i.hasNext(); ) {
    ...
}
```
while を使用する場合、ループ変数はブロックの外で宣言することになるので、以下のようなケースにバグを混入させてしまう。

```java
Iterator<Element> i = c.iterator();
while(i.hasNext()) {
    // 処理1
}

Iterator<Element> i2 = c2.iterator();
while(i.hasNext()) { // i1 変数を使用してループ処理を行っている!
    // 処理2
}
```
コピペで実装してしまうとこのようなミスをする可能性がある。一方で for ループを使用した書き方にすると、ブロック内でループ変数を定義するために、このようなバグは発生しない。
```java
for (Iterator<Element> i = c.iterator(); i.hasNext(); ){
    // 処理1
}

for (Iterator<Element> i2 = c2.iterator(); i.hasNext(); ){ // コンパイルエラーで気づくことができる
    // 処理2
}
```
また、ブロック内でループ変数を定義するため、i, i2 の宣言の出し分けは必要ない。さらにパフォーマンスを考慮すると、ループするたびに i.hasNext() を実行するのは効率的ではないため、以下のように実装した方が望ましい。
```java
for (int i = 0, int n = expensiveComputation(); i < n, i++) {
    ...
}
```

## 問題

以下の実装には問題点が存在する。その問題点を述べ、どのように解決すれば良いか述べよ。

```java
Iterator<Element> i = c.iterator();
while(i.hasNext()) {
    // 処理1
}

Iterator<Element> i2 = c2.iterator();
while(i.hasNext()) {
    // 処理2
}
```

<details>
<summary>解答</summary>

2回目のループで1回目のループ変数である i を使用している点。このようなミスを犯さないためには、for ループによる処理の実装を検討する。

```java
for (Iterator<Element> i = c.iterator(); i.hasNext(); ) {
    // 処理1
}

for (Iterator<Element> i = c2.iterator(); i.hasNext(); ) {
    // 処理2
}
```
Effective Java 項目57 より

</details>


# 項目59 ライブラリを知り、ライブラリを使う

- 乱数発生器を使用する場合は、`Random` よりも `ThreadLocalRandom`の方が早いのでそちらを使う。  
    - [ThreadLocalRandom (Java Platform SE 8 )](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ThreadLocalRandom.html)  
- java.lang, java.util, java.io およびそれらのサブパッケージの基本を知っておく。  
- 車輪の再発明は極力避けるべき、基本パッケージで要求を満たせるものが存在しなかったら、Google の Guava ライブラリといった高品質のサードパーティライブラリの使用を検討する。  
- InputStream クラスに指定した URL の内容を表示する transferTo() メソッドが追加された。

## 問題

以下の文章は「Effective Java 項目60 ライブラリを知り、ライブラリを使う」について述べたものである。括弧に当てはまる単語を答えよ。  
1. 乱数発生器を使用する場合は、`Random` よりも (1) の方が処理速度が早いのでそちらを使う。  
2. java.(2), java.(3), java.(4) およびそれらのサブパッケージの基本を知っておく。  
3. 車輪の再発明は極力避けるべき、基本パッケージで要求を満たせるものが存在しなかったら、Google の Guava ライブラリといった高品質の(5)の使用を検討する。  
4. InputStream クラスに指定した URL の内容を表示する (6) メソッドが追加された。

<details>
<summary>解答</summary>

1. ThreadLocalRandom  
2. lang  
3. util  
4. io  
5. サードパーティライブラリ  
6. transferTo()

</details>


# 項目60 正確な答えが必要ならば、float と double を避ける

double と float は正確な計算には向いていない  
悪い例
```java
public static void main(String[] args) {
    double funds = 1.0;
    int itemBought = 0;
    for (double price = 0.10; price <= funds; price += 0.10) {
        funds -= price;
        itemBought++;
    }
    System.out.println(itemBought + " items bought.");
    System.out.println("Change: $" + funds);
}
```

出力結果は以下の通り
```
3 items bought.
Change: $0.3999999999999999
```

解決策としては、int, long, BigDecimal を使用する。

```java
public static void main(String[] args) {
    final BigDecimal TEN_CENTS = new BigDecimal(".10");
    int itemBought = 0;
    BigDecimal funds = new BigDecimal("1.00");
    for (BigDecimal price = TEN_CENTS; price.compareTo(funds) < 0; price = price.add(TEN_CENTS)) {
        funds = price.subtract(price);
        itemBought++;
    }
    System.out.println(itemBought + " items bought.");
    System.out.println("Change: $" + funds);
}
```
BigDecimal の欠点としては単純な数値型よりも処理速度が低下する点である。これを避けたければ、int や long を使用して、小数点がどこにつくかを把握してコーディングすると良い。

## 問題

以下のコードには欠点が存在する。その欠点を述べ、改善策を実装せよ。

```java
public static void main(String[] args) {
    double funds = 1.0;
    int itemBought = 0;
    for (double price = 0.10; price <= funds; price += 0.10) {
        funds -= price;
        itemBought++;
    }
    System.out.println(itemBought + " items bought.");
    System.out.println("Change: $" + funds);
}
```

<details>
<summary>解答</summary>

精密性が求められる数値計算に double 型を使用している点。double や float は正確な値を提供しないため、精密な計算を行う場合は int, long, BigDecimal を使用する  
BigDecimal を使用した実装例
```java
public static void main(String[] args) {
    final BigDecimal TEN_CENTS = new BigDecimal(".10");
    int itemBought = 0;
    BigDecimal funds = new BigDecimal("1.00");
    for (BigDecimal price = TEN_CENTS; price.compareTo(funds) < 0; price = price.add(TEN_CENTS)) {
        funds = price.subtract(price);
        itemBought++;
    }
    System.out.println(itemBought + " items bought.");
    System.out.println("Change: $" + funds);
}
```
Effective Java 項目60 より
</details>

# 項目61 ボクシングされた基本データよりも基本データ型を選ぶ
Integer や Long と言ったようなボクシングされた基本データ型は、プログラムに対して冗長性を提供するが、危険性も提供してしまう。具体的には、アンボクシングを実行することにより、予期しない自体が発生する可能性がある。例えば以下のような Integer クラスを使用したコンパレータを実装したとする。

```java
Comparator<Integer> naturalOrder = (i, j) -> (i < j) ? -1 : (i == j ? 0 : 1);
```
このComparator に対して `naturalOrder.compare(new Integer(42), new Integer(42))` を実行した場合、0ではなく1が返される。これは Integer クラスに対して同一性比較 `==` を行っているために同じ値を表す Integer インスタンスであっても異なるインスタンスであった場合には `==` の結果は false となるためである。これを避けるためには、明示的に値をアンボクシングする必要がある。

```java
Comparator<Integer> naturalOrder = (iBoxed, jBoxed) -> {
    int i = iBoxed, j = jBoxed;
    (i < j) ? -1 : (i == j ? 0 : 1);
}
```
また以下のようなケースはアンボクシングとボクシングが繰り返されるため、パフォーマンスが低下するので実装は避けた方が良い。
```java
public static void main(String[] args) {
    Long sum = 0L;
    for (long i = 0; i < Integer.MAX_VALUE; i++){
        sum += i;
    }
    System.out.println(sum);
}
```
基本的には基本データ型を使用するのが望ましいが、コレクションには基本データ型を使用することができないので、その場合はボクシングされた基本データ型を使用する。

## 問題

以下のような Integer インスタンスを比較する Comparator を実装した。この実装の問題点と解決方法を述べよ。
```java
Comparator<Integer> naturalOrder = (i, j) -> (i < j) ? -1 : (i == j ? 0 : 1);
```

<details>

<summary>解答</summary>

ボクシングされた基本データ型なので、 値が同じだとしても同一性を検証する　`==` 演算子を使用した場合、false が返ってきて想定した挙動とならない。これを解消するためには、アンボクシングを行い基本データ型で比較を行う。

```java
Comparator<Integer> naturalOrder = (iBoxed, jBoxed) -> {
    int i = iBoxed, j = jBoxed;
    (i < j) ? -1 : (i == j ? 0 : 1);
}
```
Effective Java 項目61 より

</details>

# 項目62 他の型が適切な場所では、文字列を避ける

よりよいデータ型が存在する場合には String でオブジェクトを表現することは避ける。以下が主な理由である。  
1. Enum で定義できる場合には、String よりも Enum を使用した方がバグの発生を避けることができる  
2. String でデータを表現しようとすると、String が提供する振る舞いを強制されるため、本質的ではない  
3. 一意性が求められる場面での String の使用は誤作動の原因となる  

# 項目63 文字列結合のパフォーマンスに用心する
文字列の結合演算子である `+` は n 個の文字列を結合するのに 二次の計算量を必要とするので、極力使用は避ける。

```java
String[] temp = new String[] {"aaa", "bbb"};
StringBuilder sb = new StringBuilder();
for (String s : temp) {
    sb.append(s);
}
System.out.println(sb.toString());
```

## 問題
以下のコードの改善点について述べよ。そして実装せよ。  

```java
String[] temp = new String[] {"aaa", "bbb"};  
String str = "";  
for (String s : temp) {  
    str += s;  
}  
System.out.println(str);  
```

<details>
<summary>解答</summary>
ループ処理で文字列結合演算子 `+` を使用している点。パフォーマンスが低下するので、StringBuilder クラスを使用するとよい。  

```java
String[] temp = new String[] {"aaa", "bbb"};  
StringBuilder sb = new StringBuilder();  
for (String s : temp) {  
    sb.append(s);  
}  
System.out.println(sb.toString());  
```

Effective Java 項目63 より
</details>

# 項目64 インターフェースでオブジェクトを参照する

パラメータ、戻り値、変数、フィールドはできる限りインターフェースで宣言する。

いい例
```java
Set<Son> sonSet = new LinkedHashSet<>();
```

悪い例
```java
LinkedHashSet<Son> sonSet = new LinkedHashSet<>();
```
このような実装を行う理由としては、インターフェースで実装することにより、実装を柔軟に切り替えることができるからである。

## 問題
以下のコーディングの悪い点を述べよ。そしてその理由を述べよ。  

```java
LinkedHashSet<Son> sonSet = new LinkedHashSet<>();  
```

<details>
<summary>解答</summary>
パラメータ sonSet を実装クラス LinkedHashSet で宣言している点。パラメータ、戻り値、変数、フィールドはできる限りインターフェースで宣言する。このような実装を行う理由としては、インターフェースで宣言した方が実装を柔軟に切り替えることができるからである。  
Effective Java 項目64 より
</details>

# 項目65 リフレクションよりもインターフェースを選ぶ

リフレクションは強力な機能であるが、プロダクトでの使用は極力避けたほうが良い。理由は以下の通り。  
1. コンパイル時の型検査が行われないので、実行時にエラーが発生するリスクが生まれる。  
2. リフレクションを使用したコードはいわゆるおまじない的なものがあるため、実装が面倒かつ可読性が低い。  
3. 様々な理由が存在するが、リフレクションを使用するとパフォーマンスが低下する。  

また、リフレクションを利用してメソッドの呼び出しを行う場合、コンパイル時にはすでに既知となっているスーパークラスやインターフェースを利用してメソッドを呼び出すのが望ましい。

## 問題
以下の文章はリフレクションについて述べたものである。括弧に当てはまる単語を答えよ。  

リフレクションは強力な機能であるが、プロダクトでの使用は極力避けたほうが良い。理由は以下の通り。  

1. コンパイル時の(1)が行われないので、実行時にエラーが発生するリスクが生まれる。  
2. リフレクションを使用したコードはいわゆるおまじない的なものがあるため、実装が(2)かつ(3)が低い。  
3. 様々な理由が存在するが、リフレクションを使用すると(4)が低下する。  

また、リフレクションを利用してメソッドの呼び出しを行う場合、コンパイル時にはすでに既知となっている(5)や(6)を利用してメソッドを呼び出すのが望ましい。

<details>
<summary>解答</summary>

1. 型検査  
2. 面倒  
3. 可読性  
4. パフォーマンス  
5. スーパークラス  
6. インターフェース  

Effective Java 項目65 より

</details>

# 項目66 ネイティブメソッドを注意して使う

ネイティブメソッドの使用の欠点としては以下が挙げられる。  
1. ネイティブメソッド自体が安全ではないので、メモリ破壊系のエラーを受けるようになる。  
2. ネイティブ言語はJavaよりもプラットフォーム依存であり移植性が低いため、デバッグが困難になる。  
3. ネイティブメソッドの出入りにコストがかかるため、全体的にパフォーマンスが低下する可能性がある。  
4. ネイティブメソッドは書くのが面倒な「糊付けコード」を必要とし、可読性が低下する。

## 問題
以下の文章は Java におけるネイティブメソッドの使用に関するものである。括弧に当てはまる単語を答えよ。  

ネイティブメソッドの使用の欠点としては以下が挙げられる。  
1. ネイティブメソッド自体が安全ではないので、(1)系のエラーを受けるようになる。  
2. ネイティブ言語はJavaよりもプラットフォーム依存であり(2)が低いため、(3)が困難になる。  
3. ネイティブメソッドの出入りに(4)がかかるため、全体的に(5)が低下する可能性がある。  
4. ネイティブメソッドは書くのが面倒な「糊付けコード」を必要とし、(6)が低下する。

<details>
<summary>解答</summary>

1. メモリ破壊  
2. 移植性  
3. デバッグ  
4. コスト  
5. パフォーマンス  
6. 可読性  

Effective Java 項目66 より

</details>


# 項目71 チェックされる例外を不必要に使うのを避ける

例外が発生する API を使用するときに、取るべき行動は`try ~ catch`構文を使用して例外をチェックするか、`throws`句を用いて例外を外側に伝搬するかの2つである。この両方とも API の利用者の負荷を高める。また、stream では例外が発生するメソッドは使用できないため、汎用性が低い処理となってしまう。したがって、メソッドを柔軟に使用できるように、全ての例外をチェックするのではなく、例外が発生する可能性がある処理に対して、回復できるための何らかの処理が実装できる場合のみチェックし、それ以外はチェックしないように実装する。しかし、これはオブジェクトの整合性を保つため、非同期処理や外部要因で状態が変化する処理に対しては行わない。

チェックされない例外に変換する処理について説明する。以下のような処理があったとする。
```java
try {
    obj.action(args);
} catch(TheCheckException e) {
    // 例外処理を記述
}
```

チェックされない例外にするには以下のようにコードを変更する。
```java
if (obj.actionPermitted(args)) {
    obj.action(args)
} else {
    // 例外処理を記述
}
```
`actionPermitted` に関しては、例外が発生するかどうかを確認するメソッドである。

## 問題1
例外別のハンドリングの方針について述べよ。

<details>
<summary>解答1</summary>
発生した例外に対して、回復のために何か特別な処理を実装できる例外に関しては try ~ catch を用いてチェックする。それ以外はチェックしない。  
Effective Java 項目71 より
</details>

## 問題2
以下のコードで発生する例外をチェックしない例外に変換するような実装をせよ。

```java
try {
    obj.action(args);
} catch(TheCheckException e) {
    // 例外処理を記述
}
```

<details>
<summary>解答2</summary>

```java
if (obj.actionPermitted(args)) {
    obj.action(args)
} else {
    // 例外処理を記述
}
```
Effective Java 項目71 より
</details>

# 項目72 標準的な例外を使う

コードの再利用のため、例外をスローする場合は、ありきたりなケースに関しては既存の例外をスローすることが望ましい。例えば、ケースバイケースではあるが、メソッドに不正な引数を渡してしまった場合は、IllegalArgumentException をスローし、オブジェクトがメソッドを実行する前提条件の状態を満たしていなかった場合は IllegalStateException をスローするのが望ましい。また、シングルスレッドで操作されるオブジェクトがマルチスレッドで操作された場合は、ConcurrentModificationException をスローするのが望ましい。どのようなケースでどのような例外をスローするのかは、以下の表を参考にするとよい。

|  例外  |  使う機会  |
|:---|:---|
|  IllegalArgumentException  |  null ではないがパラーメータが不適切  |
|  IllegalStateException  |  メソッド呼び出しに対してオブジェクトの状態が不正  |
|  NullPointerException  |  パラメータが禁止されている null  |
|  IndexOutOfBoundsException  |  インデックスの値が範囲外  |
|  ConcurrentModificationException  |  禁止されているオブジェクトに並行した変更を検出  |
|  UnsupportedOperatoinException  |  オブジェクトがメソッドをサポートしていない  |

# 項目74 各メソッドがスローするすべての例外を文書化する

メソッドの使い勝手をよくするために、スローする例外をすべて Javadoc で文書化することが望ましい。これにより、メソッドの使用者は、そのメソッドを使用する前提条件を把握できるようになるので、誤った使用を防ぐことができる。また、例外にはチェックされる例外とチェックされない例外の2種類存在するが、チェックされない例外に関してもできる限り文書化することが望ましい。これは責任の所在を明確にするためである。また、チェックされない例外に対しては、@throws タグを使用せずに文書化する。

## 問題
以下の文章は例外の文書化について述べたものである。括弧に当てはまる単語を述べよ。

メソッドの使い勝手をよくするために、スローする例外をすべて Javadoc で文書化することが望ましい。これにより、メソッドの使用者は、そのメソッドを使用する(1)を把握できるようになるので、(2)を防ぐことができる。また、例外には(3)される例外と(3)されない例外の2種類存在するが、(3)されない例外に関してもできる限り文書化することが望ましい。これは(4)を明確にするためである。また、チェックされない例外に対しては、(5) タグを使用せずに文書化する。

<details>
<summary>解答</summary>

1. 前提条件  
2. 誤った使用  
3. チェック  
4. 責任の所在
5. @throws

Effective Java 項目74 より
</details>

# 項目75 詳細メッセージにエラー記録情報を含める
例外が発生した場合の原因の特定や、状況再現のためにできるだけ例外の内容は詳細なものにした方が良い。例えば、`IndexOutOfBoundsException` に関して述べると、配列の下限、上限、そしてインデックス番号をエラー内容に含めることで状況をより把握することが可能となる。  
しかし、エラー内容は時に様々な人が確認するので、パスワードなどの機密情報は載せるべきではない。また、エンドユーザに提供するエラーメッセージと違い、求められるのは詳細な内容なので、より実装によった内容であるほど好ましい。  
例外の詳細を記録する方法としては以下のような実装が考えられる。  

```java
public OutOfBoundsException(int lowerBound, int upperBound, int index) {
    super(String.format("Lower bound: %d, Upper bound: %d, Index: %d", lowerBound, upperBound, index));

    // 値にアクセスできるようにする
    this.lowerBound = lowerBound;
    this.upperBound = upperBound;
    this.index = index;
}
```

## 問題

以下の文章はエラーの詳細な内容について述べた文章である。括弧に当てはまる単語を述べよ。  

例外が発生した場合の(1)や、(2)のためにできるだけ例外の内容は詳細なものにした方が良い。例えば、`IndexOutOfBoundsException` に関して述べると、配列の下限、上限、そしてインデックス番号をエラー内容に含めることで状況をより把握することが可能となる。  
しかし、エラー内容は時に様々な人が確認するので、パスワードなどの(3)は載せるべきではない。また、エンドユーザに提供するエラーメッセージと違い、求められるのは詳細な内容なので、より(4)によった内容であるほど好ましい。  

<details>
<summary>解答</summary>

1. 原因特定  
2. 状況再現  
3. 機密情報  
4. 実装  

Effective Java 項目75 より
</details>

# 項目76 エラーアトミック性に努める

あるオブジェクトのメソッドを呼び出して例外がスローされた時、整合性を担保するためにそのオブジェクトをメソッドを呼び出す前の状態に戻しておくことが望ましい。このような性質をもつメソッドはエラーアトミックであると呼ばれる。エラーアトミックなメソッドを実現するためには、いくつかの方法が存在する。  

1. オブジェクトの操作を行う前に、パラメータのチェックを行う。

```java
public Object pop() {
    if (size == 0) {
        throw new EmptyStackException();
    }
    Object result = elements[--size];
    elements[size] = null; // 使われなくなった参照を取り除く
    return result;
}
```
このメソッドは if 文が存在しなくても例外がスローされるが、size の値がマイナスされているため、データは不整合を起こしている。さらに if 文を存在しない場合は `ArrayIndexOutOfBoundsException` がスローされるが、これは問題の本質を表していない(このケースでは、スタックに何も存在しないということが本質である)。  

2. 例外がスローされる操作をオブジェクトの状態を変更する操作より前で行う。  
3. 初期状態のオブジェクトを一時的にコピーしておいて、一連の処理の途中で例外がスローされた場合にはそのコピーしていた初期状態のオブジェクトを返す。  
4. 例外がスローされたら、操作が開始される前までオブジェクトの状態を戻す。　　

## 問題1
エラーアトミックなメソッドとは何か

<details>
<summary>解答1</summary>
あるオブジェクトのメソッドを呼び出して例外がスローされた時、整合性を担保するためにそのオブジェクトをメソッドを呼び出す前の状態にリセットされるメソッドのこと  
Effective Java 項目76 より
</details>

## 問題2
以下の文章はエラーアトミックなメソッドを実現する方法を述べたものである。空欄に当てはまる単語を述べよ。  

1. オブジェクトの操作を行う前に、(1)のチェックを行う。
2. 例外がスローされる操作を(2)する操作より前で行う。  
3. (3)を一時的にコピーしておいて、一連の処理の途中で例外がスローされた場合にはそのコピーしていた(3)を返す。  
4. 例外がスローされたら、(4)までオブジェクトの状態を戻す。　

<details>
<summary>解答</summary>
1. パラメータ  
2. オブジェクトの状態を変更  
3. 初期状態のオブジェクト  
4. 操作が開始される前  
Effective Java 項目76 より
</details>

# 項目77 例外を無視しない
空の catch ブロックで例外を無視しない。もし、無視するのが最善であると判断する場合は、その理由を明記し、変数を ignore と名付ける。


# 項目78 共有された可変データへのアクセスを同期する

以下のコードだと`stopRequested`は同期されず、Thread は終了しない
```java
class Scratch {
    private static boolean stopRequested;

    public static void main(String[] args) throws InterruptedException {
        Thread backgroundThread = new Thread(() -> {
            int i = 0;
            while (!stopRequested)
                i++;
        });
        backgroundThread.start();

        TimeUnit.SECONDS.sleep(1);
        stopRequested = true;
    }
}
```
なぜこのようなことが起きるかというと、上記の while 句は以下のコードを同義であるからである。
```java
if (!stopRequested) {
    while(true) {
        i++;
    }
}
```
最初にしか`stopRequested` が読み込まれないため、このような事象が発生する。これを避けるため、R/W で同期処理を実装する。

```java
class Scratch {
    private static boolean stopRequested;

    private static synchronized void requestStop() {
        stopRequested = true;
    }

    private static synchronized boolean stopRequested() {
        return stopRequested;
    }

    public static void main(String[] args) throws InterruptedException {
        Thread backgroundThread = new Thread(() -> {
            int i = 0;
            while (!stopRequested())
                i++;
        });
        backgroundThread.start();

        TimeUnit.SECONDS.sleep(1);
        requestStop();
    }
}
```

また、`hoge++` という操作は、変数 `hoge` の値を取得してその値に1を加えるという複数の処理を含んでいるため、アトミックではない。したがってその処理の合間に別スレッドが`hoge`の値を読み込み、それに対して1を加える操作を行うとすると、別スレッドで同じ値の`hoge`が存在することになり、一意性に欠け、不具合が生じる可能性がある。したがって、これを避けるためにもメソッドを`syncronized`で定義するか、もしくはAtomicLong を使用する。

```java
private static final AtomicLong nextSerialNum = new AtomicLong();
public static long generateSerialNumber() {
    return nextSerialNum.getAndIncrement();
}
```

## 問題1
以下のコードの問題点および解決方法を述べよ。  

```java
class Scratch {
    private static boolean stopRequested;

    public static void main(String[] args) throws InterruptedException {
        Thread backgroundThread = new Thread(() -> {
            int i = 0;
            while (!stopRequested)
                i++;
        });
        backgroundThread.start();

        TimeUnit.SECONDS.sleep(1);
        stopRequested = true;
    }
}
```

<details>
<summary>解答1</summary>
同期が行われないので while 文が終了しない。同期をさせるには、R/W 両方で syncronized 修飾子を使用する  

```java
class Scratch {
    private static boolean stopRequested;

    private static synchronized void requestStop() {
        stopRequested = true;
    }

    private static synchronized boolean stopRequested() {
        return stopRequested;
    }

    public static void main(String[] args) throws InterruptedException {
        Thread backgroundThread = new Thread(() -> {
            int i = 0;
            while (!stopRequested())
                i++;
        });
        backgroundThread.start();

        TimeUnit.SECONDS.sleep(1);
        requestStop();
    }
}
```
Effective Java 項目78 より
</details>

## 問題2
インクリメント演算子 ++ はアトミックな操作かどうかを答えよ。また、一意な数値を採番したい場合に有効な実装を述べよ。

<details>
<summary>解答2</summary>

アトミックな操作ではないため、一意な数値を採番したい場合は syncronized を使用するか、以下のコードのように AtomicLong クラスを使用する  

```java
private static final AtomicLong nextSerialNum = new AtomicLong();
public static long generateSerialNumber() {
    return nextSerialNum.getAndIncrement();
}  
```

Effective Java 項目78 より
</details>

# 項目83 遅延初期化を注意して使う

クラスやインスタンスの生成を極力避けたい場合、遅延初期化を行うことがある。遅延初期化とは、クラスフィールドが参照されたタイミングで初期化を行うことである。  
しかし、複数のスレッドで同時に遅延初期化が行われると、データの不整合が発生する。それを避けるため、　`syncronized` 修飾子でロックをかける必要がある。　　

```java
private FieldType field;

private synchronized FieldType getField() {
    if (field == null)
        field = computeFieldValue();
    return field;
}
```

しかし、メソッド全体にロックをかけると、今度はフィールドアクセスのコストが増大する可能性があり、なるべくそれは避けたい。  
static なフィールドに関しては、遅延初期化ホルダー・クラス・イディオムを使用することで、不必要なロックを避けることができる

```java
private static class FieldHolder {
    static final FieldType field = computeFieldValue()
}

private static FieldType getField() { return FieldHolder.field; }
```

この場合、`getField()` メソッドで `FieldHolder` 内部クラスの `field` フィールドが参照された時点でクラスが初期化される。しかもその間 `field` フィールドへのアクセスは同期される。したがって `getFileld()` メソッド自体は同期されずに済む。  
インスタンスフィールドに関しては、フィールドに`volatile` 修飾子をつけ、二重チェックを行うことにより、不必要なロックを避けることができる。　　

```java
private volatile FieldType field;
    
private FieldType getField() {
    FieldType result = field;  // (1)
    if (result != null)
        return result;
    
    synchronized (this) {  
        if (field == null)　　// (2)
            field = computeFieldValue();
        return field;
    }
}
```
`volatile` 修飾子をつけないと field 変数は最初に読み込まれたタイミングで値をローカルにキャッシュしてしまうため、別のスレッドで field の値が変更されてもローカルの値を参照してしまう。したがって元の値を見に行くように `volatile` 修飾子が必要となる。  
`volatile` を付与したために、(1) ~ (2)までの区間で field の値が変更されてしまう可能性がある。したがって `syncronized` でロックを行なったのちに再度 field が初期化されているかどうかを確認している。

[Javaのvolatile修飾子の使い方を現役エンジニアが解説【初心者向け】 | TechAcademyマガジン](https://techacademy.jp/magazine/22668)  
[遅延初期化には気をつけろ - かとじゅんの技術日誌](https://blog.j5ik2o.me/entry/20090816/1250400815)  

## 問題1
遅延初期化とは何か答えよ

<details>
<summary>解答1</summary>

遅延初期化とは、クラスフィールドが参照されたタイミングで初期化を行うことである。

Effective Java 項目83 より
</details>

## 問題2

以下は static なフィールドの遅延初期化を実現する遅延初期化ホルダー・クラス・イディオム実装パターンである。このコードについての説明の括弧に当てはまる単語を答えよ。

```java
private static class FieldHolder {
    static final FieldType field = computeFieldValue()
}

private static FieldType getField() { return FieldHolder.field; }
```

この場合、`getField()` メソッドで `FieldHolder` 内部クラスの `field` フィールドが参照された時点でクラスが(1)される。しかもその間 `field` フィールドへのアクセスは(2)される。したがって `getFileld()` メソッド自体は(2)されずに済む。  

<details>
<summary>解答2</summary>

1. 初期化  
2. 同期

Effective Java 項目83 より
</details>

## 問題3

以下はインスタンスフィールドの遅延初期化の実装パターンの一つである。後の説明の括弧に当てはまる単語を述べよ。

```java
private volatile FieldType field;
    
private FieldType getField() {
    FieldType result = field;  // A
    if (result != null)
        return result;
    
    synchronized (this) {  
        if (field == null)　　// B
            field = computeFieldValue();
        return field;
    }
}
```
`volatile` 修飾子をつけないと field 変数は最初に読み込まれたタイミングで値を(1)してしまうため、別のスレッドで field の値が変更されても値の変更を(2)できない。したがって元の値を見に行くように `volatile` 修飾子が必要となる。  
`volatile` を付与したために、A ~ B までの区間で field の値が(3)されてしまう可能性がある。したがって `syncronized` でロックを行なったのちに再度 field が(4)されているかどうかを確認している。

<details>
<summary>解答3</summary>

1. キャッシュ  
2. 検知  
3. 変更  
4. 初期化

Effective Java 項目83 より
</details>

# 項目84 スレッドスケジューラに依存しない
スレッドスケジューラはどのスレッドをどの程度実行させるかを決定する役割を担っているが、適切なオペレーティングシステムはその決定を公平に行おうとするが、各OSによってポリシーが異なる場合がある。プログラムは移植性を考慮してそのようなスレッドスケジューラのポリシーに依存しないような設計にすることが望ましい。  
スレッドスケジューラのポリシーに依存しないプログラムを設計するには、生成されるスレッドの生成数を少なくする必要がある。これに関しては、スレッドに対して有益な処理を割り当て、さらなる処理をまたせることである。しかしスレッドに割り当てる処理の単位が小さすぎると、スレッドをディスパッチするコストが増大し、パフォーマンスが低下する恐れがあるので、適切な単位のタスクを割り当てることが重要である。   
また、Thread.yield() メソッドはスレッドが使用しているプロセッサのリソースを他のスレッドに譲渡することを示すメソッドであるが、このプロセッサのリソースが他のスレッドに割り当てられるかはJVMによるので、使用すべきではない。  

【参考文献】  
[Web開発のためのJava入門 10章：スレッド : 富士通アプリケーションズ](https://www.fujitsu.com/jp/group/fap/services/java-education/course/technology/java-cobol/10-thread/)  
[Thread (Java Platform SE 8 )](https://docs.oracle.com/javase/jp/8/docs/api/java/lang/Thread.html)

## 問題

以下はプログラムのスレッドに関して述べた文章である。括弧に当てはまる単語を答えよ。

(1)はどのスレッドをどの程度実行させるかを決定する役割を担っている。適切な OS はその決定を公平に行おうとするが、各OSによってポリシーが異なる場合がある。プログラムは移植性を考慮してそのような(1)のポリシーに(2)しないような設計にすることが望ましい。  
(1)のポリシーに(2)しないプログラムを設計するには、生成されるスレッドの(3)を少なくする必要がある。これに関しては、スレッドに対して有益な処理を割り当て、さらなる処理をまたせるという設計が有効である。しかしスレッドに割り当てる処理の単位が小さすぎると、スレッドを(4)するコストが増大し、パフォーマンスが低下する恐れがあるので、適切な単位のタスクを割り当てることが重要である。   
また、Thead クラスの (5)メソッドはスレッドが使用しているプロセッサのリソースを他のスレッドに譲渡することを示すメソッドであるが、このプロセッサのリソースが他のスレッドに割り当てられるかは(6)によるので、使用すべきではない。  

<details>
<summary>解答</summary>

1. スレッドスケジューラ  
2. 依存  
3. 生成数  
4. ディスパッチ  
5. Thread.yield()  
6. JVM  

Effective Java 項目84 より

</details>

# 項目85 Java のシリアライズよりも代替手段を選ぶ

バイト列からインスタンスを生成するデシリアライズは危険が伴う。なぜなら読み込んだデータ何か異常があった場合に、プログラムが正常に動作しなくなるからである。そういった事象を極力避けるために、デシリアライズするデータの構造を限定する。例としては JSON や protobuf (プロトコルバッファ) などがあげられる。

## 問題

以下の文章はオブジェクトのシリアライズ/デシリアライズについて述べたものである。括弧に当てはまる単語を答えよ。

バイト列からインスタンスを生成するデシリアライズは危険が伴う。なぜなら読み込んだデータに何か(1)があった場合に、プログラムが正常に動作しなくなるからである。そういった事象を極力避けるために、デシリアライズするデータの(2)を限定する。例としては (3) や (4) などがあげられる。

<details>
<summary>解答</summary>

1. 異常  
2. 構造  
3. JSON
4. protobuf (プロトコルバッファ)

Effective Java 項目85 より

</details>

