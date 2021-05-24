
# はじめに
- Go で Echo を使用した Web アプリケーションと Prometheus を連携させ、アプリケーションで独自に定義したメトリクスを Prometheus でスクレイプする実装を行ったので、手順についてまとめます。

# 実装
- 今回は、クライアントからのリクエストをカウントするメトリクスを定義します。その際にクライアントから指定されたリクエストパラメータ `number` が偶数か奇数かでメトリクスに付与するラベルを変更するような実装を行います。

## Go の実装
- はじめに Go の実装を行います。まず必要なライブラリをインストールします。具体的には以下のコマンドを実行します。また、今回使用した Go のバージョンは `1.16` となります。
```sh
$ go install github.com/labstack/echo/v4@latest
$ go install github.com/labstack/echo-contrib/prometheus@latest
$ go install github.com/prometheus/client_golang/prometheus@latest
```

- 次にサーバの実装を行います。実装例を以下に示します。
```go:main.go
package main

import (
	"fmt"
	echoPrometheus "github.com/labstack/echo-contrib/prometheus"
	"github.com/labstack/echo/v4"
	"github.com/prometheus/client_golang/prometheus"
	"net/http"
	"strconv"
)

// メトリクスを記録する構造体
type MetricsMonitors struct {
	counter *prometheus.CounterVec
}

// MetricsMonitor の初期化メソッド
func NewMetricsMonitors() MetricsMonitors {
	monitors := MetricsMonitors{
		counter: prometheus.NewCounterVec(
			prometheus.CounterOpts{
				Name: "myapp_request_total",
				Help: "The total number of requests with number param",
			},
			[]string{"odd_or_even"},
		),
	}
	prometheus.MustRegister(monitors.counter)
	return monitors
}

// メトリクスを記録するメソッド
func (mm *MetricsMonitors) record(number int) {
	if number % 2 != 0 {
		m := mm.counter.WithLabelValues("odd")
		m.Inc()
	} else {
		m := mm.counter.WithLabelValues("even")
		m.Inc()
	}
}

func main() {
	e := echo.New()
	p := echoPrometheus.NewPrometheus("echo", nil)
	p.Use(e)
	metricsMonitors := NewMetricsMonitors()
	e.GET("/", func(c echo.Context) error {
		numStr := c.QueryParam("number")
		param, err := strconv.Atoi(numStr)
		
		// パラメータに数値以外が指定された場合にはステータスコード400を返す
		if err != nil {
			return c.String(http.StatusBadRequest, fmt.Sprintf("\"number\" parameter must be number, but actual is %s", numStr))
		}
		
		metricsMonitors.record(param)
		return c.String(http.StatusOK, "Hello, World!\n")
	})
	e.Logger.Fatal(e.Start(":8081"))
}
```

## Prometheus の設定
- 次に Prometheus 側の設定を行います。prometheus.yml に以下の内容を追記します。
```yaml:prometheus.yml
...
scrape_configs:
  ...

  - job_name: 'echo'
    metrics_path: '/metrics'
    static_configs:
      - targets: ['localhost:8081']
```
- これでアプリケーションが出力するメトリクスをスクレイプする準備が整いました。

# 動作確認
- 実際にアプリケーションを起動させ、Prometheus の UI からメトリクスが取得できるかどうか確認を行いました。まず以下のようなコマンドを実行してアプリケーションに対して HTTP リクエストを実行しました。
```sh
# パラメータに奇数を指定
$ curl -v "localhost:8081?number=3"
...
< HTTP/1.1 200 OK
< Content-Type: text/plain; charset=UTF-8
< Date: Fri, 30 Apr 2021 14:56:26 GMT
< Content-Length: 14
<
Hello, World!
* Connection #0 to host localhost left intact
* Closing connection 0

# パラメータに偶数を指定
$ curl -v "localhost:8081?number=4"
...
< HTTP/1.1 200 OK
< Content-Type: text/plain; charset=UTF-8
< Date: Fri, 30 Apr 2021 14:56:23 GMT
< Content-Length: 14
<
Hello, World!
* Connection #0 to host localhost left intact
* Closing connection 0
```
- 次に Prometheus が起動している `localhost:9090` にアクセスし、今回作成したメトリクスである `myapp_request_total` がスクレイプできているかを確認します。結果以下のようにメトリクスがスクレイプできていることが確認できました。