Kubernetes で運用する Spring Boot アプリケーションにおける Graceful Shutdown について

# はじめに
Kubernetes で運用する Spring Boot アプリケーションで Graceful Shutdown を行う方法についてまとめます。なお今回検証した Spring Boot のバージョンは `2.5.2` となります。

# Graceful Shutdown を実装する目的
Kubernetes でサービスを運用する場合は、Deployment の更新などの理由で頻繁に Pod 再作成および古い Pod の終了処理が行われます。この際にアプリケーションが Graceful Shutdown を実装していない場合、古い Pod がまだリクエストを処理している場合に終了処理が行われると、クライアントに正常なレスポンスを返すことができない事態が発生する可能性があります。このような事態を避けるため、アプリケーション側で Pod の終了処理が行われる際に接続しているリクエストを全て処理してからサーバの停止するといった実装が必要となってきます。

# 実装

## Graceful Shutdown
まずはじめにアプリケーションの Graceful Shutdown の機能を有効にします。有効にするためには Spring Boot の追加機能である Spring Boot Actuator を使用します。ビルドツールに gradle を使用している場合、以下の依存関係の設定を追加します。
```build.gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
}
```

次に `application.yaml` の設定を行います。Spring Boot アプリケーションでは専用のエンドポイントを叩くことで Graceful Shutdown を発動させますが、そのエンドポイントはデフォルトでは無効化されています。したがってエンドポイントを有効化および公開する設定を追加します。

```yaml:application.yaml
server:
  # シャットダウンのタイプを指定する(デフォルトは immediate)
  shutdown: graceful
management:
  endpoint:
    shutdown:
      enabled: true
  endpoints:
    web:
      exposure:
        include: "shutdown"
```

次に簡単なコントローラの処理を実装します。今回は重い処理を実現するためスリープ処理を入れています。

```kt:MailController.kt
package jp.co.emperor.penguin.knowledge.controller

import org.springframework.http.HttpStatus
import org.springframework.http.ResponseEntity
import org.springframework.web.bind.annotation.GetMapping
import org.springframework.web.bind.annotation.RequestMapping
import org.springframework.web.bind.annotation.RestController


@RestController
@RequestMapping( "/")
class MainController {

    @GetMapping("hello")
    fun get(): ResponseEntity<String> {
        Thread.sleep(10*1000L)
        return ResponseEntity<String>("Hello, World", HttpStatus.OK)
    }
}
```

実際に動作確認を行なったところ、Graceful Shutdown を有効にしているかどうかでレスポンスが変化することが確認できました。

```sh:Graceful Shutdown 無効時
$ curl -v localhost:8080/hello

*   Trying ::1...
* TCP_NODELAY set
* Connected to localhost (::1) port 8080 (#0)
> GET /hello HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.64.1
> Accept: */*
>
* Empty reply from server
* Connection #0 to host localhost left intact
curl: (52) Empty reply from server
```

```sh:Graceful Shutdown 有効時
$ curl -v localhost:8080/hello

*   Trying ::1...
* TCP_NODELAY set
* Connected to localhost (::1) port 8080 (#0)
> GET /hello HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.64.1
> Accept: */*
>
< HTTP/1.1 200
< Content-Type: text/plain;charset=UTF-8
< Content-Length: 12
< Date: Sat, 10 Jul 2021 15:53:33 GMT
<
* Connection #0 to host localhost left intact
Hello, World
```

## Kubernetes で運用する場合の設定
Spring Boot の Graceful Shutdown の挙動は以下となります。
1. Graceful Shutdown が発動したら新規のリクエストは受け付けなくなる
2. 接続中のリクエストが存在する場合は全て処理を完了させてからサーバを停止する
Kubernetes で Spring Boot アプリケーションを運用する場合は、`1.`について検討する必要があります。Service がまだ終了する Pod に対してルーティングを行うような設定になっている場合は、その Pod に対してまだ新規にリクエストが行われる可能性があるので、この時点で Shutdown が実行されると新規のリクエストに対してはエラーを返すことになってしまいます。したがって Service が終了する Pod に対してルーティングを行わなくなった後で Shutdown を実行する必要があります。この辺りの挙動に関しては [こちらの記事](https://qiita.com/superbrothers/items/3ac78daba3560ea406b2) が参考になります。<br>
これを踏まえて方針としては、「Pod の終了時には Service のルーティング対象から外れるまでスリープ処理を行い、その後に Shutdown のエンドポイントを叩いて Graceful Shutdown を実行する」という処理を実装することとします。Pod リソースには `preStop` フックと呼ばれる Pod の終了時に任意の処理を実行することができる設定を行うことができるので、その機能を用いて実装を行っていきます。具体的には以下のような設定となります。

```yaml:deployment.yaml
apiVersion: apps/v1
kind: Deployment
...
spec:
  ...
  template:
    ...
    spec:
      containers:
      - name: knowledge
        ...
        # preStop フックの設定
        lifecycle:
          preStop:
            exec:
              command: ["sh", "-c", "sleep 10; curl -X POST http://localhost:8080/actuator/shutdown"]
```

# まとめ
- Kubernetes における Spring Boot アプリケーションの Graceful Shutdown の実装についてまとめました。リリースが頻繁に行われるサービスに関しては適切にShutdown時の処理を実装していないとリリースの度にAPIのエラー率が増加することになってしまうので、Graceful Shutdown の機能を有効利用してより安定したサービスの運用を行っていきたいですね。

# 参考文献
- [Kubernetes: 詳解 Pods の終了](https://qiita.com/superbrothers/items/3ac78daba3560ea406b2)
- [コンテナライフサイクルイベントへのハンドラー紐付け | Kubernetes](https://kubernetes.io/ja/docs/tasks/configure-pod-container/attach-handler-lifecycle-event/)
- [コンテナライフサイクルフック | Kubernetes](https://kubernetes.io/ja/docs/concepts/containers/container-lifecycle-hooks/)
- [Spring Boot Actuator: 本番対応機能 - リファレンス](https://spring.pleiades.io/spring-boot/docs/current/reference/html/actuator.html)

