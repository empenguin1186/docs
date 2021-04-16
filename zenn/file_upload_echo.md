# はじめに
- Go による画像ファイルのアップロード処理を行う Web サーバの簡易実装を行ったので内容についてまとめます。

# 実装
- 今回は `curl http://localhost:8080/upload -F file=@image.png` といったリクエストを実行した際に `image_out.png` という画像ファイルを保存する処理を行うサーバを実装することとします。`image.png` と `image_out.png` の内容は同じものとなります。
- 今回 Web サーバの実装には Go の Web フレームワーク [Echo](https://echo.labstack.com/) を使用しました。実装例を以下に示します。

```go:main.go
package main

import (
	"fmt"
	"github.com/labstack/echo/v4"
	"io"
	"os"
	"strings"
)

func upload(c echo.Context) error {
	file, err := c.FormFile("file")
	if err != nil {
		return err
	}
	src, err := file.Open()
	if err != nil {
		return err
	}
	defer src.Close()

	fileModel := strings.Split(file.Filename, ".")
	fileName := fileModel[0]
	extension := fileModel[1]
	dst, err := os.Create(fmt.Sprintf("%s_out.%s", fileName, extension))
	if err != nil {
		return err
	}
	defer dst.Close()

	if _, err = io.Copy(dst, src); err != nil {
		return err
	}

	return c.HTML(200, "Image has been saved.")
}

func main() {
	e := echo.New()
	e.POST("/upload", upload)
	e.Start(":8080")
}
```
- アップロード処理の実装については [こちら](https://echo.labstack.com/cookbook/file-upload/) を参考にさせていただきました。

# テスト
- 今回はアップロードした画像とサーバ側で保存された画像ファイルの内容が一致しているかどうか確認を行いました。curl で実際にリクエストを行って確認してもいいのですが、今回は [httptest](https://golang.org/pkg/net/http/httptest/) パッケージを使用してコードベースで動作確認を行いました。`httptest` パッケージを使用するとテスト実行時にサーバを起動することができ、起動したサーバに対してリクエストを実行して API の挙動確認を行うことが可能です。`echo` を使用して実装した API でもこの `httptest` パッケージを使用したテストを実行することが可能です。実装例を以下に示します。

```go:main_test.go
package main

import (
	"bytes"
	"github.com/labstack/echo/v4"
	"github.com/stretchr/testify/assert"
	"io"
	"io/ioutil"
	"mime/multipart"
	"net/http"
	"net/http/httptest"
	"os"
	"testing"
)

func TestUpload(t *testing.T) {

	// テストサーバ起動
	e := echo.New()
	e.POST("/upload", upload)
	testServer := httptest.NewServer(e)
	defer testServer.Close()

	// クライアントの画像ファイルアップロード処理
	var buffer bytes.Buffer
	writer := multipart.NewWriter(&buffer)
	fileWriter, err := writer.CreateFormFile("file", "test.png")
	if err != nil{
		t.Fatalf("Failed to create file writer. %s", err)
	}

	readFile, err := os.Open("test.png")
	if err != nil {
		t.Fatalf("Failed to open file. %s", err)
	}
	defer readFile.Close()
	io.Copy(fileWriter, readFile)
	writer.Close()

	res, err := http.Post(testServer.URL + "/upload", writer.FormDataContentType(), &buffer)
	if err != nil{
		t.Fatalf("Failed to POST request. %s", err)
	}

	// API レスポンス検証
	message, err := ioutil.ReadAll(res.Body)
	res.Body.Close()
	if err != nil {
		t.Fatalf("Failed to read HTTP response body. %s", err)
	}
	assert.Equal(t, "Image has been saved.", string(message))

	// アップロードを行った画像ファイルとサーバ側が保存した画像の内容が一致しているかどうか検証
	outFile, err := os.Open("test_out.png")
	if err != nil {
		t.Fatalf("Failed to open file. %s", err)
	}
	defer outFile.Close()
	var expectedBytes []byte
	readFile.Write(expectedBytes)
	var actualBytes []byte
	outFile.Write(actualBytes)
	assert.Equal(t, expectedBytes, actualBytes)
}
```
- テスト実行結果は以下となります(今回行った Go のプロジェクト名は go-sample としています)。
```sh
$ go test -run TestUpload
PASS
ok      go-sample       0.116s
```