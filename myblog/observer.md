Observer パターンについて

# この記事について
GoF のデザインパターンの一つである Observer パターンについて解説してきます。実装は Go です。

# 概要
Observer パターンは観察対象のオブジェクトの状態が更新された場合に、観察者オブジェクトに対して通知を行うような処理を実装する際に使用されるパターンです。したがって状態変化に応じた処理を実装したい場合に有用なパターンとなります。

# 実装
実際に実装例を見ていきます。今回は何かしらメッセージが更新された場合に Slack や Email にメッセージを送信する処理を実装することを考えてみます。まずは基本的なインターフェースの実装を紹介します。

```go
package observer

type Observer interface {
	Notify(generator MessageGenerator)
}

type MessageGenerator interface {
	AddObserver(observer Observer)
	DeleteObserver(observer Observer)
	GetMessage() string
	Execute()
}
```
`Observer` インターフェースは `MessageGenaerator` が更新したメッセージを通知する役割を担います。通知処理の実装はこのインターフェースを実装した構造体で行います。
`MessageGenerator` インターフェースは `Observer` の管理、メッセージの更新、各種 `Observer` への通知の役割を担います。<br>

続けて具体的に上記のインターフェースを実装した構造体を実装していきます。以下のような実装になります。
```go
package observer

type RandomMessageGenerator struct {
	observers []Observer
	message   string
}

func (r *RandomMessageGenerator) AddObserver(observer Observer) {
	r.observers = append(r.observers, observer)
}

func (r *RandomMessageGenerator) DeleteObserver(observer Observer) {
	var observers []Observer
	for _, e := range r.observers {
		if e != observer {
			observers = append(observers, e)
		}
	}
	r.observers = observers
}

func (r *RandomMessageGenerator) GetMessage() string {
	return r.message
}

func (r *RandomMessageGenerator) Execute() {
	r.message = fmt.Sprintf("message changed. number=%d", rand.Intn(100))
	for _, e := range r.observers {
		e.Notify(r)
	}
}

type SlackNotifier struct{}

func (s *SlackNotifier) Notify(generator MessageGenerator) {
	fmt.Printf("send message to slack. message = \"%s\"\n", generator.GetMessage())
}

type EmailNotifier struct{}

func (e *EmailNotifier) Notify(generator MessageGenerator) {
	fmt.Printf("send message to email. message = \"%s\"\n", generator.GetMessage())
}
```

`RandomMessageGenerator` は `MessageGenerator`、`SlackNotifier` および `EmailNotifier` はそれぞれ `Observer` インターフェースを実装しています(今回は通知処理に関してはこの記事の本質ではないので、それぞれ標準出力へ出力するのみにとどめています)。<br>
上記の実装を踏まえて `main()` は以下のように実装します。
```go
package main

func main() {
	generator := observer.RandomMessageGenerator{}
	slackNotifier := observer.SlackNotifier{}
	emailNotifier := observer.EmailNotifier{}
	generator.AddObserver(&slackNotifier)
	generator.AddObserver(&emailNotifier)
	generator.Execute()
}

```
実行すると以下のような内容が標準出力に出力されます。
```sh
send message to slack. message = "message changed. number=81"
send message to email. message = "message changed. number=81"
```

# メリット
もう一度 `RandomMessageGenerator` の `Execute()` メソッドと `SlackNotifier` の `Notify()` メソッドを見てみましょう。
```go
func (r *RandomMessageGenerator) Execute() {
	r.message = fmt.Sprintf("message changed. number=%d", rand.Intn(100))
	for _, e := range r.observers {
		e.Notify(r)
	}
}

func (s *SlackNotifier) Notify(generator MessageGenerator) {
	fmt.Printf("send message to slack. message = \"%s\"\n", generator.GetMessage())
}
```
`Execute()` メソッドでは `Observer` インターフェースの `Notify()` メソッド、また `Notify()` メソッドでは `MessageGenerator` インターフェースの `GetMessage()` を呼んでいます。したがって `RandomMessageGenerator` は `Notify()` メソッドを呼び出す対象が `SlackNotifier` なのか `EmailNotifier` なのかは知らないですし、`SlackNotifier` も `GetMessage()` メソッドを呼び出す対象が `RandomMessageGenerator` なのかは知りません。これが `Observer` パターンのメリットであり、 *MessageGenerator および Observer インターフェースを実装した構造体は、それぞれインターフェースのメソッドを呼んで実装を行っているので、結合度が低く再利用可能なコード構成とすることが可能となります。*

# おまけ
今回は `RandomMessageGenerator` の `message` フィールドの更新を各種 `Observer` に通知するような実装を行いました。上記実装ではメッセージの更新は `main()` で一回のみ行っていましたが、`Observer` 側でメッセージを更新するケースも場合によっては考えられると思います。具体的な例としては以下になります。

```go
func (e *EmailNotifier) Notify(generator MessageGenerator) {
	fmt.Printf("send message to email. message = \"%s\"\n", generator.GetMessage())
	generator.Execute() // メッセージの更新
}
```
しかし、このような実装を行うと `メッセージの更新 -> 通知` が無限に行われることになります。
```sh
send message to slack. message = "message changed. number=87"
send message to email. message = "message changed. number=87"
send message to slack. message = "message changed. number=12"
send message to email. message = "message changed. number=12"
send message to slack. message = "message changed. number=61"
send message to email. message = "message changed. number=61"
send message to slack. message = "message changed. number=35"
send message to email. message = "message changed. number=35"
...
```
これを防ぐためには `RandomMessageGenerator` 側で「現在通知を行っているかどうか」という情報をフラグとして持たせる必要があります。

```go
type RandomMessageGenerator struct {
	observers []Observer
	message   string
	isNotifying bool
}
```
これを踏まえて `Execute()` メソッドを以下のように修正します。
```go
func (r *RandomMessageGenerator) Execute() {
    r.message = fmt.Sprintf("message changed. number=%d", rand.Intn(100))
	if !r.isNotifying {
		r.isNotifying = true
		for _, e := range r.observers {
			e.Notify(r)
		}
	}
}
```
上記の実装とすることで通知は無限に行われずメッセージの更新のみ実行されるようになります。