<!-- TOC -->

- [Effective Java 練習問題](#effective-java-練習問題)
- [項目1 コンストラクタの代わりに static ファクトリメソッドを検討する](#項目1-コンストラクタの代わりに-static-ファクトリメソッドを検討する)
- [項目3 privateのコンストラクタかenum型でシングルトン特性を強制する](#項目3-privateのコンストラクタかenum型でシングルトン特性を強制する)
  - [問題](#問題)
  - [解答](#解答)
- [項目4 private のコンストラクタでインスタンス化不可能を矯正する](#項目4-private-のコンストラクタでインスタンス化不可能を矯正する)
- [項目5 資源を直接結びつけるよりも依存性注入を選ぶ](#項目5-資源を直接結びつけるよりも依存性注入を選ぶ)
- [項目6 不必要なオブジェクトの生成を避ける](#項目6-不必要なオブジェクトの生成を避ける)
  - [問題](#問題-1)
  - [解答](#解答-1)
  - [問題](#問題-2)
  - [解答](#解答-2)
- [項目7 使われなくなったオブジェクト参照を取り除く](#項目7-使われなくなったオブジェクト参照を取り除く)
  - [問題](#問題-3)
  - [解答](#解答-3)
  - [問題](#問題-4)
  - [解答](#解答-4)
- [項目9 try-finally よりも try-with-resources を選ぶ](#項目9-try-finally-よりも-try-with-resources-を選ぶ)
- [項目12 toString を常にオーバーライドする](#項目12-tostring-を常にオーバーライドする)
- [項目17 可変性を最小限にする](#項目17-可変性を最小限にする)
- [項目18 継承よりもコンポジションを選ぶ](#項目18-継承よりもコンポジションを選ぶ)
  - [解答](#解答-5)
- [項目20 抽象クラスよりもインターフェースを選ぶ](#項目20-抽象クラスよりもインターフェースを選ぶ)
- [項目21 将来のためにインターフェースを設計する](#項目21-将来のためにインターフェースを設計する)
  - [問題](#問題-5)
  - [解答](#解答-6)
- [項目23 タグ付きクラスよりもクラス階層を選ぶ](#項目23-タグ付きクラスよりもクラス階層を選ぶ)
- [項目24 非 static のメンバークラスよりも static のメンバークラスを選ぶ](#項目24-非-static-のメンバークラスよりも-static-のメンバークラスを選ぶ)
- [項目25 ソースファイルを単一のトップレベルのクラスに限定する](#項目25-ソースファイルを単一のトップレベルのクラスに限定する)
  - [問題](#問題-6)
  - [解答](#解答-7)
- [項目27 無検査警告を取り除く](#項目27-無検査警告を取り除く)
  - [問題](#問題-7)
  - [解答](#解答-8)
- [項目28 配列よりもリストを選ぶ](#項目28-配列よりもリストを選ぶ)
  - [解答](#解答-9)
- [項目35 序数の代わりにインスタンスフィールドを使う](#項目35-序数の代わりにインスタンスフィールドを使う)
- [項目36 ビットフィールドの代わりに EnumSet を使う](#項目36-ビットフィールドの代わりに-enumset-を使う)
- [項目40 常に Override アノテーションを使う](#項目40-常に-override-アノテーションを使う)
- [項目41 型を定義するためにマーカーインターフェースを使う](#項目41-型を定義するためにマーカーインターフェースを使う)
- [項目42 無名クラスよりもラムダを選ぶ](#項目42-無名クラスよりもラムダを選ぶ)
- [項目43 ラムダよりもメソッド参照を選ぶ](#項目43-ラムダよりもメソッド参照を選ぶ)
- [項目44 関数型インターフェースを使う](#項目44-関数型インターフェースを使う)
- [項目51 メソッドのシグニチャを注意深く設計する](#項目51-メソッドのシグニチャを注意深く設計する)
- [項目54 null ではなく、空コレクションか空配列を返す。](#項目54-null-ではなく空コレクションか空配列を返す)
- [項目57 ローカル変数のスコープを最小限にする](#項目57-ローカル変数のスコープを最小限にする)
- [項目58 従来のforループよりもfor-each　ループを選ぶ](#項目58-従来のforループよりもfor-each　ループを選ぶ)
- [項目59 ライブラリを知り、ライブラリを使う](#項目59-ライブラリを知りライブラリを使う)
- [項目60 正確な答えが必要ならば、float と double を避ける](#項目60-正確な答えが必要ならばfloat-と-double-を避ける)
- [項目61 ボクシングされた基本データよりも基本データ型を選ぶ](#項目61-ボクシングされた基本データよりも基本データ型を選ぶ)
- [項目62 他の型が適切な場所では、文字列を避ける](#項目62-他の型が適切な場所では文字列を避ける)
  - [解答](#解答-10)
- [項目63 文字列結合のパフォーマンスに用心する](#項目63-文字列結合のパフォーマンスに用心する)
- [項目64 インターフェースでオブジェクトを参照する](#項目64-インターフェースでオブジェクトを参照する)
- [項目65 リフレクションよりもインターフェースを選ぶ](#項目65-リフレクションよりもインターフェースを選ぶ)
- [項目66 ネイティブメソッドを注意して使う](#項目66-ネイティブメソッドを注意して使う)
- [項目71 チェックされる例外を不必要に使うのを避ける](#項目71-チェックされる例外を不必要に使うのを避ける)
- [項目72 標準的な例外を使う](#項目72-標準的な例外を使う)
  - [問題](#問題-8)
  - [解答](#解答-11)
- [項目74 各メソッドがスローするすべての例外を文書化する](#項目74-各メソッドがスローするすべての例外を文書化する)
- [項目75 詳細メッセージにエラー記録情報を含める](#項目75-詳細メッセージにエラー記録情報を含める)
- [項目76 エラーアトミック性に努める](#項目76-エラーアトミック性に努める)
- [項目77 例外を無視しない](#項目77-例外を無視しない)
- [項目78 共有された可変データへのアクセスを同期する](#項目78-共有された可変データへのアクセスを同期する)
- [項目83 遅延初期化を注意して使う](#項目83-遅延初期化を注意して使う)
- [項目84 スレッドスケジューラに依存しない](#項目84-スレッドスケジューラに依存しない)
- [項目85 Java のシリアライズよりも代替手段を選ぶ](#項目85-java-のシリアライズよりも代替手段を選ぶ)

<!-- /TOC -->

# Effective Java 練習問題

# 項目1 コンストラクタの代わりに static ファクトリメソッドを検討する

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

# 項目3 privateのコンストラクタかenum型でシングルトン特性を強制する

## 問題
staticファクトリメソッド、もしくはEnumを使用したシングルトンパターンの実装のメリットをそれぞれ答えよ。

## 解答
staticファクトリメソッド  
1. シングルトンのスコープを変更できる。例えば、このパターンで実装すると、スレッド単位で唯一のインスタンスを保持するような実装に変更したい場合も対応することができる。  
2. ジェネリックのシングルトンファクトリを定義することが可能。  
3. メソッド参照が可能。  

Enum
1. １つ目のパターンと異なり、シリアライズの機能が備わっている。  
2. シリアライズやリフレクションによる複数のインスタンスの生成を防ぐことができる。

# 項目4 private のコンストラクタでインスタンス化不可能を矯正する
ユーティリティクラスとは何か。また、その実装方法を述べよ。

<details>
<summary>解答</summary>

static なメソッドのみで構成される、インスタンス化を不可能にしたクラスのこと。private のコンストラクタを定義して実装する。

</details>

# 項目5 資源を直接結びつけるよりも依存性注入を選ぶ

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

## 問題

生成コストが高く、使いまわしても良いオブジェクトはインスタンスフィールドにするべきか、それともstaticフィールドにすべきか答えよ。

## 解答

staticフィールドにすべき

## 問題

以下のコードの問題点について述べよ。
```java
Long sum = 0L;
for(long i=0, i <= Integer.MAX_VALUE; i++) {
    sum += i;
}
```

## 解答

sum をLong型で定義している点。for文が走るごとにLongインスタンスが生成されるため、パフォーマンスが低下する。基本データ型であるlongを使用した方がいい。

# 項目7 使われなくなったオブジェクト参照を取り除く

## 問題

メモリリークを防ぐために、使われなくなったオブジェクトに対して参照を取り除くにはどうしたらいいか

## 解答

参照にnullを代入する

## 問題

メモリリークが発生しやすい原因を2つ述べよ。そしてその解決策を述べよ。

## 解答

キャッシュとコールバック。どちらも弱い参照(WeakHashMap)を使用することで解決することができる。

# 項目9 try-finally よりも try-with-resources を選ぶ

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

toString() メソッドをオーバライドする動機を述べよ。

<details>
<summary>解答</summary>

デフォルトの toString() メソッドではそのクラスが何を表しているかがわかりにくい。そこでそのクラスを端的に表す文字列を返すことで、デバッグしやすくするため。

</details>


# 項目17 可変性を最小限にする

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
以下の文章は継承とコンポジションにまとめたものである。括弧に当てはまる単語を述べよ  
- 継承を使用すると、(1)でオーバーライドしたメソッドが(2)の実装に依存してしまうので、極力使用は避ける
  - 例) HashSet の addAll は内部で add が呼ばれている
- 継承よりも(3)を用いる方が良い。移譲とは、あるインターフェースを実装する場合に、private フィールドにメソッドの実装を任せることである。
- 継承は、例えばクラスAとBの間に (4) の関係が存在する場合に使用する

## 解答
1. サブクラス  
2. スーパークラス  
3. 移譲  
4. A is a part of B

# 項目20 抽象クラスよりもインターフェースを選ぶ

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

## 問題

既存のインターフェースに新たにメソッドを追加するときに、実装クラスがそのまま使用することを可能にするためにデフォルトメソッドを定義する方法が考えられるが、この方法の問題点を答えよ。  

## 解答

既存のインターフェースに新たにメソッドを追加しなければならなくなった場合、そのインターフェースを実装している全てのクラスはその追加されたメソッドを実装しなければならない。このコストを軽減するために、インターフェースのデフォルト実装を用いる方法があるが、これは好ましくない。なぜなら、そのデフォルト実装は実装クラスの知識を持っておらず、実装クラスでそのデフォルト実装のメソッドを呼び出したときに例外が発生する場合があるからである。

# 項目23 タグ付きクラスよりもクラス階層を選ぶ

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

## 問題

一般的にJavaのソースコードは、一つのソースファイルにつき一つのトップレベルのクラスを定義しているが、その理由を述べよ。

## 解答
一つのソースファイルに複数のトップレベルのクラスを定義するのは、ややこしくなったりコンパイル時に挙動がおかしくなる可能性があるから。もしどうしても複数のトップレベルのクラスを定義する場合は static のメンバークラスとして定義する。

# 項目27 無検査警告を取り除く

## 問題

以下の文章はジェネリクスにおける無検査警告の対応について述べたものである。括弧に当てはまる単語を答えよ。  

ジェネリクスを用いてコーディングを行う場合、無検査警告を無視せず、できる限り全てを取り除くようにする。警告を取り除くことは(1)を発生しないことを意味するからである。もし警告を取り除くことができないのであれば、(2)であることが保証できる箇所にのみ(3)アノテーションを付与して警告を抑制する。この場合変数宣言に対してアノテーションを付与することになるが、できる限り付与するスコープは(4)する。  
例えば、無検査警告が発生するようなパターンとしては以下のようなものがある。  
Set<Lark> exaltation = new Hashset();  
HashSet<>() とすれば取り除くことが可能。

## 解答
1. ClassCastException  
2. 型安全  
3. @SuppressWarning  
4. 小さく

# 項目28 配列よりもリストを選ぶ
- 配列は(1)であるのに対し、リストは(2)であるため、リストを使用した方が良い。(1)とは、例えばクラス A がクラス B のサブタイプだとすると、(3) も (4)のサブタイプとなる特性のことをさす。したがって、以下のコードはコンパイル時にはエラーとならず、実行時に例外が発生する。
```
Object[] objectArray = new Long[1];
objectArray[0] = "I don't fit in.";
```
- また、配列はジェネリクスを使用すると、(5)を出すが、リストは警告を出さないため安全である。

## 解答
1. 共変  
2. 不変  
3. A[]  
4. B[]  
5. 未検査キャスト警告

# 項目35 序数の代わりにインスタンスフィールドを使う

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

以下の文章はビットフィールド表現による集合を使用した処理の欠点を述べたものである。括弧に当てはまる単語を述べよ。  
1. 必要とするビット数によって型を (1)か(2) かを選択しなければならない。  
2. (3)を伴う処理を実装する場合には、使い勝手が悪くなる。  

<details>
<summary>解答</summary>

1. int  
2. long  
3. イテレーション

Effective Java 項目36 より

</details>

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
<summary>解答</summary>

```java
public class Text {
    public enum Style { BOLD, ITALIC, UNDERLINE, STRIKETHROUGH}

    public void applyStyles(Set<Style> styles) { ... }
}
```

Effective Java 項目36 より

</details>

# 項目40 常に Override アノテーションを使う

以下は Override アノテーションに関して述べた文章である。括弧に当てはまる単語を答えよ。  
toString() や equals() などのメソッドをオーバーライドするときは Override アノテーションを付与した方が良い。なぜなら、(1)が間違っていた場合に(2)が発生し、誤った実装であることに気づくことができるからである。しかし、(3)クラスで定義されている(3)メソッドに関してはアノテーションを付与しなくても構わない。  

<details>
<summary>解答</summary>

1 シグニチャ  
2 コンパイルエラー  
3 抽象  
Effective Java 項目40 より

</details>

# 項目41 型を定義するためにマーカーインターフェースを使う

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

</details>



# 項目42 無名クラスよりもラムダを選ぶ

関数型インターフェースとは何か答えよ。

<details>
<summary>解答</summary>
抽象メソッドを一つだけもつインターフェースのこと。  
Effective Java 項目43 より
</details>

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
<summary>解答</summary>
list.sort(Comparator.comparingInt(String::length));  
Effective Java 項目43 より
</details>

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
<summary>解答</summary>

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

以下はメソッドのシグニチャについて述べた文章である。括弧に当てはまる単語を述べよ。  

1 (1)を注意深く選ぶ  
2 便利な(2)を提供しすぎない  
3 長い(3)のリストは避ける。方法は以下。  
- 複数の(2)に分割し、パラメータを減らす  
- パラメータの集まりを保持する(4)を用意する  
- パラメータの一部がオプションである場合は、(5) パターンを採用し、必要なパラメータがセットされた時点で execute() メソッドを実行して状態検証を行い、処理を実行する。  
4 変更容易性のためパラメータに関しては、クラスよりも(6)を選ぶ  
5 変更容易性のため boolean パラメータよりは、2つの要素を持つ (7) を使用する  

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

コレクションや配列を返すメソッドを実装するときに、null を返してはいけない理由を述べよ。

<details>
<summary>解答</summary>

メソッドを使用する側で null チェックを行う必要があり、メソッドが扱いづらいものとなるから。  
Effective Java 項目54 より

</details>

# 項目57 ローカル変数のスコープを最小限にする

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

# 項目58 従来のforループよりもfor-each　ループを選ぶ

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

# 項目59 ライブラリを知り、ライブラリを使う

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

Effective Java 項目59 より

</details>

# 項目60 正確な答えが必要ならば、float と double を避ける

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

以下の文章は String クラスの使用について述べたものである。括弧に当てはまる単語を答えよ。

よりよいデータ型が存在する場合には String でオブジェクトを表現することは避ける。以下が主な理由である。  
1. (1) で定義できる場合には、String よりも (1) を使用した方がバグの発生を避けることができる  
2. String でデータを表現しようとすると、String が提供する(2)を強制されるため、(3)ではない  
3. (4)が求められる場面での String の使用は誤作動の原因となる  

## 解答
1. Enum  
2. 振る舞い  
3. 本質的  
4. 一意性

# 項目63 文字列結合のパフォーマンスに用心する

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

例外別のハンドリングの方針について述べよ。

<details>
<summary>解答</summary>
発生した例外に対して、回復のために何か特別な処理を実装できる例外に関しては try ~ catch を用いてチェックする。それ以外はチェックしない。  
Effective Java 項目71 より
</details>

以下のコードで発生する例外をチェックしない例外に変換するような実装をせよ。

```java
try {
    obj.action(args);
} catch(TheCheckException e) {
    // 例外処理を記述
}
```

<details>
<summary>解答</summary>

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

## 問題

以下のケースに関してスローすべき Excepiton を答えよ

1. null ではないがパラーメータが不適切  
2. メソッド呼び出しに対してオブジェクトの状態が不正  
3. パラメータが禁止されている null  
4. インデックスの値が範囲外  
5. 禁止されているオブジェクトに並行した変更を検出  
6. オブジェクトがメソッドをサポートしていない

## 解答

1. IllegalArgumentException  
2. IllegalStateException  
3. NullPointerException  
4. IndexOutOfBoundsException
5. ConcurrentModificationException  
6. UnsupportedOperatoinException

# 項目74 各メソッドがスローするすべての例外を文書化する

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

エラーアトミックなメソッドとは何か

<details>
<summary>解答</summary>
あるオブジェクトのメソッドを呼び出して例外がスローされた時、整合性を担保するためにそのオブジェクトをメソッドを呼び出す前の状態にリセットされるメソッドのこと  
Effective Java 項目76 より
</details>

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
簡単なので省略。

# 項目78 共有された可変データへのアクセスを同期する

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
<summary>解答</summary>
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

インクリメント演算子 ++ はアトミックな操作かどうかを答えよ。また、一意な数値を採番したい場合に有効な実装を述べよ。

<details>
<summary>解答</summary>

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

遅延初期化とは何か答えよ

<details>
<summary>解答</summary>

遅延初期化とは、クラスフィールドが参照されたタイミングで初期化を行うことである。

Effective Java 項目83 より
</details>

以下は static なフィールドの遅延初期化を実現する遅延初期化ホルダー・クラス・イディオム実装パターンである。このコードについての説明の括弧に当てはまる単語を答えよ。

```java
private static class FieldHolder {
    static final FieldType field = computeFieldValue()
}

private static FieldType getField() { return FieldHolder.field; }
```

この場合、`getField()` メソッドで `FieldHolder` 内部クラスの `field` フィールドが参照された時点でクラスが(1)される。しかもその間 `field` フィールドへのアクセスは(2)される。したがって `getFileld()` メソッド自体は(2)されずに済む。  

<details>
<summary>解答</summary>

1. 初期化  
2. 同期

Effective Java 項目83 より
</details>

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
<summary>解答</summary>

1. キャッシュ  
2. 検知  
3. 変更  
4. 初期化

Effective Java 項目83 より
</details>

# 項目84 スレッドスケジューラに依存しない

以下はプログラムのスレッドに関して述べた文章である。括弧に当てはまる単語を答えよ。

(1)はどのスレッドをどの程度実行させるかを決定する役割を担っている。適切な OS はその決定を公平に行おうとするが、各OSによってポリシーが異なる場合がある。プログラムは移植性を考慮してそのような(1)のポリシーに(2)しないような設計にすることが望ましい。  
(1)のポリシーに(2)しないプログラムを設計するには、生成されるスレッドの(3)を少なくする必要がある。これに関しては、スレッドに対して有益な処理を割り当て、さらなる処理をまたせるという設計が有効である。しかしスレッドに割り当てる処理の単位が小さすぎると、スレッドをディスパッチするコストが増大し、パフォーマンスが低下する恐れがあるので、適切な単位のタスクを割り当てることが重要である。   
また、(4)メソッドはスレッドが使用しているプロセッサのリソースを他のスレッドに譲渡することを示すメソッドであるが、このプロセッサのリソースが他のスレッドに割り当てられるかは(5)によるので、使用すべきではない。  

<details>
<summary>解答</summary>

1. スレッドスケジューラ  
2. 依存  
3. 生成数  
4. Thread.yield()  
5. JVM  

Effective Java 項目84 より

</details>

# 項目85 Java のシリアライズよりも代替手段を選ぶ

以下の文章はオブジェクトのシリアライズ/デシリアライズについて述べたものである。括弧に当てはまる単語を答えよ。

バイト列からインスタンスを生成するデシリアライズは危険が伴う。なぜなら読み込んだデータ何か(1)があった場合に、プログラムが正常に動作しなくなるからである。そういった事象を極力避けるために、デシリアライズするデータの(2)を限定する。例としては (3) や (4) などがあげられる。

<details>
<summary>解答</summary>

1. 異常  
2. 構造  
3. JSON
4. protobuf (プロトコルバッファ)

Effective Java 項目85 より

</details>