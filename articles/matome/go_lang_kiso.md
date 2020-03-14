<!-- TOC -->

- [Go 言語入門](#go-言語入門)
  - [Hello World 表示プログラム](#hello-world-表示プログラム)
  - [変数の使用](#変数の使用)
  - [基本的なデータ型](#基本的なデータ型)
  - [定数の使用](#定数の使用)
  - [ポインタについて](#ポインタについて)
  - [関数について](#関数について)
- [配列](#配列)
- [スライス](#スライス)
- [map](#map)
- [if](#if)
- [switch](#switch)
- [ループ処理](#ループ処理)
- [range](#range)
- [構造体](#構造体)
- [メソッド](#メソッド)
- [インターフェース](#インターフェース)
- [構造体の判定](#構造体の判定)
- [goroutine](#goroutine)
- [チャネル](#チャネル)
- [Webサーバ作成](#webサーバ作成)

<!-- /TOC -->
# Go 言語入門

## Hello World 表示プログラム

main.go の内容は以下の通り
```go
// Go のコードは必ずいずれかのパッケージに属していなければならない
package main

// 入出力を行うライブラリ
import "fmt"

// main() 関数の内容が実行される
func main() {
	fmt.Println("hello world")
}
```

```sh
go build main.go // コンパイル
go run main.go // コンパイル & 実行
```

## 変数の使用

```go
package main

import "fmt"

func main() {

	// 変数の宣言には var を使用する
	var foo = "hello world"

	// 宣言と代入を一括で行うには := を使用する
	bar := "hello world"
	
	// 複数の変数を宣言することが可能
	a, b := 10, 15

	// 型の異なる変数も一括で宣言することが可能
	var (
		c int
		d string
	)

	c = 22
	d = "hoge"

	fmt.Println(msg)
}
```
変数や関数には命名規則が存在し、先頭の文字が大文字の場合は public, 小文字の場合は private となる。ここでは `fmt.Println(msg)`は public 扱いとなる。

## 基本的なデータ型

```go
package main

import "fmt"

func main() {

	var a string = "aaa" // 初期値は空文字
	var b int = 33       // 初期値は0
	var c float64 = 22.2 // 初期値は0
	var d bool = true    // 初期値はfalse

	fmt.Printf("a: %s, b: %d, c: %f, d: %t \n", a, b, c, d) // %v　とするとよしなに変換してくれる
}
```
後は何もないということを表すのに `nil` を使用する

## 定数の使用

```go
package main

import "fmt"

func main() {

	const foo = "hoge"

	// const には値の再代入は不可能
	foo = "aaaa"

	// 0 始まりの連番を割り振りたいときは iota を使用する
	const (
		bar = iota
		baz
	)

	fmt.Println(bar, baz)
}
```

## ポインタについて

C言語と同じ
```go
package main

import "fmt"

func main() {

	foo := 1
	var p *int

	p = &foo

	fmt.Println(p)
	fmt.Println(*p)
}
```

## 関数について

基本的な記法は以下の通り
```go
package main

import "fmt"

func main() {
	printHi("hoge")

	foo := hi1("fuga")
	fmt.Println(foo)

	bar := hi2("piyo")
	fmt.Println(bar)
}

// void 型は戻り値に何も指定しなくても良い
func printHi(name string) {
	fmt.Println("Hi, " + name + "!!")
}

// 戻り値の型を指定した関数
func hi1(name string) string {
	return "Hi, " + name + "!!"
}

// 戻り値の型を変数名付きで関数
func hi2(name string) (msg string) {
	msg = "Hi, " + name + "!!"
	return // msg を return する
}
```

もう少し詳しく書いていく
```go
package main

import "fmt"

func main() {
	fmt.Println(reverse(2, 3))

	// 関数を変数に代入することも可能
	f := func(foo, bar int) (int, int) {
		return bar, foo
	}

	fmt.Println(f(2, 3))

	// 即時関数のような記法も可能
	func(name string) {
		fmt.Println("Hello, " + name)
	}("Hoge")
}

// 複数の値を返す関数も設定可能
func reverse(foo, bar int) (int, int) {
	return bar, foo
}
```

# 配列

```go
package main

import "fmt"

func main() {
	var foo [5]int
	foo[1] = 1
	foo[3] = 3

	// 配列の長さが自明である場合は [...] で省略可能
	bar := [...]string{"hoge", "fuga"}
	fmt.Println(bar[0])
	fmt.Println(len(bar))
}
```

# スライス

Go言語の配列は C言語と異なり、変数自体は配列のポインタではなくデータそのものであるため、メモリ効率が悪い点がある。C言語における配列のようなデータを扱う型としてスライスが存在する。
```go
package main

import "fmt"

func main() {
	foo := [...]int{2, 3, 5, 7, 11}

	// start:end で配列の要素を取り出してスライスというデータ型で定義
	fmt.Println(foo[2:4])
	fmt.Println(foo[:4])
	fmt.Println(foo[2:])

	// 配列と同じく len が使用可能
	fmt.Println(len(foo[2:]))

	// 最初の位置から切り出しうる最大の要素数
	fmt.Println(cap(foo[2:4]))

}
```

```go
package main

import "fmt"

func main() {

	// スライスを初めから定義する場合は make() を使用する
	// foo := make([]int, 3) // int 型で要素数が3のスライスを生成する

	// [...]の代わりに[]を使用することでスライスを初期化することができる
	bar := []int{2, 3, 5, 7, 11}

	// スライスに要素を追加するときは append() を使用する
	bar = append(bar, 1, 2, 3)
	fmt.Println(bar)

	// スライスのコピーは copy() を使用する
	// コピー元と同じ要素数を確保
	baz := make([]int, len(bar))

	// コピーを行う。なお、戻り値はコピーした要素数となる
	n := copy(baz, bar)
	bar = append(bar, 44, 55, 66) // copy() の後にコピー元のスライスを変更してもコピー先には影響を及ぼさない
	fmt.Println(bar)
	fmt.Println(baz)
	fmt.Println(n)

}
```

# map

```go
package main

import "fmt"

func main() {

	// 初期化
	foo := map[string]int{"a": 1, "b": 2, "c": 3}
	fmt.Println(foo)

	// 削除
	delete(foo, "a")
	fmt.Println(foo)

	// 要素の存在確認 & 値の取得
	isExist, value := foo["b"]
	fmt.Println(value)
	fmt.Println(isExist)

}
```

# if

```go
package main

import "fmt"

func main() {

	if num := -1; num > 0 { // この分岐処理でしか使用しない変数に関しては if の中に含めることが可能
		fmt.Println("A")
	} else if num > -5 {
		fmt.Println("B")
	} else {
		fmt.Println("C")
	}
}
```

# switch

```go
package main

import "fmt"

func main() {

	num := 10

	// 基本的な使用方法
	switch num {
	case 10:
		fmt.Println("hoge")
	default:
		fmt.Println("fuga")
	}

	// if 文っぽくも書ける
	switch {
	case num > 1:
		fmt.Println("piyo")
	case num == 0:
		fmt.Println("hogehoge")
	default:
		fmt.Println("fugafuga")
	}
}
```

# ループ処理

ループ処理は for 文で統一されています。
```go
package main

import (
	"fmt"
)

func main() {

	// 通常の for 文
	for i := 0; i < 10; i++ {
		if i == 3 {
			continue
		}
		if i == 9 {
			break
		}
		fmt.Println("i:", i)
	}

	// 条件付き while 文
	j := 0
	for j < 10 {
		fmt.Println("j:", j)
		j++
	}

	// while(true)
	k := 0
	for {
		fmt.Println("k:", k)
		k++
		if k == 3 {
			break
		}
	}

}
```

# range

```go
package main

import (
	"fmt"
)

func main() {

	arr := []int{1, 2, 3}

	// インデックスと要素を出力する
	for i, e := range arr {
		fmt.Println(i, e)
	}

	// インデックスを使用しない場合
	for _, e := range arr {
		fmt.Println(e)
	}

	// map にも range を使用することが可能
	m := map[string]int{"hoge": 1, "fuga": 2, "piyo": 3}
	for k, v := range m {
		fmt.Println(k, v)
	}
}
```

# 構造体

```go
package main

import (
	"fmt"
)

type person struct {
	name string
	age  int
}

func main() {
	foo := new(person)
	foo.name = "hoge"
	foo.age = 24

	// new を使用するとポインタが返される。
	fmt.Println(foo) // &{hoge 24}
	fmt.Println(*foo)

	// 直接値を設定する以下のような記法では戻り値はポインタとはならない
	bar := person{"fuga", 25}
	fmt.Println(bar)

	baz := person{name: "piyo", age: 26}
	fmt.Println(baz)
}
```

# メソッド

Go はクラスが存在しないので、メソッドを定義する場合は関数にレシーバというものを設定すると構造体からメソッドを使用することが可能となる。メソッドには値渡しと参照渡しの２種類が存在する。
```go
package main

import (
	"fmt"
)

type person struct {
	name string
	age  int
}

// 値渡し　
func (p person) foo() {
	p.age++
}

// 参照渡し
func (p *person) bar() {
	p.age++
}

func main() {
	p := person{"hoge", 23}

	p.foo()
	fmt.Println(p) // 23

	p.bar()
	fmt.Println(p) // 24
}
```

# インターフェース

インターフェースで定義されているメソッドを実装している構造体は同じインターフェースを満たしている型として処理することができる。
```go
package main

import (
	"fmt"
)

type animal interface {
	bark()
}

type dog struct{}
type cat struct{}

func (d dog) bark() {
	fmt.Println("wan, wan!!")
}

func (c cat) bark() {
	fmt.Println("nyaa, nyaa!!")
}

func main() {
	animals := []animal{dog{}, cat{}}

	for _, animal := range animals {
		animal.bark()
	}
}
```

# 構造体の判定

関数の引数に空のインターフェースを指定すると、どんな構造体でも引数として使用することができる。関数内で構造体の型を特定したい場合は、引数のインターフェースを表す変数を t とすると、t.(<任意の構造体>) または t.(type) を使用する。
```go
package main

import (
	"fmt"
)

type animal interface {
	bark()
}

type dog struct{}
type cat struct{}

func (d dog) bark() {
	fmt.Println("wan, wan!!")
}

func (c cat) bark() {
	fmt.Println("nyaa, nyaa!!")
}

func evaluate1(t interface{}) {
	_, isDog := t.(dog)
	if isDog {
		fmt.Println("This type is \"Dog\"")
	} else {
		fmt.Println("This type is not \"Dog\"")
	}
}

func evaluate2(t interface{}) {
	switch t.(type) {
	case dog:
		fmt.Println("This type is \"Dog\"")
	default:
		fmt.Println("This type is not \"Dog\"")
	}
}

func main() {
	animals := []animal{dog{}, cat{}}
	for _, animal := range animals {
		evaluate1(animal)
		evaluate2(animal)
	}

}
```

実行結果
```
This type is "Dog"
This type is "Dog"
This type is not "Dog"
This type is not "Dog"
```

# goroutine

Go では並列処理を実行したい場合は関数を使用するときに前に`go`をつけると実現できる。
```go
package main

import (
	"fmt"
	"time"
)

func hoge() {
	time.Sleep(time.Second * 3)
	fmt.Println("function \"hoge()\" finished !!")
}

func fuga() {
	fmt.Println("function \"fuga()\" finished !!")
}

func main() {
	go hoge()
	go fuga()
	time.Sleep(time.Second * 4)
}
```

実行結果を確認すると、`fuga()`の方が先に完了している。
```
function "fuga()" finished !!
function "hoge()" finished !!
```

# チャネル

goroutine だけでは値を返す関数を並列に実行することは不可能だが、これはチャネルを使用することで実現が可能である。チャネルとはデータの受け渡しを行うデータであり、これを関数の引数に設定してあげることで並列実行を行っても値を返す関数を作成することができる。
```go
package main

import (
	"fmt"
	"time"
)

func hoge(output chan string) { // 関数の引数にチャネルを指定
	fmt.Println("function \"hoge()\" started !!")
	time.Sleep(time.Second * 3)
	output <- "function \"hoge()\" finished !!"
}

func fuga() {
	fmt.Println("function \"fuga()\" finished !!")
}

func main() {

	output := make(chan string) // チャネルはスライスと同じく参照型のデータなので、make で初期化する
	go hoge(output)

	// output に実際に値が入ってくるまで処理は待機する
	fmt.Println(<-output) // チャネルを使用するときは <- を使用する
	go fuga()
	time.Sleep(time.Second * 2)
}
```

実行結果
```
function "hoge()" started !!
function "hoge()" finished !!
function "fuga()" finished !!
```

# Webサーバ作成

リクエストを受け取ってレスポンスを返す Web サーバを実装したい場合は `net/http`パッケージを使用して以下のようにコーディングする。
```go
package main

import (
	"fmt"
	"net/http"
)

// リクエストハンドラー
func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello world!! to [%s]", r.URL.Path[1:])
}

func main() {

	http.HandleFunc("/", handler)
	http.ListenAndServe(":8080", nil)
}
```

実行結果
```
$  curl localhost:8080/
Hello world!! to []
```