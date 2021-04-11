# はじめに
- Go 言語で JSON-RPC の実装を行ったので内容をまとめます。

# JSON-RPC とは
- JSON-RPC とは RPC(Remote Procedure Call)の規格の一つです。RPC とはサーバ側が実装しているメソッドをクライアントが呼び出すことによって処理を行う仕組みのことを指します。そしてクライアントがサーバに対してリクエストを行う際に、メソッド名やメソッドの引数といった情報を構造化するのに JSON を使用したものを JSON-RPC と呼びます。[こちらのサイト](https://www.jsonrpc.org/specification#request_object) に具体的な仕様についての説明がなされているので、興味のある方はご覧になってみてください。具体的にはクライアント側は以下のようなリクエストをサーバ側に対して行います。

```sh
POST /add HTTP/1.1
Host: localhost:8080
Content-Length: 126
Content-Type: application/json

{
  "jsonrpc": "2.0",
  "method": "Calculator.Add",
  "params": {
      "Add": 13,
      "Added": 10
  },
  "id": 123
}
```
- `jsonrpc` フィールドは現在の JSON-RPC のバージョンである `2.0` を指定しています。`method` フィールドで呼び出すサーバ側のメソッドを指定し、そのメソッドに渡す引数を `params` フィールドに設定しています。`id` フィールドはリクエストを識別するために付与されるもので、サーバからのレスポンスもリクエストで指定された `id` の値を返すことになります。上記のリクエストに対するサーバからのレスポンスは以下のようになります。

```sh
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 48

{
  "jsonrpc": "2.0",
  "result": {
    "Computation": 22
  },
  "id": 123
}
```
- `jsonrpc` と `id` フィールドはリクエストの説明の際に述べた内容と同じです。`result` フィールドに実際に呼び出されたメソッドの処理結果が含まれています。`result` フィールドには数値や文字列といった基本データ型だけでなく、開発者が作成した型のデータを含めることも可能です。今回は int 型の `Computation` フィールドを含むオブジェクトを `result` フィールドに設定しています。

# 実装
- それでは実際に JSON-RPC を用いた通信の実装をサーバ側とクライアント側に分けて実装していきたいと思います。今回使用した Go のバージョンは `1.15.5` です。

## サーバ側
- まずはサーバ側の実装について紹介します。Go の標準パッケージである [jsonrpc](https://golang.org/pkg/net/rpc/jsonrpc/) は JSON-RPC 1.0 しか対応していないようだったので、今回 JSON-RPC の実装については [gorilla/rpc](https://github.com/gorilla/rpc) というサードパーティのライブラリを使用しました。以下にコードを示します。

```go:main.go
package main

import (
	"github.com/gorilla/rpc/v2"
	"github.com/gorilla/rpc/v2/json2"
	"net/http"
)

type (
	Calculator struct {}
	AdditionArgs struct {
		Add, Added int
	}
	AdditionResult struct {
		Computation int
	}
)

func (c Calculator) Add(r *http.Request, args *AdditionArgs, result *AdditionResult) error {
	result.Computation = args.Add + args.Added
	return nil
}

func main() {
	s := rpc.NewServer()
	s.RegisterCodec(json2.NewCodec(), "application/json")
	calculator := &Calculator{}
	s.RegisterService(calculator, "")
	http.Handle("/add", s)
	http.ListenAndServe(":8080", nil)
}
```
- `Calculator` という構造体の `Add` という足し算を行うメソッドが定義されています。今回この `Add` メソッドをクライアントから呼び出すこととします。メソッドの引数については int 型の `足される数`と`足す数` をメンバとして持つ `AdditionArgs` という構造体で表現しています。計算結果については `AdditionResult` の `Computation` に格納することとしています。この `AdditionResult` の内容がクライアントに返却するレスポンスに設定されることになります。

## クライアント側

- 続いてクライアント側の実装について紹介します。クライアントの実装に関しては [ybbus/jsonrpc](https://github.com/ybbus/jsonrpc) というサードパーティのライブラリを使用しました。以下にコードを示します。
```go:main
package main

import (
	"github.com/ybbus/jsonrpc/v2"
	"log"
)

type (
	Calculator struct {}
	AdditionArgs struct {
		Add, Added int
	}
	AdditionResult struct {
		Computation int
	}
)

func main() {

	added := 10
	add := 12
	rpcClient := jsonrpc.NewClient("http://localhost:8080/add")
	response, err := rpcClient.Call("Calculator.Add", &AdditionArgs{Added: added, Add: add}) // (1)
	if err != nil {
		log.Fatalln(err)
	}
	var result AdditionResult
	err = response.GetObject(&result) // (2)
	if err != nil {
		log.Fatalln(err)
	}
	log.Printf("%d + %d = %d", added, add, result.Computation)
}
```
- `(1)` でリクエストの内容を設定しています。設定するメソッド名は `${構造体名}.${メソッド名}` となります。`(2)` でサーバからのレスポンスを `AdditionResult` にバインドしています。実行結果は以下のようになります。
```sh
go run main.go
2021/04/11 13:00:31 10 + 12 = 22
```
- また、curl での実行結果は以下となります。
```sh
$ curl -v http://localhost:8080/add -d '{"method":"Calculator.Add","params":{"Add":12,"Added":10},"id":0,"jsonrpc":"2.0"}' -H "Content-Type: application/json" | python -m json.tool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*   Trying ::1...
* TCP_NODELAY set
* Connected to localhost (::1) port 8080 (#0)
> POST /add HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/x.y.z
> Accept: */*
> Content-Type: application/json
> Content-Length: 81
>
} [81 bytes data]
* upload completely sent off: 81 out of 81 bytes
< HTTP/1.1 200 OK
< Content-Type: application/json; charset=utf-8
< X-Content-Type-Options: nosniff
< Date: Sun, 11 Apr 2021 04:33:05 GMT
< Content-Length: 53
<
{ [53 bytes data]
100   134  100    53  100    81  13250  20250 --:--:-- --:--:-- --:--:-- 33500
* Connection #0 to host localhost left intact
* Closing connection 0
{
    "id": 0,
    "jsonrpc": "2.0",
    "result": {
        "Computation": 22
    }
}
```

# 参考文献
- [JSON-RPC 2.0 Specification](https://www.jsonrpc.org/specification#request_object)
- [jsonrpc - The Go Programming Language](https://golang.org/pkg/net/rpc/jsonrpc/)
- [ybbus/jsonrpc: A simple go implementation of json rpc 2.0 client over http](https://github.com/ybbus/jsonrpc)
- [rpc/server.go at master · gorilla/rpc](https://github.com/gorilla/rpc/blob/master/v2/server.go)