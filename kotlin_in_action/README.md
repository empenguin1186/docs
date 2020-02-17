<!-- TOC -->

- [Kotlin in Action](#kotlin-in-action)
    - [Kotlin の特徴](#kotlin-の特徴)
        - [静的型付け](#静的型付け)
        - [関数型言語](#関数型言語)
    - [2 章 Kotlin の基本](#2-章-kotlin-の基本)
        - [文と式](#文と式)
            - [問題](#問題)
            - [解答](#解答)
        - [クラスとプロパティ](#クラスとプロパティ)
            - [問題](#問題)
            - [解答](#解答)
        - [カスタムアクセサ](#カスタムアクセサ)
            - [問題](#問題)
            - [解答](#解答)
        - [enum class について](#enum-class-について)
            - [問題1](#問題1)
            - [解答1](#解答1)
            - [問題2](#問題2)
            - [解答2](#解答2)
        - [スマートキャスト](#スマートキャスト)
            - [問題](#問題)
            - [解答](#解答)
        - [while と for のループ](#while-と-for-のループ)
            - [問題1](#問題1)
            - [解答1](#解答1)
            - [問題2](#問題2)
            - [解答2](#解答2)
            - [問題3](#問題3)
            - [解答3](#解答3)
        - [コレクションの要素であるかどうかを確認する](#コレクションの要素であるかどうかを確認する)
            - [問題](#問題)
            - [解答](#解答)
        - [Kotlin における例外処理](#kotlin-における例外処理)
            - [問題](#問題)
            - [解答](#解答)
    - [第3章 関数定義と呼び出し](#第3章-関数定義と呼び出し)
        - [拡張関数](#拡張関数)
            - [問題](#問題)
            - [解答](#解答)
        - [コレクションを扱う](#コレクションを扱う)
        - [正規表現を使用する](#正規表現を使用する)
            - [問題](#問題)
            - [解答](#解答)
        - [コードを整理する: ローカル変数と拡張](#コードを整理する-ローカル変数と拡張)
    - [第4章 クラス、オブジェクト、インターフェイス](#第4章-クラスオブジェクトインターフェイス)
        - [Kotlin のインターフェース](#kotlin-のインターフェース)
            - [問題](#問題)
            - [解答](#解答)
        - [open, final, abstract 修飾子: デフォルトは final](#open-final-abstract-修飾子-デフォルトは-final)
            - [問題](#問題)
            - [解答](#解答)
        - [可視修飾子 : デフォルトは public](#可視修飾子--デフォルトは-public)
        - [内部クラスとネストされたクラス : デフォルトはネストされたクラス](#内部クラスとネストされたクラス--デフォルトはネストされたクラス)
        - [シールドクラス : 限定されたクラス改装を定義する](#シールドクラス--限定されたクラス改装を定義する)
            - [問題](#問題)
            - [問題](#問題)
            - [解答](#解答)
        - [クラスを初期化する : プライマリコンストラクタと初期化ブロック](#クラスを初期化する--プライマリコンストラクタと初期化ブロック)
            - [問題](#問題)
            - [解答](#解答)
        - [セカンダリコンストラクタ : スーパークラスを別のやり方で初期化する](#セカンダリコンストラクタ--スーパークラスを別のやり方で初期化する)
            - [問題](#問題)
            - [解答](#解答)
        - [インターフェース内で宣言されたプロパティを実装する](#インターフェース内で宣言されたプロパティを実装する)
        - [getter と setter からバッキングフィールドにアクセスする](#getter-と-setter-からバッキングフィールドにアクセスする)
            - [問題](#問題)
            - [解答](#解答)
        - [アクセサの可視性を変更する](#アクセサの可視性を変更する)
            - [問題](#問題)
            - [解答](#解答)
        - [コンパイラに生成されるメソッド : データクラスと移譲](#コンパイラに生成されるメソッド--データクラスと移譲)
            - [問題](#問題)
            - [解答](#解答)
        - [クラス移譲 : by キーワードを使う](#クラス移譲--by-キーワードを使う)
            - [問題](#問題)
            - [解答](#解答)
        - [オブジェクト宣言 : 簡単になったシングルトンを簡単に作る](#オブジェクト宣言--簡単になったシングルトンを簡単に作る)
            - [問題](#問題)
            - [解答](#解答)
        - [コンパニオンオブジェクト](#コンパニオンオブジェクト)
            - [問題](#問題)
            - [解答](#解答)
        - [無名クラス: Kotlin による無名内部クラスの置き換え](#無名クラス-kotlin-による無名内部クラスの置き換え)
            - [問題](#問題)
            - [解答](#解答)
    - [第5章 ラムダを使ったプログラミング](#第5章-ラムダを使ったプログラミング)
        - [ラムダ式をとンバ参照](#ラムダ式をとンバ参照)
            - [ラムダの特徴](#ラムダの特徴)
                - [問題](#問題)
                - [解答](#解答)
            - [スコープ内の変数アクセス](#スコープ内の変数アクセス)
            - [メンバ参照](#メンバ参照)
                - [問題](#問題)
                - [解答](#解答)
        - [コレクション操作のための関数型API](#コレクション操作のための関数型api)
            - [必須の関数: filter と map](#必須の関数-filter-と-map)
            - [all, any, count, find : コレクションに述部を適用する](#all-any-count-find--コレクションに述部を適用する)
                - [問題](#問題)
                - [解答](#解答)
            - [groupBy: リストのグループ化](#groupby-リストのグループ化)
                - [問題](#問題)
                - [解答](#解答)
            - [flatMap と flatten : ネストされたコレクション内の要素操作](#flatmap-と-flatten--ネストされたコレクション内の要素操作)
                - [問題](#問題)
                - [解答](#解答)
            - [遅延コレクション操作 : シーケンス](#遅延コレクション操作--シーケンス)
                - [問題](#問題)
                - [解答](#解答)
            - [シーケンス操作の実行 : 中間操作と終端操作](#シーケンス操作の実行--中間操作と終端操作)
                - [問題](#問題)
                - [解答](#解答)
            - [シーケンスの作成](#シーケンスの作成)
        - [Java の関数型インターフェースの使用](#java-の関数型インターフェースの使用)
            - [Java　のメソッドに引数としてラムダを渡す](#java　のメソッドに引数としてラムダを渡す)
        - [レシーバ付きラムダ : with と apply](#レシーバ付きラムダ--with-と-apply)
            - [with 関数](#with-関数)
            - [apply 関数](#apply-関数)
    - [6 章 Kotlin の型システム](#6-章-kotlin-の型システム)
        - [null 許容性の区別](#null-許容性の区別)
            - [null 許容型](#null-許容型)
            - [型の意味](#型の意味)
            - [安全呼び出し演算子 : ?.](#安全呼び出し演算子--)
                - [問題](#問題)
                - [解答](#解答)
            - [エルビス演算子 : ?:](#エルビス演算子--)
                - [問題](#問題)
                - [解答](#解答)
            - [安全キャスト : as?](#安全キャスト--as)
                - [問題](#問題)
                - [解答](#解答)
            - [非 null 表明 : !!](#非-null-表明--)
                - [問題](#問題)
                - [解答](#解答)
            - [let 関数](#let-関数)
                - [問題](#問題)
                - [解答](#解答)

<!-- /TOC -->

# Kotlin in Action

## Kotlin の特徴

### 静的型付け  
使用できる型が限定されるので、その分メソッド呼び出しのパフォーマンスが向上し、さらに信頼性が高くなる。また、コードの把握にも役立つ

### 関数型言語
Kotlin の特徴は以下の通り。  
- 第一級関数：関数を値として扱うことができる。関数の一部の処理を変更したい場合に役立つ  
- イミュータビリティ：イミュータブルなオブジェクトを扱う。オブジェクトは生成後は変更できない。  
- 副作用なし：同じ値が入力されたら同じ値が返却され、他のオブジェクトには影響を及ぼさない
- 関数型のコードは安全なマルチスレッドでテストが簡単。

## 2章 Kotlin の基本

### 文と式
Kotlin において if は文ではなく式である。したがって、ダイレクトに関数の戻り値に使用することが可能である。

```kt
fun main() {
    println(max(1,2))
}

fun max(a: Int, b: Int) = if (a > b) a else b
```

#### 問題

以下の関数を return を使用せずにかきかえよ

```kt
fun max(a: Int, b: int) {
    return if (a > b) a else b
}
```

#### 解答
```kt
fun main() {
    println(max(1,2))
}

fun max(a: Int, b: Int) = if (a > b) a else b
```

### クラスとプロパティ

Kotlin では、GetterやSetterはそのプロパティのアクセス修飾子によって自動生成される。`val`の場合はGetterのみ、`var`の場合は Getter と Setter が自動生成される。
```kt
class Person(val name: String, var isMarried: Boolean)

fun main() {
    val person = Person("Bob", true)
    println(person.name)
}
```

#### 問題

以下のクラスにおいて自動生成される Getter/Seterは何か
```kt
class Person(val name: String, var isMarried: Boolean)
```

#### 解答
Getter ... name, isMarried  
Setter ... isMarried  
val はGetterのみ、var はGetter/Setter の両方が生成される。

### カスタムアクセサ

クラスのプロパティに関しては任意の処理を行うGetter を以下のように定義することができる。
```kt
class Rectangle (val height: Int, val width: Int) {
    val isSquare: Boolean
    	get() {
            return height == width
        }
}
```

#### 問題
カスタムアクセサの実装方法について以下の空欄に当てはまるものを答えよ。

```kt
class Rectangle (val height: Int, val width: Int) {
    val isSquare: Boolean
    	(1) {
            return height == width
        }
}
```

#### 解答
(1) get()  
ちなみに実際にGetterを使用するときは <インスタンス>.isSquare と記述する。


### enum class について

enum の定義は以下の通り
```kt
enum class Collor(val r: Int, val g: Int, val b: Int) {
    RED(255, 0, 0), ORANGE(255, 165, 0), YELLOW(255, 255, 0);
    fun rgb() = (r * 356 + g) * 256 + b
}
```

when を使用して switch 文のような書き方が可能である。また、switch 文と異なり、複数の条件を指定することが可能である。
```kt
fun getMnemonnic(collor: Collor) = 
    	when(collor) {
            Collor.RED ->  "Hoge"
            Collor.ORANGE, Collor.YELLOW -> "Fuga"
        }
```

また Kotlin の when 構文は、 使用がEnum 定数と文字列、数字リテラルに限定される　Java　の switch 文と異なり、任意のオブジェクトで使用すること可能である。
```kt
fun mix(c1: Collor, c2: Collor) =
    when(setOf(c1, c2)) {
        setOf(Collor.RED, Collor.ORANGE) -> Collor.YELLOW
        setOf(Collor.ORANGE, Collor.YELLOW) -> Collor.RED
        setOf(Collor.YELLOW, Collor.RED) -> Collor.ORANGE
        else -> throw Exception("Dirty color")
    }
```
setOf() は、順不同でもその要素を含んでいれば true を返すものである。

#### 問題1
enum class の定義方法を述べよ

#### 解答1
```
enum class Collor(val r: Int, val g: Int, val b: Int) {
    RED(255, 0, 0), ORANGE(255, 165, 0), YELLOW(255, 255, 0);
    fun rgb() = (r * 356 + g) * 256 + b
}
```

#### 問題2
以下のプログラムについて、正しい挙動を述べた文章を選択せよ。
```
fun mix(c1: Collor, c2: Collor) =
    when(setOf(c1, c2)) {
        setOf(Collor.RED, Collor.ORANGE) -> Collor.YELLOW
        setOf(Collor.ORANGE, Collor.YELLOW) -> Collor.RED
        setOf(Collor.YELLOW, Collor.RED) -> Collor.ORANGE
        else -> throw Exception("Dirty color")
    }
```
1. mix(Collor.RED, Collor.ORANGE)　の時のみ Collor.YELLOW が戻る。mix(Collor.ORANGE, Collor.RED) では else に遷移する。  
2. mix(Collor.ORANGE, Collor.RED) でも Collor.YELLOW が戻る。  

#### 解答2
2. が正しい。setOf() は、順不同でもその要素を含んでいれば true を返すものである。

### スマートキャスト

以下のように Interface を実装したクラスを識別して何かしらの処理を行う関数があったとする。
```kt
interface Expr

class Num(val value: Int): Expr

class Sum(val left: Expr, val right: Expr) : Expr

fun eval(e: Expr): Int {
    if (e is Sum) {
        val n = e as Num
        return n.value
    }
    
    if (e is Sum) {
        return eval(e.left) + eval(e.left)
    }
    throw IllegalArgumentException("Unknown expression")
} 
```

この実装では、`is` で入力がそのクラスかどうかを判別したのち、キャストしてからそのクラスのメンバにアクセスしている。ここでキャストはプログラマが手動で行なっている。  
Kotlin ではスマートキャストという機能が提供されており、キャスト処理をコンパイラ側が自動で行なってくれる。
```kt
fun eval(e: Expr): Int = 
	when(e) {
        is Num ->
        	e.value
        is Sum ->
        	eval(e.right) + eval(e.left)
        else ->
        	throw IllegalArgumentException("Unknown expression")
    }
```

#### 問題

以下の実装をスマートキャストを用いた実装に書き換えよ。
```kt
interface Expr

class Num(val value: Int): Expr

class Sum(val left: Expr, val right: Expr) : Expr

fun eval(e: Expr): Int {
    if (e is Sum) {
        val n = e as Num
        return n.value
    }
    
    if (e is Sum) {
        return eval(e.left) + eval(e.left)
    }
    throw IllegalArgumentException("Unknown expression")
} 
```

#### 解答

```kt
fun eval(e: Expr): Int = 
	when(e) {
        is Num ->
        	e.value
        is Sum ->
        	eval(e.right) + eval(e.left)
        else ->
        	throw IllegalArgumentException("Unknown expression")
    }
```

### while と for のループ

while の構文は Java と同じ
```kt
while (condition) {

}
```

do while も以下のように目新しいものはない
```kt
do {

} while (condition) {

}
```

for 文は Kotlin では　range の概念を扱う.

```kt
for (i in 1 .. 10) {

}
```
for 文のインデックスには降順や、偶数のみの指定も可能である。
```kt
for (i in 100 downTo 1 step 2) {

}
```

また、Map の for 文は以下のようにかける
```kt
for ((letter, binary) in binaryReps) { // (key, value) in Map
    println("$letter = $binary")
}
```

コレクションをインデックス付きで for 文を回す時は以下のようにかける
```kt
val list = arrayListOf("10", "11", "12")
for ((index, element) in list.withIndex()) {
    ...
}
```
ここら辺はだいたい他の言語と使用は同じ

#### 問題1
Kotlin の for 文で100 から始まり、1 までの偶数に対して for 文を回す時の for 文の書き方を説明せよ。

#### 解答1

```kt
for (i in 100 downTo 1 step 2) {
    ...
}
```

#### 問題2
Kotlin において Map の for 文の書き方を説明せよ

#### 解答2
```kt
for ((key, value) in map) {
    ...
}
```

#### 問題3
Kotlin においてインデックス付きのコレクション for 文の書き方を説明せよ

#### 解答3

```kt
for ((index, element) in list.withIndex()) {
    ...
}
```

### コレクションの要素であるかどうかを確認する

```kt
fun isLetter(c: Char) = c in 'a' .. 'z' || 'A' .. 'Z'
```
in 演算子は when 句でも使用することができる

#### 問題
入力 Char 変数がアルファベットかどうか判定する関数を in 演算子を用いて実装せよ

#### 解答

```kt
fun isAlphabet(c: Char) = c in 'a' .. 'z' || c in 'A' .. 'Z'
```

### Kotlin における例外処理

Kotlin では、検査例外と非検査例外の区別はつけていない。したがって throws 節は存在しない。例外に関しては処理しても処理しなくても良い。  
また、 Kotlin における try は if や　when と同じく式として扱うので以下のような実装をすることが可能である。  

```kt
fun readNumber(reader: BufferedReader) {
    val number = try {
        Integer.parseInt(readder.readLine())
    } catch (e: NumberFormatException) {
        null
    }

    println(number)
}
```

#### 問題
Kotlin において検査例外と非検査例外の区別はつけられているか

#### 解答
つけられていない

## 第3章 関数定義と呼び出し

###　拡張関数

Kotlin では既存のクラスに新たにメソッドを生やすことができる。以下は String クラスに新しく末尾の文字を取得する `lastChar()` メソッドを実装したものである。

```kt
package strings

fun String.lastChar(): Char = this.get(this.length - 1)

println("Kotlin".lastChar())
```
String.lastChar() の String の部分でレシーバ型を定義し、関数の実装部分の this はこの String を示唆している。また、これはあくまで strings パッケージの中で定義されているメソッドである。また、メンバプロパティに関しても拡張することができる。

#### 問題

文字列の末尾の一文字を取得する戻り値が Char 型の lastChar() メソッドを String クラスの拡張関数として実装せよ。

#### 解答

```kt
fun String.lastChar(): Char = this.get(this.length - 1)
```

### コレクションを扱う

可変長配列を使用する場合は　vararg 修飾子を使用する。

```kt
fun listOf<T>(vararg values: T) List<T> {...}
```

配列の中身を取り出したい場合は、スプレッド演算子を使用する。
```kt
fun main(args: Array<String>) {
    val list = listOf("args: ", *args)
    println(list)
}
```

### 正規表現を使用する

トリプルクォートで囲むと、正規表現をエスケープする必要がなくなる。
```kt
fun parsePath(path: String) {
    val regex = """(.+)/(.+)\.(.+)""".toRegex()
    val matchResult = regex.matchEntire(path)
    if (matchResult != null) {
        val (directory, filename, extension) = matchResult.destructured
        println("Dir: $directory, name: $filename, ext: $extension")
    }
}
```

#### 問題
以下の実装の括弧に当てはまる表現を、正規表現をエスケープしない形式で述べよ。
```kt
fun parsePath(path: String) {
    val regex = "(1).toRegex()
    val matchResult = regex.matchEntire(path)
    if (matchResult != null) {
        val (directory, filename, extension) = matchResult.destructured
        println("Dir: $directory, name: $filename, ext: $extension")
    }
}
```

#### 解答
"""(.+)/(.+)\.(.+)"""

### コードを整理する: ローカル変数と拡張

以下のような実装があったとする
```kt
class User(val id: Int, val name: String, val address: String)

fun saveUser(user: User) {
    if (user.name.isEmpty()) {
        throw IllegalArgumentException(
                "Can't save user ${user.id}: empty Name"
        )
    }

    if (user.address.isEmpty()) {
        throw IllegalArgumentException(
                "Can't save user ${user.id}: empty Address"
        )
    }
}
```
条件分岐のロジックが重複しているので、なんとか一つにまとめたい。また、この判定処理は他の箇所では必ずしも必要ではないかもしれないので、クラス内に実装することは避けたい。これらの課題を解決するには、以下のような実装が考えられる。

```kt
class User(val id: Int, val name: String, val address: String)

fun User.validateBeforeSave() {
    fun validate(value: String, fieldName: String) {
        if (value.isEmpty()) {
            throw IllegalArgumentException(
                    "Can't save user $id: empty $fieldName"
            )
        }
    }
    validate(name, "Name")
    validate(address, "Address")
}

fun saveUser(user: User) {
    user.validateBeforeSave()

}
```
拡張関数ではクラスのメンバにアクセスすることができるので上記のような実装を実現することができる。

## 第4章 クラス、オブジェクト、インターフェイス

### Kotlin のインターフェース
インターフェースはデフォルトの実装を持つことが可能。  
実装クラスがメソッドを実装する場合には必ず Override 修飾子をつける。  

#### 問題

以下の文章はKotlin におけるインターフェースに関するものである。括弧に当てはまる文章を述べよ。　　
インターフェースは(1)の実装を持つことが可能。  
実装クラスがメソッドを実装する場合には必ず(2)修飾子をつける。  

#### 解答
1. デフォルト  
2. override


### open, final, abstract 修飾子: デフォルトは final

以下のようなコードがあったとする。
```kt
interface Clickable {
    fun click()
    fun showOff() = println("I'm clickable!")
}

open class RichButton : Clickable {
    fun disable() {}

    open fun animate() {}

    override fun click() {}
}
```
RichButton は Clickable を実装している。どのメソッドがサブクラスでオーバーライドできるか否かは以下の通り。

|  メソッド  |  説明  |
|:--:|:---|
|  disable()  |  open 修飾子が付与されていないので、サブクラスではオーバーライドできない  |
|  animate()  |  open 修飾子が付与されているので、サブクラスではオーバーライドできる  |
|  click()  |  インターフェースのメンバをオーバーライドしているため、サブクラスでもオーバーライドできる(インターフェースのメンバはデフォルトで open )  |

#### 問題

以下の文章は Kotlin における open, final 修飾子について述べたものである。括弧に当てはまる単語を答えよ。

(1) 修飾子が付与されたメンバは、サブクラスでオーバーライド可能である。また、インターフェースのメンバはデフォルトで (1) 修飾子が付与される。サブクラスにメソッドをオーバーライドさせたくない場合は (2) 修飾子を付与する。

#### 解答

1. open  
2. final


### 可視修飾子 : デフォルトは public

Kotlin では何も修飾子が付与されていないと、デフォルトで public となる。  
またモジュール内で参照可能な internal という修飾子が追加された。

### 内部クラスとネストされたクラス : デフォルトはネストされたクラス
Java では内部クラスは暗黙的に外部クラスの参照を保持していたが、Kotlin では明示的に宣言しない限り、外部クラスの参照を持たない。

### シールドクラス : 限定されたクラス改装を定義する

when を使用するときにデフォルトの処理を実装するケースが存在するが、sealed クラスを定義すると、その内部クラスに対しての処理を実装するだけでよくなり、デフォルトの処理を実装する必要がなくなる。

```kt
sealed class Expr {
    class Num(val value: Int) : Expr()
    class Sum(val left: Expr, val right: Expr) : Expr()
}

fun eval(e: Expr): Int =
        when(e) {
            is Expr.Num -> e.value
            is Expr.Sum -> eval(e.right) + eval(e.left)
        }
```
#### 問題
以下は Kotlin における sealed class の実装である。括弧に当てはまる単語を答えよ。

```kt
(1) class Expr {
    (2) Num(val value: Int) : Expr()
    (2) Sum(val left: Expr, val right: Expr) : Expr()
}

fun eval(e: Expr): Int =
        when(e) {
            is Expr.Num -> e.value
            is Expr.Sum -> eval(e.right) + eval(e.left)
        }
```
また、when の処理では(3)のクラスに対する処理を実装していないと、コンパイルエラーが発生する

#### 解答
1. sealed  
2. class  
3. 全て

### クラスを初期化する : プライマリコンストラクタと初期化ブロック

```kt
// 引数のないデフォルトコンストラクタを定義する場合
open class User

// 引数のないデフォルトコンストラクタのみもつクラスを継承する場合はこんな感じ
public class TwitterUser(val name: String): User()

// private のコンストラクタを宣言する場合
class Secretive private constructor() {}

// 愚直にコンストラクタを定義する場合
class User constructor(_nickname: String) {
    val nickname: String

    init {
        nickname = _nickname
    }
}
```

#### 問題
コンストラクタが呼び出された場合の初期化処理を行うにはどうしたら良いか。

#### 解答
init メソッドを実装する

### セカンダリコンストラクタ : スーパークラスを別のやり方で初期化する

複数のコンストラクタを実装する場合はセカンダリコンストラクタを使用する
```kt
open class Hoge {
    constructor(ctx: Context) {
        ...
    }
    constructor(ctx: Context, attr: AttributeSet) {
        ...
    } 
}
```
また、コンストラクタから他のコンストラクタを呼び出すことも可能。その場合は `this` を使用する。
```kt
class MyButton : View {
    constructor(ctx: Context): this(ctx, MY_STYLE) {
        ...
    }
    constructor(ctx: Context, attr: AttributeSet): super(ctx, attr) {
        ...
    }
}
```

#### 問題
Kotlin においてセカンダリコンストラクタを宣言するにはどうしたら良いか

#### 解答
constructor(fieldValiable: fieldName) { ... } を実装する

### インターフェース内で宣言されたプロパティを実装する

```kt
interface User {
    val nickname: String
}

class PrivateUser(override val nickname: String): User

class SubscribingUser(val email: String): User {
    override val nickname: String
        get() = email.substringBefore('@')
}

class FaceBookUser(val accoutId: Int): User {
    override val nicknmae: String
        get() = getFaceBookName(accoutId)
}
```
PrivateUser はそのまま nickname をコンストラクタの引数に設定したクラス、SubscribingUser は nickname が参照されるたびに @ よりも前の文字を返す処理を行うクラス、FaceBookUser は nickname を FaceBookAPI　から取得して初期化するクラスである。

### getter と setter からバッキングフィールドにアクセスする

```kt
class User(val name: String) {
    var address: String = "unspecified"
        set(value: String) {
            println("""Address was changed for $name: "$field" -> "$value".""".trimIndent())
            field = value
        } 
}
val user = User("Alice")
user.address = "新宿"
```
$field 識別子によってバッキングフィールドの値にアクセスすることができる

#### 問題
Kotlin で以下のようなスクリプトを実行した場合、どのような出力がされるか

```kt
class User(val name: String) {
    var address: String = "unspecified"
        set(value: String) {
            println("""Address was changed for $name: "$field" -> "$value".""".trimIndent())
            field = value
        } 
}
val user = User("Alice")
user.address = "新宿"
```

#### 解答
Address was changed for Alicew: "unspecified" -> "新宿"


### アクセサの可視性を変更する

以下のように実装するとクラス内でのみ counter の値を変更することができる。
```kt
class LengthCounter {
    var counter: Int = 0
        private set
    fun addWord(word: String) {
        counter += word.length
    }
}
```

#### 問題
Kotlin でクラス内のみでセッターを使用したい場合はどのようにすればいいか

#### 解答
```kt
class Hoge {
    var counter: Int
        private set
    ...
}
```

### コンパイラに生成されるメソッド : データクラスと移譲

全てのクラスがオーバーライド可能な以下のメソッドが存在する。  

- toString(): String  
- equals(other: Any?): Boolean  
- hashCode()  

データを表現するクラスに関しては、これらのメソッドを実装するのが望ましいとされている。しかし、クラスをデータクラスとして定義することにより、コンパイラが自動的に生成してくれる。また、データクラスは同時に copy() メソッドも実装しており、インスタンスのコピーを生成することができる。  

#### 問題
Kotlin のクラスは (1) メソッドなど共通でいくつかのメソッドがオーバーライド可能である。この場合データクラスの操作を行うには適切にこれらのメソッドをオーバーライドしなければならない。しかし、クラスを (2) として定義することにより、これらのメソッドをコンパイラが自動的に生成してくれる。また、データクラスは同時に (3) メソッドも実装しており、インスタンスのコピーを生成することができる。 

#### 解答
1. toString()  
2. data class  
3. copy


### クラス移譲 : by キーワードを使う

```kt
class CountingSet<T>(
        private val innerSet: MutableCollection<T> = HashSet<T>()
): MutableCollection<T> by innerSet {
    var objectAdded = 0

    override fun add(element: T): Boolean {
        objectAdded++
        return innerSet.add(element)
    }

    override fun addAll(elements: Collection<T>): Boolean {
        objectAdded += elements.size
        return innerSet.addAll(elements)
    }
}
```
拡張が許されていないクラスを拡張する方法として、デコレーターパターンを用いた方法が存在するが、実装するメソッドを逐一その移譲するクラスのメソッドに置き換えるのは手間がかかる作業となる。しかし、by キーワードを使用すると、拡張したいメソッドだけを変更して、変更を行わないメソッドの記述を省略することができる。新たに実装したい場合は override を付与して実装を行う。

#### 問題
以下の文章は Kotlin におけるクラス移譲に関する文章である。括弧に当てはまる単語を答えよ。  

拡張が許されていないクラスを拡張する方法として、(1)を用いた方法が存在するが、実装するメソッドを逐一その(2)するクラスのメソッドに置き換えるのは手間がかかる作業となる。しかし、(3) キーワードを使用すると、拡張したいメソッドだけを変更して、変更を行わないメソッドの記述を省略することができる。新たに実装したい場合は (4) を付与して実装を行う。以下は (2) を用いたコーディングの例である。  

```kt
class CountingSet<T>(
        private val innerSet: (5) = HashSet<T>()
): MutableCollection<T> (3) innerSet {
    var objectAdded = 0

    override fun add(element: T): Boolean {
        objectAdded++
        return innerSet.add(element)
    }

    override fun addAll(elements: Collection<T>): Boolean {
        objectAdded += elements.size
        return innerSet.addAll(elements)
    }
}
```

#### 解答
1. デコレーターパターン  
2. 移譲  
3. by  
4. override  
5. MutableColldection<T>

### オブジェクト宣言 : 簡単になったシングルトンを簡単に作る
object 修飾子を用いてクラスの宣言とシングルトンインスタンスの生成を同時に行う。

```kt
object Payroll {
    val allEmployees = arrayListOf<String>()
    
    fun calculateSalary() {
        for (e in allEmployees) {
            ...
        }
    }
}
```
また、オブジェクト宣言を行なったクラスに関してもクラスやインターフェースの継承を行うことができる。  

#### 問題
Kotlin においてシングルトンパターンを実装するにはどのようにしたらよいか。

#### 解答
class ではなく object 修飾子を用いる。

### コンパニオンオブジェクト

Kotlin には static 修飾子が存在しないが、companion オブジェクトを使用して static メソッドを実装することができる。以下は companion オブジェクトでファクトリメソッドを実装した例である。
```kt
class User private constructor(val nickname: String) {
    companion object {
        fun newSubscribingUser(email: String) = User(email.substringBefore('@'))
    }
}

val subscribingUser = User.newSubscribingUser("hoge@yahoo.co.jp")
```

#### 問題
以下のコードは Kotlin における Java の static メソッドを実装した例である。括弧に当てはまる単語を述べよ。  

```kt
class User private constructor(val nickname: String) {
    (1) object {
        fun newSubscribingUser(email: String) = User(email.substringBefore('@'))
    }
}
```

#### 解答
1. companion

### 無名クラス: Kotlin による無名内部クラスの置き換え

object 修飾子は Java における無名クラスの機能をもつ
```
val window = Window(Frame())
window.addMouseListener(
        object : MouseAdapter() {
            override fun mouseClicked(e: MouseEvent?) {
                super.mouseClicked(e)
            }

            override fun mouseEntered(e: MouseEvent?) {
                super.mouseEntered(e)
            }
        }
)
```
この場合無名オブジェクトはシングルトンではなく、宣言時に新たに生成される。また、無名オブジェクトを使用した関数で宣言されている変数は、無名オブジェクトの実装からでも使用することができる。

#### 問題
以下のコードは無名クラスを用いた実装例である。括弧に当てはまる単語を述べよ。

```kt
val window = Window(Frame())
window.addMouseListener(
        (1) : MouseAdapter() {
            override fun mouseClicked(e: MouseEvent?) {
                super.mouseClicked(e)
            }

            override fun mouseEntered(e: MouseEvent?) {
                super.mouseEntered(e)
            }
        }
)
```

#### 解答
1. object

## 第5章 ラムダを使ったプログラミング

### ラムダ式をとンバ参照

#### ラムダの特徴
ラムダ式には以下の特性がある  
1. ラムダ式は全て波括弧{}の中で定義する  
2. 関数の引数に使用することができる  
3. ラムダ式が関数の引数の最後の引数であった場合、括弧の外に出すことができる  

```kt
function(a, b, {...}) => function(a, b){}
```
4. ラムダ式が唯一の関数の引数である場合、関数の括弧は省略できる。  
```kt
function() {...} => function {...}
```
5. ラムダがただ一つの引数をとる場合にはデフォルトの引数名である `it` を使用することができる。  

##### 問題
以下の文章はラムダ式の特性について述べた文章である。括弧に当てはまる単語を述べよ。  

1. ラムダ式は全て(1)の中で定義する  
2. 関数の(2)に使用することができる  
3. ラムダ式が関数の引数の(3)であった場合、括弧の外に出すことができる  

```kt
function(a, b, {...}) => function(a, b){}
```
4. ラムダ式が(4)の関数の引数である場合、関数の括弧は省略できる。  
```kt
function() {...} => function {...}
```
5. ラムダがただ一つの引数をとる場合にはデフォルトの引数名である (5) を使用することができる。

##### 解答
1. 波括弧{}  
2. 引数  
3. 最後の引数  
4. 唯一  
5. it

#### スコープ内の変数アクセス

Kotlin ではラムダの外の変数に対して値の変更を行うことができる。
```kt
fun countStatusCode(messages: Collection<String>) {
    var countClientError = 0
    var countServerError = 0
    messages.forEach {
        if (it.startsWith("4")) {
            countClientError++
        } else if (it.startsWith("5")) {
            countServerError++
        }
    }
    println("$countClientError client errors, $countServerError server errors")
}

countStatusCode(arrayListOf("500 internal server error", "400 client error"))
```

#### メンバ参照

以下の書き方は同じ挙動をとる
```kt
class Person(val name: String, val age: Int) {}

// ラムダを使用した方法
people.maxBy {it.age}

// メンバ参照を使用した方法
people.maxBy(Person::age)
```

メンバ参照はトップレベルの関数やコンストラクタも参照することができる(例: ::function, ::constructor)

##### 問題

以下のようなコードがあったとする。(1)に関してメンバ参照を使用した方法に置きかえよ

```kt
class Person(val name: String, val age: Int) {}
val people = listOf(Person("Alice", 29), Person("Bob", 21))
maximumAge = people.maxBy {it.age} // (1)
```

##### 解答

```kt
maximumAge = people.maxBy(Person::age)
```

### コレクション操作のための関数型API

#### 必須の関数: filter と map

filter は特定の条件でコレクションの要素をフィルタリングする関数で、元々の値は変更しない。map はコレクションの要素をあるルールに基づいて別のオブジェクトに変換する関数である。

```kt
var numbers = arrayListOf<Int>(1,2,3,4,5)
numbers.filter { it % 2 == 0 }.forEach(::println)

var result = people.map { it.name }
result.forEach(::println)
```

#### all, any, count, find : コレクションに述部を適用する

|  操作  |  説明  |
|:--:|:---|
|  all  |  全ての要素が与えられた条件を満たしているかどうか  |
|  any  |  どれか一つの要素でも条件を満たしているかどうか  |
|  count  |  条件を満たしている要素の数を返す  |
|  find  |  条件を満たしている要素の数を返す  |

count と似たような操作で size というものがあるが、これは count と違い条件を満たす中間コレクションを保持するので、ただ条件を満たしている要素数を取得したい場合は効率が悪い。  
find は条件を満たした最初の要素を返し、条件を満たす要素が存在しない場合は null を返す。 同意語としては firstOrNull が存在する。  

##### 問題
以下の文章はコレクションに述部を適用した実装方法のについてのものである。括弧に当てはまる単語を述べよ。  

コレクションに適用できる述部メソッドは、(1),(2),(3),(4) が存在する (3) と似たような操作で size というものがあるが、これは (3) と違い条件を満たす中間コレクションを保持するので、ただ条件を満たしている要素数を取得したい場合は効率が悪い。  
(4) は条件を満たした最初の要素を返し、条件を満たす要素が存在しない場合は null を返す。 同意語としては firstOrNull が存在する。  

##### 解答
1. all  
2. any  
3. count  
4. find  

#### groupBy: リストのグループ化

同じような属性をもつインスタンスを一つのグループにまとめることができる。
```kt
class Person(val name: String, val age: Int) {}

val people = listOf(Person("Alice", 29), Person("Bob", 21), Person("Tom", 29))

val group = people.groupBy(Person::age)
```
この場合 group は age をキーとしたマップとなる。

##### 問題

Age が同じ値をとるインスタンスをグルーピングするために、括弧に当てはまる単語を答えよ。
```kt
class Person(val name: String, val age: Int) {}

val people = listOf(Person("Alice", 29), Person("Bob", 21), Person("Tom", 29))

val group = people.(1)
```

##### 解答
groupBy(Person::age)

#### flatMap と flatten : ネストされたコレクション内の要素操作

flatMap はコレクションの各要素を map 処理で特定の操作を行なったのち、その結果を結合する。
```kt
val strings = listOf("abc", "def")
println(strings.flatMap { it.toList() })

class Book(val name: String, val authors: List<String>)

val books = listOf(Book("Tom", listOf("foo", "bar")), Book("Bob", listOf("char", "foofoo")))
println(books.flatMap { it.authors.toSet() })
```

##### 問題

以下のような文字列のコレクションが存在した時に、一文字一文字のコレクションに変換するためにはどういった処理を実装すれば良いか答えよ

```kt
val strings = listOf("abc", "def")
```

##### 解答

```kt
strings.flatMap { it.toList() }
```

#### 遅延コレクション操作 : シーケンス

今までは map や filter を使用してコレクションを操作する方法を述べてきたが、大量の要素を含むコレクションに対してこれらの API を適用すると、中間操作で生成されたオブジェクトの影響で処理効率が低下する恐れがある。したがってこれを回避するために、Sequence インターフェースを使用する。Sequence インターフェースの特徴は以下の通り

- <コレクション>.asSequence() でシーケンスに変換することが可能。  
- Sequence インターフェース はコレクションと同じ API をサポートしている。  
- シーケンスからコレクションへの変換は <シーケンス>.toList() で行う。  
- シーケンスはインデックスを持たないのでインデックスを用いた操作を行う場合はコレクションに変換する必要がある。  

##### 問題

Kotlin におけるシーケンスについて述べた文章である。括弧に当てはまる単語を答えよ

- シーケンスはコレクションと違い map や filter API などの操作結果を保持する(1)を生成しないので、処理効率が良い。
- <コレクション>.(2) でシーケンスに変換することが可能。  
- Sequence インターフェース はコレクションと同じ (3) をサポートしている。  
- シーケンスからコレクションへの変換は <シーケンス>.(4) で行う。  
- シーケンスは(5)を持たないので(5)を用いた操作を行う場合はコレクションに変換する必要がある。  

##### 解答
1. オブジェクト  
2. asSequence()  
3. API  
4. toList()  
5. インデックス  

#### シーケンス操作の実行 : 中間操作と終端操作
シーケンスとコレクションにおける処理フローの違いは、コレクションは各要素の中間操作が完了したのちに終端操作が行われるのに対し、シーケンスは各要素に対して中間操作と終端操作を一気に行う。find などを使用する場合はシーケンスの方が処理効率が良い。

##### 問題
シーケンスとコレクションにおける中間操作と終端操作の処理手順の違いを答えよ。

##### 解答
シーケンスとコレクションにおける処理フローの違いは、コレクションは各要素の中間操作が完了したのちに終端操作が行われる(先行操作)のに対し、シーケンスは各要素に対して中間操作と終端操作を一気に行う(遅延操作)。find などを使用する場合はシーケンスの方が処理効率が良い。

#### シーケンスの作成

### Java の関数型インターフェースの使用

#### Java　のメソッドに引数としてラムダを渡す

Java のメソッドを使用するにおいて、object を使用した無名クラスとラムダの違いは、ラムダはメソッドの引数に変更がない場合は同じインスタンスを使いまわす。しかし、ラムダのスコープの外の変数を使用する場合は新たにインスタンスが生成される。

### レシーバ付きラムダ : with と apply

#### with 関数
with 関数は2つの引数をもつ。最初の引数はラムダで使用するメソッドを保持しているクラスで、2つ目の引数はラムダである。ラムダは括弧の外に出すことができるので、以下のような使い方となる。  

```kt
with(<ラムダにメソッドを提供するクラス>) { this.<メソッド> }
```

#### apply 関数
apply 関数は with 関数と同じような処理を行うが、戻り値は常にレシーバオブジェクトとなる。これはコンストラクタでインスタンスを生成したのちに何かしらの初期化を複数行うような処理を実装する場合に用いられる。

```kt
<戻り値に指定するクラス>.apply { 処理 }
```
StringBuilder で文字列を追加して最後に toString で String に変換するということがよくあるが、それは buildString メソッドを使用すると簡略化できる。

```kt
fun alphabet() = buildString {
    for (letter in 'A' .. 'Z') {
        append(letter)
    }
    append("\nNow I know the alphabet!")
}
```

## 6章 Kotlin の型システム

### null 許容性の区別

#### null 許容型
Kotlin には null 許容型と非許容型が存在し、この二つは完全に別の型となる。

#### 型の意味
Java の型は不完全な要素を有している。例えば、String クラスは文字列と null を代入することができるが、この二つに提供できる API は異なる。また、 以下の2つの実行結果が異なることから、null はString 型ではないことがわかる。

```java
"aaa" instanceof String // true
null instanceof String // false
```

#### 安全呼び出し演算子 : ?.

null 値が入るメソッドやクラスフィールドを参照する場合、Java では null チェックを行なったのちに null でなければメソッドの実行結果やフィールドの値を返し、null であった場合は null を返すような処理を行なっていたが、Kotlin では安全呼び出し演算子(?.)を用いることでこれを簡単に実現できる。

```kt
fun printAllCaps(s: String?) {
    val allCaps: String? = s?.toUpperCase()
    println(allCaps)
}

printAllCaps("abc") // ABC
printAllCaps(null) // null
```
安全呼び出し演算子の戻り値はこの場合 String? 型になることは注意したい。また、この安全呼び出し演算子はインスタンスのフィールドなどに適用する場合は、繰り返し使用することが可能である。

```kt
val hoge = foo?.bar?.piyo
```

##### 問題

以下の処理を安全呼び出し演算子を用いて簡略化せよ

```kt
fun printAllCaps(s: String?) {
    val allCaps = if (s != null) s.toUpperCase() else null
    println(allCaps)
}
```

##### 解答

```kt
fun printAllCaps(s: String?) {
    val allCaps: String? = s?.toUpperCase()
    println(allCaps)
}
```

#### エルビス演算子 : ?:
エルビス演算子を使用すると、null 許容型を扱う場合、その値が null であった場合にデフォルトの値に置き換えることができる

```kt
fun upperCaseorDefault(s: String?): String {
    val result = s?.toUpperCase() ?: "hoge"
    return result
}

println(upperCaseorDefault("hoge")) // HOGE
println(upperCaseorDefault(null)) // hoge
```
戻り値は非許容でも許容でもOK

##### 問題
以下の関数において引数の値が null であった場合にデフォルトの文字列を返すようにして、関数の戻り値の型を null 非許容型にしたい。エルビス演算子を用いて実装せよ。

```kt
fun upperCaseorDefault(s: String?): String? {
    val result = s?.toUpperCase()
    return result
}
```

##### 解答

```kt
fun upperCaseorDefault(s: String?): String {
    val result = s?.toUpperCase() ?: "DEFAULT"
    return result
}
```

#### 安全キャスト : as?
as? 演算子は安全にキャストを行うための演算子である。as 演算子では評価するインスタンスが規定の型ではなかった場合には例外を発生させていたが、as? 演算子は null を返す。これはエルビス演算子との親和性が高い。

```kt
class Person(val firstName: String, val lastName: String) {
    override fun equals(other: Any?): Boolean {
        val otherPerson = other as? Person ?: return false
        return otherPerson.firstName == firstName && otherPerson.lastName == lastName
    }
}
```

##### 問題

安全キャストとは何か答えよ。また、安全キャストを用いて equals メソッドを実装せよ

```kt
class Person(val firstName: String, val lastName: String) {
    override fun equals(other: Any?): Boolean {
        val otherPerson = other (1) Person ?: return false
        return otherPerson.firstName == firstName && otherPerson.lastName == lastName
    }
}
```

##### 解答
キャストを行い、対象のインスタンスが規定のクラスにキャストできなかった場合は null を返す演算子のこと。  
1. as?

#### 非 null 表明 : !!
その値には null が入らないことを明示的に示すためには、 !! 演算子を用いる。この演算子が使用される箇所としては、ある関数で null チェックを行い、使用するクライアント側が null 許容型としてその関数の戻り値を宣言した場合にはいささか冗長な実装となってしまう。そこで、null が返ってこないことがわかっているのならば !! を使用してコンパイラの判断の余地を減らすのが良い実装となる。

##### 問題
非 null 表明 (!!) を使用するケースを一つのべよ。

##### 解答
null を返さない関数を使用するクライアント側で戻り値を宣言するときに使用する。

#### let 関数

安全呼び出し演算子とともに用いることで、null 許容型の変数を null チェックして値が null 以外の場合に規定の処理を行うことができるようになる。
```kt
fun toUpperCase(s: String): String {
    return s.toUpperCase()
}

val hoge: String? = "foo"
val fuga: String? = null

hoge?.let { println(toUpperCase(it)) } // FOO が出力
fuga?.let { println(toUpperCase(it)) } // 何も出力されない
```

##### 問題
以下のような関数と変数が存在している場合に let を用いて hoge, fuga に toUppserCase を適用させたものを出力する処理を各変数に対して１行で実装せよ

```kt
fun toUpperCase(s: String): String {
    return s.toUpperCase()
}

val hoge: String? = "foo"
val fuga: String? = null
```

##### 解答

```kt
hoge?.let {println(toUpperCase(it))}
fuga?.let {println(toUpperCase(it))}
```
