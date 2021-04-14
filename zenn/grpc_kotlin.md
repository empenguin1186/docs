# はじめに
- Kotlin で Spring Boot と gRPC を使用したデモアプリケーションを実装したので実装内容をまとめます。

# gRPC とは
- [gRPC](https://grpc.io/) とは Google が開発した高性能な RPC(Remote Procedure Call) フレームワークです。RPC とはサーバ側に実装されているメソッドをクライアント側が呼び出して処理を実行する仕組みのことを指します。RPC には gRPC の他にも JSON-RPC と呼ばれるクライアントとサーバ間のデータのやり取りに JSON を使用する規格も存在します。JSON-RPC に関しては [以前私が執筆した記事](https://zenn.dev/empenguin/articles/9ce4b7dd4edb66) があるので興味のある方はご覧になってみてください。
- gRPC を使用するメリットとしては通信の高速化が挙げられます。これは gRPC はデフォルトでストリームや HPACK によるヘッダ圧縮などの HTTP/2 の機能が使用されているためです。

# プロトコルバッファ
- gRPC は**プロトコルバッファ**と呼ばれる IDL (Interface Definition Language) を使用することで、クライアントが呼び出すことができるメソッドや、リクエストやレスポンスの構造を定義します。プロトコルバッファの具体例を以下に示します。
```
service Calculator {
    rpc Add (AdditionRequest) returns (AdditionReply);
}

message AdditionRequest {
    int32 added = 1;
    int32 add = 2;
}

message AdditionReply {
    int32 result = 1;
}
```
- `service.rpc` 句でクライアントが呼び出すことのできるメソッドの定義を行っています。今回は`AdditionRequest` を受け取り、何らかの処理を行い `AdditionReply` を返す `Add` という名前のメソッドを定義しています。
- `message` 句では `Add` メソッドで使用するデータの構造を定義しています。例えば `AdditionRequest` は `added` と `add` という int 型の2つのフィールドを持つオブジェクトとして定義されています。フィールドに割り振られている番号については各フィールドの順序を表しています。
- gRPC は上記のようなプロトコルバッファのファイルを作成してさえいれば、クライアント側とサーバ側で異なるプログラミング言語を使用していたとしても、プロトコルバッファの内容に基づいたコードを自動で生成してくれる機能が備わっています。サポートされている言語に関しては [こちら](https://grpc.io/docs/languages/) をご覧ください。

# 実装
- それでは実際に Kotlin での実装の説明に移りたいと思います。実装については大きく分けて以下の3つの工程に分けて説明したいと思います。
  - 必要なライブラリのインストールとコードの自動生成の設定
  - プロトコルバッファのファイルの作成及びコードの自動生成実行
  - 実装
- また今回の Spring プロジェクトについては [Spring Initializr](https://start.spring.io/) から作成しました。

## 必要なライブラリのインストールとコードの自動生成の設定

- まずはじめに必要なライブラリのインストールとプロトコルバッファのファイルから Kotlin のコードを自動生成する設定を行います。今回ビルドツールには gradle を採用しました。また、今回 gRPC サーバの実装には [LogNet/grpc-spring-boot-starter](https://github.com/LogNet/grpc-spring-boot-starter) を使用しました。以下に設定例を示します。

```kt:build.gradle.kts
import com.google.protobuf.gradle.*
import org.jetbrains.kotlin.gradle.tasks.KotlinCompile

plugins {
	id("org.springframework.boot") version "2.4.4"
	kotlin("jvm") version "1.4.30"
	kotlin("plugin.spring") version "1.4.30"
	id("com.google.protobuf") version "0.8.8"
	id("idea")
	id("java")
}

group = "jp.co.emperor.penguin"
version = "0.0.1-SNAPSHOT"
java.sourceCompatibility = JavaVersion.VERSION_11

repositories {
	mavenCentral()
}

dependencies {
	implementation("org.springframework.boot:spring-boot-starter-web")
	implementation("org.jetbrains.kotlin:kotlin-reflect")
	implementation("org.jetbrains.kotlin:kotlin-stdlib-jdk8")
	implementation(platform(org.springframework.boot.gradle.plugin.SpringBootPlugin.BOM_COORDINATES))
	implementation("com.google.protobuf:protobuf-java:3.6.1")
	implementation("io.grpc:grpc-stub:1.15.1")
	implementation("io.grpc:grpc-protobuf:1.15.1")
	implementation("io.github.lognet:grpc-spring-boot-starter:4.4.5")
}

val springBootVersion = "2.4.4"
configurations {
	all {
		resolutionStrategy.eachDependency {
			if (requested.group == "org.springframework.boot") {
				useVersion(springBootVersion)
			}
		}
	}
}

tasks.withType<KotlinCompile> {
	kotlinOptions {
		freeCompilerArgs = listOf("-Xjsr305=strict")
		jvmTarget = "11"
	}
}

tasks.withType<Test> {
	useJUnitPlatform()
}

protobuf {
	protoc {
		artifact = "com.google.protobuf:protoc:3.6.1"
	}
	plugins{
		id("grpc"){
			artifact = "io.grpc:protoc-gen-grpc-java:1.15.1"
		}
	}
	generateProtoTasks{
		ofSourceSet("main").forEach{
			it.plugins{
				id("grpc")
			}
		}
	}
}
```
- 今回の実装は [こちらのソースコード](https://github.com/google/protobuf-gradle-plugin/blob/master/examples/exampleKotlinDslProject/build.gradle.kts) を参考にしています。

## プロトコルバッファのファイルの作成及びコードの自動生成実行
- 次にプロトコルバッファのファイルを作成します。`src/main/proto` というディレクトリを作成し、以下のような内容のファイルを作成します。
```:Calculator.proto
syntax = "proto3";
option java_package = "jp.co.emperor.penguin.knowledge.proto";

service Calculator {
    rpc Add (AdditionRequest) returns (AdditionReply);
}

message AdditionRequest {
    int32 added = 1;
    int32 add = 2;
}

message AdditionReply {
    int32 result = 1;
}
```
- `syntax` 句にはプロトコルバッファのバージョンを指定しています。今回は最新バージョンである `proto3` を指定しています。
- `option java_package` 句は生成したコードが含まれるパッケージ名を指定しています。
- `service` 及び `message` 句に関してはプロトコルバッファの項で説明した内容と同じです。
- 次に gradle のタスクを実装してコードを自動生成します。ターミナルで以下のコマンドを実装します。
```sh
$ ./gradlew clean generateProto
...
BUILD SUCCESSFUL in 3s
```
- タスクが成功すると `build/generated/source/proto/main/grpc` 配下に先ほど作成したファイルの内容が反映されたソースコードが生成されます。
```sh
$ tree build/generated/source/proto/main/grpc 
build/generated/source/proto/main/grpc
└── jp
    └── co
        └── emperor
            └── penguin
                └── knowledge
                    └── proto
                        └── CalculatorGrpc.java
```
- 以降の実装はこの `CalculatorGrpc.java` を使用します。

## 実装
- コードの自動生成まで完了したので、実際に `Add` メソッドを実装していきます。実装例を以下に示します。
```kt:CalculatorService.kt
package jp.co.emperor.penguin.knowledge.service

import io.grpc.stub.StreamObserver
import jp.co.emperor.penguin.knowledge.proto.CalculatorGrpc
import jp.co.emperor.penguin.knowledge.proto.CalculatorOuterClass
import org.lognet.springboot.grpc.GRpcService

@GRpcService // (1)
class CalculatorService : CalculatorGrpc.CalculatorImplBase() {
    override fun add(
        request: CalculatorOuterClass.AdditionRequest?,
        responseObserver: StreamObserver<CalculatorOuterClass.AdditionReply>?
    ) {
        val replyBuilder = CalculatorOuterClass.AdditionReply.newBuilder().setResult((request?.added?.plus(request?.add)) ?: 0) // (2)
        responseObserver?.onNext(replyBuilder.build()) // (3)
        responseObserver?.onCompleted() // (4)
    }
}
```
- `(1)` で `@GRpcService` アノテーションをクラスに付与し `CalculatorGrpc.CalculatorImplBase()` を実装することによって、`Add` メソッドが呼ばれた場合に `CalculatorService` の `add` メソッドが呼ばれるようになります。`(2)` では足し算の処理を実装しています。引数が null の場合は計算結果は 0 となります。`(3)` でレスポンスに `(2)` の計算結果をセットしています。`(4)` で処理の終了をクライアントに通知します。
- また、動作確認で使用するツールからのリクエストを受け付けるために、 `application.yaml` に以下の設定を追加します。
```yaml:application.yaml
grpc:
  enableReflection: true
```
- 最後に以下の gradle のタスクを実行して gRPC サーバを起動します。以下のようなログが出力されれば成功です。
```sh
$ ./gradlew bootRun
...
[           main] o.l.springboot.grpc.GRpcServerRunner     : 'io.grpc.protobuf.services.ProtoReflectionService' service has been registered.
[           main] o.l.springboot.grpc.GRpcServerRunner     : gRPC Server started, listening on port 6565.
```

# 動作確認
- 実装が完了したので、実際にリクエストを実行して動作確認を行います。今回 gRPC API の動作確認は [fullstorydev/grpcurl](https://github.com/fullstorydev/grpcurl) を使用しました。まずはインストールを行います。macOS の場合は以下のコマンドを実行することでインストールが可能です。
```sh
brew install grpcurl
```
- 次に gRPC サーバが提供している Service の一覧を確認してみます。以下のコマンドを実行します。
```sh
$ grpcurl -plaintext localhost:6565 list
Calculator
grpc.health.v1.Health
grpc.reflection.v1alpha.ServerReflection
```
- 上記の結果より作成した `Calculator` が登録されていることが確認できました。さらに `describe` コマンドで Service の詳細を確認することが可能です。
```sh
$ grpcurl -plaintext localhost:6565 describe Calculator
Calculator is a service:
service Calculator {
  rpc Add ( .AdditionRequest ) returns ( .AdditionReply );
}
```
- `Calculator` は `AdditionRequest` を引数として `AdditionReply` を返す `Add` メソッドを提供しています。これは「実装」の項で説明した内容と一致しています。
- 最後にリクエストを実行し、想定通りの挙動となっているかの確認を行います。以下のようなコマンドを実行します。
```
$ grpcurl -plaintext -d '{"added": 1, "add": 2}' localhost:6565 Calculator/Add
{
  "result": 3
}
```
- `result` フィールドの値が3になっていることから、`added + add` という処理が実行されていることが確認できました。

# 参考文献
- https://blog.takehata-engineer.com/entry/server-side-kotlin-grpc-project-creation-to-start-confirmation
- https://boctoc1969.hatenablog.com/entry/2020/11/19/173321
- https://github.com/LogNet/grpc-spring-boot-starter
- https://github.com/fullstorydev/grpcurl
- https://github.com/grpc/grpc-java/blob/master/documentation/server-reflection-tutorial.md
- https://github.com/grpc/grpc/blob/master/doc/server-reflection.md