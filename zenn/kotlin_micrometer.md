# はじめに
- Spring Boot アプリケーションを Prometheus と連携させて監視設定を行い、アラートが発生した場合に特定の Slack チャンネルに通知を行う実装をおこなったので、手順についてまとめます。

# 全体像
- 監視システムの全体像については以下の図のようになります。まず、Spring Boot アプリケーションの方で Prometheus 形式のメトリクスを出力し、それを外部に公開する設定を行います。そして Prometheus 本体に Spring Boot アプリケーションのメトリクスを取得しにいくように設定を修正します。Prometheus 側で設定していた監視ルールに引っかかった場合には Alertmanager に対して通知が行われ、Alertmanager は通知先の Slack チャンネルに通知を行います。
![](https://storage.googleapis.com/zenn-user-upload/cjr8ycid7ia1mvnafouy1n59ky4h)

# 実装
- 続いて実装について説明します。今回言語は Kotlin を採用しました。

## Spring の設定

- まずは Spring Boot アプリケーションが Prometheus のメトリクスを外部に公開するような設定を行います。まず、メトリクスの公開に必要なライブラリをインストールします。`build.gradle.kts` に以下の設定を追加します。
```kt:build.gradle.kts
dependencies {
	implementation("org.springframework.boot:spring-boot-starter-web")
	implementation("io.micrometer:micrometer-registry-prometheus:1.6.6")
	implementation("org.springframework.boot:spring-boot-starter-actuator:2.4.5")
}
```
- 次に、`application.yaml` にメトリクスを公開するエンドポイントを有効にする設定を追加します。具体的には以下のような設定となります。
```yaml:application.yaml
management:
  endpoints:
    web:
      exposure:
        include: prometheus
```
- この設定を追加することで、`/actuator/prometheus` のパスでリクエストを実行すると、以下のような Prometeus 形式のメトリクスがレスポンスとして返ってきます。
```sh
$ curl localhost:8080/actuator/prometheus
# HELP system_load_average_1m The sum of the number of runnable entities queued to available processors and the number of runnable entities running on the available processors averaged over a period of time
# TYPE system_load_average_1m gauge
system_load_average_1m 3.78955078125
# HELP jvm_gc_max_data_size_bytes Max size of long-lived heap memory pool
# TYPE jvm_gc_max_data_size_bytes gauge
jvm_gc_max_data_size_bytes 0.0
# HELP system_cpu_usage The "recent cpu usage" for the whole system
# TYPE system_cpu_usage gauge
system_cpu_usage 2.6819144785064596E-8
# HELP jvm_buffer_count_buffers An estimate of the number of buffers in the pool
# TYPE jvm_buffer_count_buffers gauge
jvm_buffer_count_buffers{id="direct",} 9.0
jvm_buffer_count_buffers{id="mapped",} 0.0
...
```
- 次に検証用として以下の内容のコントローラを実装します。今回は意図的にステータスコードを400系に変えてアラートを発生させたいため、リクエストパラメータである `isSc4xx` の値が `true` の場合にクライアントにステータスコード400を返すようにしています。
```kt:MainController.kt
package jp.co.emperor.penguin.knowledge.controller

import org.springframework.http.HttpStatus
import org.springframework.http.ResponseEntity
import org.springframework.web.bind.annotation.GetMapping
import org.springframework.web.bind.annotation.RequestMapping
import org.springframework.web.bind.annotation.RequestParam
import org.springframework.web.bind.annotation.RestController

@RestController
@RequestMapping( "/")
class MainController {

    @GetMapping
    fun get(
        @RequestParam(name = "isSc4xx", defaultValue = "false", required = false) isSc4xx: Boolean
    ): ResponseEntity<String> {
        val httpStatus = if (isSc4xx) HttpStatus.BAD_REQUEST else HttpStatus.OK
        return ResponseEntity<String>("Hello, World", httpStatus)
    }
}
```

## Prometheus の設定
- 続いて Prometheus の設定について説明していきます。

### Prometheus 本体の設定
- まず Prometheus 本体が Spring Boot アプリケーションが出力するメトリクスをスクレイプするように設定を修正します。`prometheus.yml` に以下の設定を追記します。
```yaml:prometheus.yml
...
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      - localhost:9093

rule_files:
  - rules.yml

scrape_configs:
  ...

  - job_name: 'spring'
    metrics_path: '/actuator/prometheus'
    static_configs:
      - targets: ['localhost:8080']
```
- 今回は `localhost:8080` で Spring Boot アプリケーションを起動し、スクレイプを行う際のリクエストパスは `/actuator/prometheus` となるので、それを `prometheus.yml` に設定しています。また、Alertmanager は `localhost:9093` で起動し、`rules.yml` に記載されている内容を監視ルールとして設定しています。

### 監視ルールの設定
- 次に監視ルールの設定を行います。今回は一回でも Spring Boot アプリケーションが400系のステータスコードを返却した際にアラートを発生させるようなルールを設定します。先述した `rules.yml` に以下の内容を記述します。

```yaml:rules.yml
groups:
  - name: test
    rules:
      - alert: ErrorRate
        expr: (sum by(instance)(rate(http_server_requests_seconds_count{status=~"^4\\d\\d$",uri="/"}[5m])) / sum by(instance)(rate(http_server_requests_seconds_count{uri="/"}[5m])) * 100) > 0 #(1)
        for: 1m
        labels:
          severity: __severity__
        annotations:
          summary: "High Error Rate Detected"
          description: "error rate has exceeded the threshold with a value of {{ $value }}."
```
- `(1)` で具体的な監視ルールの設定を行っています。`http_server_requests_seconds_count`というメトリクスには、Spring Boot アプリケーションにリクエストが行われた際のリクエストパスや返却したステータスコードの情報がラベルとして含まれています。このラベルを用いて集計対象のメトリクスのフィルタリングを行っています。
- `rate`関数は対象のメトリクスが、設定した時間内(今回は5分)で1秒あたりどれくらい増加しているのかを算出してくれます。
- また、`by(instance)` とすることで、起動しているアプリケーションごとに監視ルールを適用することが可能となっています。

### アラートの設定
- 最後にアラートの設定です。`alertmanager.yml` に以下のような設定を行います。
```yaml:alertmanager.yml
global:
  slack_api_url: '${通知先の Slack チャンネルの Incomming Webhook URL}'

route:
  receiver: slack-notifications
  group_interval: 5s

receivers:
- name: 'slack-notifications'
  slack_configs:
  - channel: '${通知先の Slack チャンネル名}'
    text: |-
      {{ range .Alerts }}
      *Alert:* {{ .Labels.alertname }}
      *Summary:* {{ .Annotations.summary }}
      *Description:* {{ .Annotations.description }}
      *Details:*
      {{ range .Labels.SortedPairs }} *{{ .Name }}:* `{{ .Value }}`
      {{ end }}
      {{ end }}
    send_resolved: true
```

# 動作確認
- 監視の設定が完了したので、実際にアプリケーションにリクエストを実行してアラートが発生して通知が行われるかどうかの確認を行いました。リクエストは以下のように実行しました。
```sh
# 200系レスポンス
$ curl -v "localhost:8080"
...
< HTTP/1.1 200
< Content-Type: text/plain;charset=UTF-8
< Content-Length: 12
< Date: Fri, 30 Apr 2021 05:03:14 GMT
<
* Connection #0 to host localhost left intact
Hello, World* Closing connection 0

# 400系レスポンス
$ curl -v "localhost:8080?isSc4xx=true"
...
< HTTP/1.1 400
< Content-Type: text/plain;charset=UTF-8
< Content-Length: 12
< Date: Fri, 30 Apr 2021 05:03:12 GMT
< Connection: close
<
* Closing connection 0
Hello, World
```
- その後、Alertmanager のUIを確認すると、アラートが発生していることが確認できました。
![](https://storage.googleapis.com/zenn-user-upload/gapn3df2eiegzi0sprqciro44hq6)
- Slack のチャンネルにも以下のような通知が行われていることが確認できました。
![](https://storage.googleapis.com/zenn-user-upload/fe07fbupcklqjxba3sy6q6dditev)

# 資料
- [Configuration | Prometheus](https://prometheus.io/docs/alerting/latest/configuration/)
- [Spring Boot Actuator: 本番対応機能 - リファレンス](https://spring.pleiades.io/spring-boot/docs/current/reference/html/production-ready-features.html#production-ready-enabling)
- [Overview | Prometheus](https://prometheus.io/docs/introduction/overview/)