# はじめに
- Go による TLS 通信を受け付けるサーバの実装方法について学んだので内容をまとめます。

# 実装
- 最初は標準ライブラリでの実装方法を学んだのですが、実際 Go で Web アプリケーションを開発する際には Web Framework を使用すると思うので、今回は標準ライブラリでの実装方法に加えて Go の Web Framework である echo と gin による TLS 通信の実装方法についても調べて実装しました。

## 環境
- 今回使用した Go, echo, gin のバージョンは以下となります。

|  項目  |  バージョン  |
|:--:|:--:|
|  Go  |  1.15.5  |
|  echo  |  v4.2.1  |
|  gin  |  v1.6.3  |

## 秘密鍵及び証明書

- 今回は自己署名のルート認証局証明書を用いてサーバ証明書の署名を行うようにします。まずルート認証局証明書を作成するため以下のコマンドを実行しました。
```sh
// 秘密鍵作成
$ openssl genrsa -out ca.key 2048

// 証明書署名要求を作成
$ openssl req -new -sha256 -key ca.key -out ca.csr

// 自己署名のルート認証局証明書作成
$ openssl x509 -in ca.csr -days 365 -req -signkey ca.key -sha256 -out ca.crt

// 証明書の内容を確認
$ openssl x509 -in ca.crt -text
Certificate:
    Data:
        Version: 1 (0x0)
        Serial Number: 13728320786437535549 (0xbe84cb04fe38773d)
    Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=JP, ST=Tokyo, L=Minato-ku, O=empenguin.com
        Validity
            Not Before: Apr  5 14:02:00 2021 GMT
            Not After : Apr  5 14:02:00 2022 GMT
        Subject: C=JP, ST=Tokyo, L=Minato-ku, O=empenguin.com
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
...
```

- 次にサーバが使用する秘密鍵と証明書を以下のコマンドを実行して作成しました。なお Common Name は `localhost` としています。
```sh
// 秘密鍵作成
$ openssl genrsa -out server.key 2048

// 証明書署名要求作成
$ openssl req -new -nodes -sha256 -key server.key -out server.csr

// サーバ証明書作成
$ openssl x509 -req -days 365 -in server.csr -sha256 -out server.crt -CA ca.crt -CAkey ca.key -CAcreateserial -extensions Server

// 証明書の内容を確認
$ openssl x509 -in server.crt -text
Certificate:
    Data:
        Version: 1 (0x0)
        Serial Number: 18369478063848121124 (0xfeed7ef930ff6324)
    Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=JP, ST=Tokyo, L=Minato-ku, O=empenguin.com
        Validity
            Not Before: Apr  5 14:04:29 2021 GMT
            Not After : Apr  5 14:04:29 2022 GMT
        Subject: C=JP, ST=Tokyo, L=Minato-ku, O=adely.com, CN=localhost
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:

```

## 標準ライブラリ(net/http)を使用する方法

- まずは標準ライブラリである `net/http` を用いて TLS 通信を受け付けるサーバの実装を行いました。実装例は以下となります。
```go:main.go
package main

import (
	"fmt"
	"net/http"
)

const (
    FIELD = "Message"
    MESSAGE = "Hello, World!"
    FORMAT = "{\"%s\":\"%s\"}"
    PORT = ":18443"
)

func main() {
  m := http.NewServeMux()
	m.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintf(w, fmt.Sprintf(FORMAT, FIELD, MESSAGE))
	})
	http.ListenAndServeTLS(PORT, "server.crt", "server.key", m)
}
```

## echo を使用する方法

- 次に echo を使用した実装例を以下に示します。
```go:main.go
package main

import (
	"fmt"
	"github.com/labstack/echo/v4"
	"net/http"
)

const (
    FIELD = "Message"
    MESSAGE = "Hello, World!"
    FORMAT = "{\"%s\":\"%s\"}"
    PORT = ":18443"
)

func main() {
	e := echo.New()
	e.GET("/", func(c echo.Context) error {
		return c.HTML(http.StatusOK, fmt.Sprintf(FORMAT, FIELD, MESSAGE))
	})
	e.StartTLS(PORT, "server.crt", "server.key")
}
```

## gin を使用する方法

- 最後に gin を使用した実装例を以下に示します。
```go:main.go
package main

import (
	"github.com/gin-gonic/gin"
)

const (
    FIELD = "Message"
    MESSAGE = "Hello, World!"
    PORT = ":18443"
)

func main() {
	r := gin.Default()
	r.GET("/", func(c *gin.Context) {
		c.JSON(200, gin.H{
			FIELD:MESSAGE,
		})
	})
	r.RunTLS(PORT, "server.crt", "server.key")
}
```

# 動作確認

- curl コマンドを実行して動作確認を行いました。どの実装でも実行結果は以下となりました。
```sh
// 証明書を指定せずにリクエスト
$ curl https://localhost:18443/ 
curl: (60) SSL certificate problem: unable to get local issuer certificate
More details here: https://curl.haxx.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the web page mentioned above.

// 証明書を指定してリクエスト
$ curl --cacert ca.crt https://localhost:18443/ 
{"Message":"Hello, World!"}
```

# おまけ
- 標準ライブラリでの実装で使用した `http.ListenAndServeTLS()`の第4引数である [http.Handler](https://golang.org/pkg/net/http/#Handler) は、`ServeHTTP(ResponseWriter, *Request)` というメソッドをもつインターフェースです。また、echo を用いた実装で使用した `echo.New()` の戻り値である [*Echo](https://pkg.go.dev/github.com/labstack/echo#Echo), gin を用いた実装で使用した `gin.Default()` の戻り値である [*Engine](https://pkg.go.dev/github.com/gin-gonic/gin#Engine) はどちらも `ServeHTTP(ResponseWriter, *Request)` を実装しているので([func (*Echo) ServeHTTP](https://pkg.go.dev/github.com/labstack/echo#Echo.ServeHTTP), [func (*Engine) ServeHTTP](https://pkg.go.dev/github.com/gin-gonic/gin#Engine.ServeHTTP))、以下のような実装でも TLS 通信を実装することができます。

```go:main.go
package main

import (
	"fmt"
	"github.com/gin-gonic/gin"
	"github.com/labstack/echo/v4"
	"net/http"
)

const (
	FIELD = "Message"
    MESSAGE = "Hello, World!"
    FORMAT = "{\"%s\":\"%s\"}"
    PORT = ":18443"
)

func ServeWithEcho() *echo.Echo {
	e := echo.New()
	e.GET("/", func(c echo.Context) error {
		return c.HTML(http.StatusOK, fmt.Sprintf(FORMAT, FIELD, MESSAGE))
	})
	return e
}

func ServeWithGin() *gin.Engine {
	r := gin.Default()
	r.GET("/", func(c *gin.Context) {
		c.JSON(200, gin.H{
			FIELD:MESSAGE,
		})
	})
	return r
}

func main() {
　// echo を用いた TLS 通信
  http.ListenAndServeTLS(PORT, "server.crt", "server.key", ServeWithEcho())
	
  // gin を用いた TLS 通信
  http.ListenAndServeTLS(PORT, "server.crt", "server.key", ServeWithGin())
}

```