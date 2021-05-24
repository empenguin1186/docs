
# はじめに
- Prometheus のインストルメンテーションであるカウンタ、ゲージ、サマリ、ヒストグラムについて学んだので、内容について実装を踏まえながらまとめました。

# インストルメンテーション
- サービスの安定稼働を実現するためには、常にシステムを監視して異常をいち早く検知し対応することが必要となってきます。Prometheus ではインストルメンテーションというシステムに設定する計測機器をカスタマイズすることによって、各システムの事情に応じた監視を実現しています。インストルメンテーションは複数存在し、「何を計測したいか」という目的によって使い分けることが重要となってきます。今回はインストルメンテーションの中でもカウンタ、ゲージ、サマリ、ヒストグラムに絞って説明を行います。

## カウンタ
- カウンタはシステム上発生する何かしらのイベントを追跡するインストルメンテーションです。例としてはリクエストされた回数などを計測したい場合に使用されます。
```go
package main

import (
	echoPrometheus "github.com/labstack/echo-contrib/prometheus"
	"github.com/labstack/echo/v4"
	"github.com/prometheus/client_golang/prometheus"
	"github.com/prometheus/client_golang/prometheus/promauto"
	"net/http"
)

func main() {
	requests := promauto.NewCounter(prometheus.CounterOpts{
		Name: "hello_called_total",
		Help: "The total number of /hello called",
	})
	
	e := echo.New()
	p := echoPrometheus.NewPrometheus("echo", nil)
	p.Use(e)
	e.GET("/hello", func(c echo.Context) error {
		requests.Inc()
		return c.String(http.StatusOK, "Hello, World!\n")
	})
	e.Logger.Fatal(e.Start(":8081"))
}
```
- `localhost:8081/metrics` にリクエストすることで実際のカウンタの値を取得することができます。リクエスト実行すると、以下のようなレスポンスが返されます。
```sh
$ curl localhost:8081/metrics
...
# HELP hello_called_total The total number of /hello called
# TYPE hello_called_total counter
hello_called_total 1
...
```

## ゲージ
- ゲージはキューイングされているイベントの数や使用可能なスレッド数などといったある時点でのシステムの状態を計測する際に使用されます。カウンタはリクエスト総数といった単に増加していくメトリクスを計測するのに使用されるのに対し、ゲージは場合によって値が増減するようなメトリクスの計測に使用されます。以下はリクエストを処理するのにかかった時間をゲージで計測するコードになります。

```go
package main

import (
	echoPrometheus "github.com/labstack/echo-contrib/prometheus"
	"github.com/labstack/echo/v4"
	"github.com/prometheus/client_golang/prometheus"
	"github.com/prometheus/client_golang/prometheus/promauto"
	"net/http"
	"time"
)

func main() {
	lastTime := promauto.NewGauge(prometheus.GaugeOpts{
		Name: "hello_last_called",
		Help: "time when /hello last called",
	})

	e := echo.New()
	p := echoPrometheus.NewPrometheus("echo", nil)
	p.Use(e)
	e.GET("/hello", func(c echo.Context) error {
		lastTime.Set(float64(time.Now().Unix()))
		return c.String(http.StatusOK, "Hello, World!\n")
	})
	e.Logger.Fatal(e.Start(":8081"))
}
```
- `localhost:8081/metrics` にリクエスト実行すると、以下のようなレスポンスが返されます。
```sh
$ curl localhost:8081/metrics
...
# HELP hello_last_called time when /hello last called
# TYPE hello_last_called gauge
hello_last_called 1.621666703e+09
...
```

### コールバック
- Golang のゲージには、上記のように予め自分が指定したタイミングで値を変更することができる機能に加え、`/metrics` にリクエストすることで各種メトリクスを取得する際に予めに指定した関数に基づきゲージの値を計算するコールバックという機能も備わっています。以下の例ではメトリクス取得時にシステムで稼働している goroutine の数を表す `active_goroutine_total` というゲージを定義しています。
```go
package main

import (
	echoPrometheus "github.com/labstack/echo-contrib/prometheus"
	"github.com/labstack/echo/v4"
	"github.com/prometheus/client_golang/prometheus"
	"net/http"
	"runtime"
	"time"
)

func main() {
	prometheus.Register(prometheus.NewGaugeFunc(
		prometheus.GaugeOpts{
			Name: "active_goroutine_total",
			Help: "The number of active goroutine",
		},
		func() float64 {
			return float64(runtime.NumGoroutine())
		},
	))

	e := echo.New()
	p := echoPrometheus.NewPrometheus("echo", nil)
	p.Use(e)
	e.GET("/hello", func(c echo.Context) error {
		for i := 0; i < 5; i++ {
			go func() {
				time.Sleep(100 * time.Second)
				e.Logger.Info("goroutine completed")
			}()
		}
		return c.String(http.StatusOK, "Hello, World!\n")
	})
	e.Logger.Fatal(e.Start(":8081"))
}
```
- 下記のようにリクエストを実行すると、`active_goroutine_total` の値が変化していることが確認できます。
```sh
# リクエスト実行前のゲージを確認
$ curl localhost:8081/metrics
...
# HELP active_goroutine_total The number of active goroutine
# TYPE active_goroutine_total gauge
active_goroutine_total 5
...

# リクエスト(5つの goroutine が立ち上がる)
$ curl localhost:8081/hello                
Hello, World!

# リクエスト実行後のゲージを確認(値が5から10へと増えている)
$ curl localhost:8081/metrics
...
# HELP active_goroutine_total The number of active goroutine
# TYPE active_goroutine_total gauge
active_goroutine_total 10
...
```

