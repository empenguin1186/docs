# はじめに
- HTTP/2 のサーバプッシュを Go で実装したので、内容をまとめます。

# サーバプッシュとは
- 基本的に HTTP による通信はクライアントがサーバに対してリクエストを実行し、リクエストを受け付けたサーバがそのリクエストに応じた処理を実行しレスポンスを返すといったもので、一連の処理は必ずクライアントから始まっていました。これに対してサーバプッシュとは、クライアントからのリクエストが実行される前に、サーバがコンテンツをクライアントに対して送信するという機能です。クライアントは実際にそのコンテンツをリクエストするまでは、そのコンテンツがサーバから送信されていることを認識することはできませんが、リクエストを実行すれば通常よりも短い時間でコンテンツを取得することが可能となります。以下にサーバプッシュを実装した場合と実装しなかった場合の処理フローを示します。

# 実装
- それでは実装に移りたいと思います。今回言語は Go、フレームワークは echo を使用しました。今回ディレクトリ構成は以下のようにしています。
```sh
$ tree .
.
├── ca.crt
├── go.mod
├── go.sum
├── main.go
├── static
│   ├── index.html
│   └── lena.png
├── server.crt
└── server.key
```
- HTTP/2 はTLS通信が必須となっているので、TLS通信に必要な自己署名証明書 `ca.crt`、サーバ証明書 `server.crt`、サーバ秘密鍵 `server.key` を用意しています。サイトで使用するファイルは `static` 配下に配置しています。`lena.png` については画像処理の分野でよく用いられる [lena](http://optipng.sourceforge.net/pngtech/img/lena.html) の画像を使用しました。
- 次に Go の実装例について以下に示します。実装については [echo のサイト](https://echo.labstack.com/cookbook/http2-server-push/) で公開されている実装を参考にさせていただきました。

```go:main.go
package main

import (
	"flag"
	"github.com/labstack/echo/v4"
	"log"
	"net/http"
)

const defaultEnableServerPush = true

func main() {
	var enableServerPush bool
	flag.BoolVar(&enableServerPush, "enableServerPush", defaultEnableServerPush, "server push option")
	flag.Parse()

	log.Printf("enableServerPush: %t", enableServerPush)
	e := NewServerPushServer(enableServerPush)
	e.StartTLS(":18443", "server.crt", "server.key")
}

func NewServerPushServer(enableServerPush bool) *echo.Echo {
	e := echo.New()
	e.Static("/", "static")
	if enableServerPush {
		e.GET("/", serverPush)
	}
	return e
}

func serverPush(c echo.Context) error {
	pusher, ok := c.Response().Writer.(http.Pusher)
	if ok {
		if err := pusher.Push("/lena.png", nil); err != nil {
			return err
		}
	}
	return c.File("static/index.html")
}
```
- 今回サーバプッシュの有無でレスポンスタイムがどのように変化するのかを検証するため、起動時に指定する `enableServerPush` オプションでサーバプッシュを有効にするのか無効にするのかの設定を行えるようにしています。
- `index.html` の実装については以下となります。
```html:index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Server Push Test</title>
</head>
<body>
<img class="top-image" src="/lena.png">
</body>
</html>
```

# 検証
- サーバプッシュを有効にした場合と無効にした場合でレスポンスタイムがどう変化するのかを検証しました。具体的には `main.go` を実行し、Chrome で `https://localhost:18443/` にアクセスした際にデベロッパーツールで通信内容を確認しました。また、サーバプッシュを無効にしてサーバを起動したい場合は以下のコマンドで実行できます。

```sh
$ go run main.go --enableServerPush=false
```

## サーバプッシュ無効時
- まずはサーバプッシュを無効にした場合の通信内容です。デベロッパーツールの `Network` タブでは以下のように表示されました。全体で168ms 要していることが確認できます。

- `lena.png` の `Waterfall`の項目にマウスをホバーさせると、以下のような内容が表示されます。画像をリクエストしてから(Request sent)ダウンロードが完了するまで(Content Download)に約40ms 要していることが確認できます。

## サーバプッシュ有効時
- 次にサーバプッシュを有効にした場合の通信内容です。以下が `Network` タブの内容です。全体で77ms 要していることが確認できます。

- `lena.png` の `Waterfall` の内容は以下となります。画像の取得はすでにキューイングされた画像を読み込むだけなので、約9ms しかかかっていません。

# まとめ
- Go によるサーバプッシュの実装を行い、サーバプッシュ有効時と無効時の挙動を確認することで、サーバプッシュ有効時のメリットを実感することができました。