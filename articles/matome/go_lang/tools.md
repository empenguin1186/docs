<!-- TOC -->

- [便利なツール群](#便利なツール群)
  - [goimports](#goimports)
    - [機能](#機能)
    - [使用例](#使用例)
    - [補足](#補足)
  - [go vet](#go-vet)
    - [機能](#機能-1)
    - [使用例](#使用例-1)
  - [golint](#golint)
    - [機能](#機能-2)
    - [使用例](#使用例-2)
  - [gopls](#gopls)
    - [機能](#機能-3)
    - [VS Code で使用するには](#vs-code-で使用するには)
    - [参考文献](#参考文献)
  - [go mod](#go-mod)
    - [機能](#機能-4)
    - [使用例](#使用例-3)
      - [環境変数の設定](#環境変数の設定)
      - [Module 機能を有効にする](#module-機能を有効にする)
      - [go get 実行](#go-get-実行)
    - [参考文献](#参考文献-1)
  - [その他](#その他)
    - [ghq, peco](#ghq-peco)
    - [参考文献](#参考文献-2)

<!-- /TOC -->

# 便利なツール群

## goimports
### 機能
- コードのフォーマット
- 必要なパッケージの import 文の追加
- 不要なパッケージの import 文の削除
  - Go では不要なパッケージが import されているとコンパイルエラーとなる。

### 使用例
```sh
$ goimports -w main.go
```
- `-w`はファイルの上書きを行うオプション。 


### 補足
- goimports を高速化させるツールとして `dragon-import` というものが存在する。
  - https://blog.monochromegane.com/blog/2015/12/23/dragon-imports/

## go vet
### 機能
- バグの原因になりそうな箇所を表示してくれるツール

### 使用例
```
$ go vet ./...
go vet ./...
# github.com/...
utils/ecdsa.go:40:10: crypto/ecdsa.PublicKey composite literal uses unkeyed fields
utils/ecdsa.go:47:10: crypto/ecdsa.PrivateKey composite literal uses unkeyed fields
```
- ファイルパスを`./...`と指定することでカレントディレクトリ配下に存在する全てのファイルを対象とすることができる。

## golint
### 機能
- Go らしくないコードに対して警告を表示するツール

### 使用例
```sh
$ golint ./...
block/blockchain.go:20:2: don't use ALL_CAPS in Go names; use CamelCase
block/blockchain.go:20:2: exported const MINING_DIFFICULTY should have comment (or a comment on this block) or be unexported
block/blockchain.go:21:2: don't use ALL_CAPS in Go names; use CamelCase
```

## gopls
### 機能
- 公式で開発されている Go の LSP(Language Server Protocol) サーバであり、コードの補完や定義ジャンプの機能を提供する

### VS Code で使用するには
Go の settings.json に以下を記述し、再起動することでコード補完が動作することを確認できた。
```json
{
  "go.useLanguageServer": true,
  "go.languageServerExperimentalFeatures": {
      "format": true,
      "autoComplete": true,
      "rename": true,
      "goToDefinition": true,
      "hover": true,
      "signatureHelp": true,
      "goToTypeDefinition": true,
      "goToImplementation": true,
      "documentSymbols": true,
      "workspaceSymbols": true,
      "findReferences": true,
      "diagnostics": true
  },
  "[go]": {
      "editor.snippetSuggestions": "none",
      "editor.formatOnSave": true,
      "editor.codeActionsOnSave": {
          "source.organizeImports": true
      },
  },
  "go.toolsEnvVars": {
      "GO111MODULE": "on"
  },
  "go.formatTool": "goimports",
  "files.eol": "\n"
}
```

### 参考文献
- https://github.com/Microsoft/vscode-go/wiki/Go-modules-support-in-Visual-Studio-Code
- https://qiita.com/ayatokura/items/4301e0d1d8b339f722eb

## go mod

### 機能
- 公式が提供している Go のプロジェクトの依存管理ツール。

### 使用例

#### 環境変数の設定
- まずはじめに`$GO111MODULE`という環境変数を`on`にしておく
```sh
$ export GO111MODULE=on
```

#### Module 機能を有効にする
```sh
$ go mod init path/to/project
go: creating new go.mod: module path/to/project
```
- コマンド実行後、`go.mod`というファイルが作成される。  

#### go get 実行
- go.mod 作成後、`go get <パッケージ>`でパッケージをインストールした場合新たに `go.sum`というファイルが作成される。これはパッケージのバージョンをロックするために作成された、インストールしたパッケージのリストおよびそのスナップショットが記述されたファイルである。これによりどの環境でも同じバージョンのパッケージがインストールされることとなる。

### 参考文献
- https://blog.golang.org/using-go-modules
- https://github.com/golang/go/wiki/Modules

## その他
### ghq, peco
- ソースコードの取得、リポジトリ間の移動を簡単に行うことができるツール

### 参考文献
- https://qiita.com/strsk/items/9151cef7e68f0746820d