## サマリ
- サマリを使用すると、計測対象イベントの発生回数と、そのイベントで計測する値の総数をメトリクスとして取得することができます。例えば、下記のコードではレイテンシを計測する `hello_latency_seconds` というサマリを定義しています。
```go
package main

import (
	echoPrometheus "github.com/labstack/echo-contrib/prometheus"
	"github.com/labstack/echo/v4"
	"github.com/prometheus/client_golang/prometheus"
	"math/rand"
	"net/http"
	"time"
)

func main() {
	latency := prometheus.NewSummary(prometheus.SummaryOpts{
		Name: "hello_latency_seconds",
		Help: "Latency for hello request",
	})
	prometheus.MustRegister(latency)

	e := echo.New()
	p := echoPrometheus.NewPrometheus("echo", nil)
	p.Use(e)
	e.GET("/hello", func(c echo.Context) error {
		start := time.Now().Unix()
		time.Sleep(time.Duration(rand.Intn(5)) * time.Second)
		latency.Observe(float64(time.Now().Unix() - start))
		return c.String(http.StatusOK, "Hello, World!\n")
	})
	e.Logger.Fatal(e.Start(":8081"))
}
```
- `/hello` に対して何回かリクエストした後にメトリクスを取得すると、以下のようなレスポンスが返ってきます。
```sh
$ curl localhost:8081/metrics
...
# HELP hello_latency_seconds Latency for hello request
# TYPE hello_latency_seconds summary
hello_latency_seconds_sum 5
hello_latency_seconds_count 3
...
```
- `hello_latency_seconds_count` は `/hello` に対してリクエストが実行された回数を表していて、`hello_latency_seconds_sum` は3回のリクエストのレイテンシの合計を表しています。この2つのメトリクスを使用することで、レイテンシの平均を算出することが可能となります。
- また、サマリでは分位数を計算することも可能です。下記のコードではリクエストが実行された場合に1から100までの値が登録されるサマリ `number_between_1_and_100` を定義しています。
```go
package main

import (
	echoPrometheus "github.com/labstack/echo-contrib/prometheus"
	"github.com/labstack/echo/v4"
	"github.com/prometheus/client_golang/prometheus"
	"net/http"
)

func main() {
	number := prometheus.NewSummary(prometheus.SummaryOpts{
		Name: "number_between_1_and_100",
		Help: "number between 1 and 100",
		Objectives: map[float64]float64{0.5: 0, 0.9: 0, 0.99: 0}, // (1)
	})
	prometheus.MustRegister(number)

	e := echo.New()
	p := echoPrometheus.NewPrometheus("echo", nil)
	p.Use(e)
	e.GET("/hello", func(c echo.Context) error {
		for i := 1; i < 101; i++ {
			number.Observe(float64(i))
		}
		return c.String(http.StatusOK, "Hello, World!\n")
	})
	e.Logger.Fatal(e.Start(":8081"))
}
```
- リクエストを実行した後にメトリクスを取得すると以下のようなレスポンスとなり、設定した分位数に対応した結果が返ってきます。
```sh
$ curl localhost:8081/metrics 
...
# HELP number_between_1_and_100 number between 1 and 100
# TYPE number_between_1_and_100 summary
number_between_1_and_100{quantile="0.5"} 50
number_between_1_and_100{quantile="0.9"} 90
number_between_1_and_100{quantile="0.99"} 99
number_between_1_and_100_sum 5050
number_between_1_and_100_count 100
...
```
- 取得したい分位数は、Goのコードでは(1)の箇所で map を用いて設定を行っています。keyの方に取得したい分位数を設定しています。value に設定する値に関しては、[ライブラリのNewSummaryの項目](https://pkg.go.dev/github.com/prometheus/client_golang/prometheus#NewSummary)で説明されていますが、今回は一律0としています。

## ヒストグラム
- ヒストグラムは、指定した範囲に存在する値がどれくらい存在するかを計測する際に使用されます。下記のコードでは、レイテンシの値の分布を計測するヒストグラム `hello_latency_seconds` を定義しています。
```go
import (
	echoPrometheus "github.com/labstack/echo-contrib/prometheus"
	"github.com/labstack/echo/v4"
	"github.com/prometheus/client_golang/prometheus"
	"math/rand"
	"net/http"
	"time"
)

histogram := prometheus.NewHistogram(prometheus.HistogramOpts{
		Name: "hello_latency_seconds",
		Help: "Histogram or latency for hello request",
	})
	prometheus.MustRegister(histogram)

	e := echo.New()
	p := echoPrometheus.NewPrometheus("echo", nil)
	p.Use(e)
	e.GET("/hello", func(c echo.Context) error {
		start := time.Now().Unix()
		time.Sleep(time.Duration(rand.Intn(5)) * time.Second)
		histogram.Observe(float64(time.Now().Unix() - start))
		return c.String(http.StatusOK, "Hello, World!\n")
	})
	e.Logger.Fatal(e.Start(":8081"))
```
- 何回かリクエストを実行してメトリクスを取得したところ、以下のような結果となりました。

```sh
$ curl localhost:8081/metrics
...
# HELP hello_latency_seconds Histogram or latency for hello request
# TYPE hello_latency_seconds histogram
hello_latency_seconds_bucket{le="0.005"} 0
hello_latency_seconds_bucket{le="0.01"} 0
hello_latency_seconds_bucket{le="0.025"} 0
hello_latency_seconds_bucket{le="0.05"} 0
hello_latency_seconds_bucket{le="0.1"} 0
hello_latency_seconds_bucket{le="0.25"} 0
hello_latency_seconds_bucket{le="0.5"} 0
hello_latency_seconds_bucket{le="1"} 1
hello_latency_seconds_bucket{le="2.5"} 4
hello_latency_seconds_bucket{le="5"} 5
hello_latency_seconds_bucket{le="10"} 5
hello_latency_seconds_bucket{le="+Inf"} 5
hello_latency_seconds_sum 11
hello_latency_seconds_count 5
...
```
- `hello_latency_seconds_bucket` は設定した範囲内に存在する値がどれくらい存在するかを表しています。例えば、`hello_latency_seconds_bucket{le="2.5"}` というメトリクスの値は4となっていますが、これはレイテンシが2.5秒以下のリクエストが4回実行されたことを表しています。
- 集計範囲については自分で設定することも可能で、これはバケットと呼ばれています。例えば、以下のコードは範囲を1~5秒まで1秒ずつ区切るようなバケットを定義しています。
```go
package main

import (
	echoPrometheus "github.com/labstack/echo-contrib/prometheus"
	"github.com/labstack/echo/v4"
	"github.com/prometheus/client_golang/prometheus"
	"math/rand"
	"net/http"
	"time"
)

func main() {
	histogram := prometheus.NewHistogram(prometheus.HistogramOpts{
		Name: "hello_latency_seconds",
		Help: "Histogram or latency for hello request",
    // 1~5秒の範囲で1秒ずつ
		Buckets: prometheus.LinearBuckets(1, 1, 5),
	})
	prometheus.MustRegister(histogram)

	e := echo.New()
	p := echoPrometheus.NewPrometheus("echo", nil)
	p.Use(e)
	e.GET("/hello", func(c echo.Context) error {
		start := time.Now().Unix()
		time.Sleep(time.Duration(rand.Intn(5)) * time.Second)
		histogram.Observe(float64(time.Now().Unix() - start))
		return c.String(http.StatusOK, "Hello, World!\n")
	})
	e.Logger.Fatal(e.Start(":8081"))
}
```
- 何回かリクエストを実行してメトリクスを取得したところ、以下のような結果となりました。
```sh
$ curl localhost:8081/metrics
...
# HELP hello_latency_seconds Histogram or latency for hello request
# TYPE hello_latency_seconds histogram
hello_latency_seconds_bucket{le="1"} 2
hello_latency_seconds_bucket{le="2"} 2
hello_latency_seconds_bucket{le="3"} 2
hello_latency_seconds_bucket{le="4"} 3
hello_latency_seconds_bucket{le="5"} 3
hello_latency_seconds_bucket{le="+Inf"} 3
hello_latency_seconds_sum 5
hello_latency_seconds_count 3
...
```

# まとめ
- Prometheus のカウンタ、ゲージ、サマリ、ヒストグラムについて学びました。これらのインストルメンテーションと[Prometheusのアラート機能](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/)を適切に使用することでシステム固有の課題に直面したとしても容易に異常を検知することができそうです。


# 参考文献
- [Goのprometheusライブラリ](https://pkg.go.dev/github.com/prometheus/client_golang/prometheus)
- [Instrumenting a Go application | Prometheus](https://prometheus.io/docs/guides/go-application/)
- [Prometheus Middleware | Echo - High performance, minimalist Go web framework](https://echo.labstack.com/middleware/prometheus/)