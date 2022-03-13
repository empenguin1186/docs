State パターンについて

# この記事について
GoF のデザインパターンの一つである State パターンについて解説してきます。実装は Go です。

# 概要
State パターンは何かしらの「状態」をオブジェクトとして表現し、状態変化をオブジェクトを切り替えることによって表現する際に使用されるパターンです。それぞれの状態によって処理全体の振る舞いを変更したい場合に有用なパターンとなります。

# 実装
実際に実装例を見ていきます。今回は曜日ごとの店の開店時間を変更する処理を考えてみます。まず今回使用するインターフェースについて紹介します。

```go
package state

type DayOfWeekState interface {
	ToString() string
	IsStoreOpen(hour int) bool
}
```
`DayOfWeekState` インターフェースは自身が何曜日かを表現する `ToString()` メソッドと、与えられた時間帯において店が開店しているかどうかを判定する `IsStoreOpen()` メソッドを定義しています。
続けて具体的に上記のインターフェースを実装した構造体を実装していきます。また今回曜日ごとの開店時間は以下のように定義します。

|  曜日  |  開店時間  |
|:--:|:--:|
|  日曜日  |  10 ~ 17  |
|  月曜日  |  閉店(定休日)  |
|  火曜日  |  9 ~ 21  |

また今回は~~実装が面倒くさいので~~世界を改変して 日曜日 -> 月曜日 -> 火曜日 -> 日曜日といったように時間が流れることとします。これを踏まえて実装は以下となります。

```go
package state

type sunday struct{}

func (s *sunday) ToString() string {
	return "Sunday"
}

func (s *sunday) IsStoreOpen(hour int) bool {
	if hour >= 10 && hour <= 17 {
		return true
	}
	return false
}

type monday struct{}

func (m *monday) ToString() string {
	return "Monday"
}

func (m *monday) IsStoreOpen(hour int) bool {
	return false
}

type tuesDay struct{}

func (t *tuesDay) ToString() string {
	return "TuesDay"
}

func (t *tuesDay) IsStoreOpen(hour int) bool {
	if hour >= 9 && hour <= 21 {
		return true
	}
	return false
}
```
次に上記の `State` を管理して毎日の店の開店状況を管理する `context` 構造体を実装します。具体的には以下のようなものとなります。 
```go
package state

var sundaySingleton *sunday = &sunday{}
var mondaySingleton *monday = &monday{}
var tuesdaySingleton *tuesDay = &tuesDay{}

type context struct {
	dayOfWeekStates []DayOfWeekState
	index           int
}

func NewContext() *context {
	return &context{dayOfWeekStates: []DayOfWeekState{sundaySingleton, mondaySingleton, tuesdaySingleton}, index: 0}
}

func (c *context) OneDayPass() {
	if c.index == len(c.dayOfWeekStates)-1 {
		c.index = 0
	} else {
		c.index++
	}
}

func (c *context) GetMessage(hour int) string {
	dayOfWeek := c.dayOfWeekStates[c.index]
	var text string
	if dayOfWeek.IsStoreOpen(hour) {
		text = "is open"
	} else {
		text = "is close"
	}
	return fmt.Sprintf("Today is %s. Store %s.", dayOfWeek.ToString(), text)
}
```
`OneDayPass()` は一日が経過したことを表現するメソッドとなっており、これを呼ぶことにより曜日の切り替えを行います。`GetMessage()` は店の開店状況に応じたメッセージを返すメソッドとなっています。<br>
最後に `main()` を以下のように実装します。
```go
package main

func main() {
	context := state.NewContext()
	hour := 18
	for i := 0; i < 4; i++ {
		fmt.Println(context.GetMessage(hour))
		context.OneDayPass()
	}
}
```
実行すると以下のような内容が標準出力に出力され、各曜日ごとの営業時間に応じたメッセージが出力されていることが確認できるかと思います。
```sh
Today is Sunday. Store is close.
Today is Monday. Store is close.
Today is TuesDay. Store is open.
Today is Sunday. Store is close.
```

# メリット
State パターンを採用せずに同じような処理を実装しようとすると、以下のような感じになるかと思います(雑ですが)。
```go
package main

type DayOfWeek int

const (
	Sunday DayOfWeek = iota
	Monday
	Tuesday
)

func main() {
	hour := 18
	dayOfWeek := Sunday

	for i := 0; i < 4; i++ {
		printStoreIsOpen(dayOfWeek, hour)
		if dayOfWeek == 2 {
			dayOfWeek = 0
		} else {
			dayOfWeek++
		}
	}

}

func printStoreIsOpen(dayOfWeek DayOfWeek, hour int) {
	var dayOfWeekString string
	var openOrClose string
	switch dayOfWeek {
	case Sunday:
		dayOfWeekString = "Sunday"
		if hour >= 10 && hour <= 17 {
			openOrClose = "open"
		} else {
			openOrClose = "close"
		}
		break
	case Monday:
		dayOfWeekString = "Monday"
		openOrClose = "close"
		break
	case Tuesday:
		dayOfWeekString = "Tuesday"
		if hour >= 9 && hour <= 21 {
			openOrClose = "open"
		} else {
			openOrClose = "close"
		}
		break
	default:
		dayOfWeekString = "Undefined"
		openOrClose = "Undefined"
	}
	fmt.Printf("Today is %s. Store %s.\n", dayOfWeekString, openOrClose)
}
```
曜日ごとの分岐や店が開店しているかどうかの分岐が一つの関数で実装されているので、コード自体の複雑性が増しているように見受けられます。一方 State パターンでは同じ処理を実装するのに以下のメソッドを定義しています。
```go
func (c *context) GetMessage(hour int) string {
	dayOfWeek := c.dayOfWeekStates[c.index]
	var text string
	if dayOfWeek.IsStoreOpen(hour) {
		text = "is open"
	} else {
		text = "is close"
	}
	return fmt.Sprintf("Today is %s. Store %s.", dayOfWeek.ToString(), text)
}
```
`GetMessage()` は曜日ごとの分岐は存在しないですし、店が開店しているかどうかの判定に関しても `IsStoreOpen()` を呼び出すだけで個々の曜日ごとの処理はここでは実装されていませんので、先ほどの実装よりかは簡潔にかけているかと思います。個々の曜日ごとの処理は全て `DayOfWeek` を実装している構造体に委ねています。したがって **State パターンを使用する場合は呼び出し側のコードを簡潔に書くことができるのは一つのメリットかと考えています。また、今回の例では呼び出し側が日曜日、月曜日、火曜日などの具体的な曜日の構造体のことを知らなくとも処理を実装できており、コードの結合度自体も低くなっているのもメリットの一つだと思います。**