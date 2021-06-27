# はじめに
- Docker Desktop で立ち上げた kubernetes で prometheus, alertmanager, grafana をデプロイしてアプリケーションの監視フローを構築する手順についてまとめます。

# 全体像
- 今回構築するシステムの概要は以下になります。
![](https://storage.googleapis.com/zenn-user-upload/9ee1aa9d55654bc133b03827.png)

# 設定

## アプリケーションサーバ

### アプリケーションサーバの実装
- まずメトリクスを取得する対象であるアプリケーションサーバの実装を行います。アプリケーションサーバの実装は Go の Web フレームワークである echo を用いて行います。実装例を以下に示します。

```go:main.go
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
		Name: "api_request_total",
		Help: "The total number of api called",
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
- 内容としては `/hello` というパスに対してリクエストが実行された場合に `api_request_total` というメトリクスの値をインクリメントし、クライアントには `Hello, World!` というレスポンスを返すという単純なものです。また prometheus が使用するメトリクスについては `/metrics` というパスに対してリクエストを実行すれば取得することができます。

### アプリケーションサーバのデプロイ
- 続いてアプリケーションサーバを kubernetes で起動するための deployment のマニフェストファイルを作成します。設定例を以下に示します。
```yaml:echo-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: echo-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: echo
  template:
    metadata:
      name: echo
      labels:
        app: echo
    spec:
      containers:
      - name: echo-server
        image: echo_server:latest
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 8081
```
- `image` に設定している `echo_server:latest` イメージは先ほど作成したアプリケーションサーバを起動する DockerImage になります。

### サービスのデプロイ
- 今回アプリケーションサーバの Pod と prometheus の Pod は別々になるので、prometheus の Pod からアプリケーションサーバに対してリクエストを行うことができるようにサービスの設定を行う必要があります。内部からのリクエストのみに使用されるので、Service のタイプは `ClusterIP` となります。
```yaml:echo-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: echo-service
spec:
  selector:
    app: echo
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8081
  type: ClusterIP
```
- これでアプリケーションサーバ側の設定は完了です。

## prometheus, alertmanager, grafana の設定
- 続いて prometheus, alertmanager, grafana の設定を行います。今回はこれら3つのツールを同じ Pod で稼働させるように deployment の設定を行います。deployment の設定例は以下となります。
```yaml:prometheus-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: prometheus-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: prometheus
  template:
    metadata:
      name: prometheus
      labels:
        app: prometheus
    spec:
      containers:
      - name: prometheus
        image: prom/prometheus:v2.27.1
        args:
        - --config.file=/etc/prometheus/prometheus.yaml
        ports:
        - containerPort: 9090
        volumeMounts:
        - name: config-volume
          mountPath: /etc/prometheus
      - name: alertmanager
        image: prom/alertmanager:v0.22.2
        args:
        - --config.file=/etc/alertmanager/alertmanager.yaml
        ports:
        - containerPort: 9093
        volumeMounts:
        - name: alertmanager-volume
          mountPath: /etc/alertmanager
      - name: grafana
        image: grafana/grafana:7.5.9
        env:
          - name: GF_SECURITY_ADMIN_USER
            valueFrom:
              secretKeyRef:
                name: grafana-secret
                key: username
          - name: GF_SECURITY_ADMIN_PASSWORD
            valueFrom:
              secretKeyRef:
                name: grafana-secret
                key: password
        volumeMounts:
          - name: datasource-volume
            mountPath: /etc/grafana/provisioning/datasources
            readOnly: true
      volumes:
      - name: config-volume
        configMap:
          name: prometheus-config
      - name: alertmanager-volume
        secret:
          secretName: alertmanager-secret
      - name: datasource-volume
        configMap:
          name: grafana-config
```
- マニフェストファイルを確認すると、prometheus は `prometheus-config`、alertmanager は `alertmanager-secret`、grafana は `grafana-config` と `grafana-secret` の内容を参照しています。これからそれぞれのリソースにどのような設定を行っているのかを説明します。

### prometheus-config
- `prometheus-config` には prometheus 本体の設定とアラートルールの設定が含まれています。具体的には以下のような内容です。
```yaml:prometheus-config.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: prometheus-config
data:
  # prometheus 本体の設定
  prometheus.yaml: |
    global:
      scrape_interval:     15s
      evaluation_interval: 15s

    alerting:
      alertmanagers:
      - static_configs:
        - targets:
          - localhost:9093

    rule_files:
      - /etc/prometheus/rules.yaml

    scrape_configs:
      - job_name: 'prometheus'
        static_configs:
        - targets: ['localhost:9090']
      - job_name: 'echo'
        metrics_path: '/metrics'
        static_configs:
        - targets: ['echo-service:80']
  
  # アラートルールの設定
  rules.yaml: |
    groups:
      - name: test
        rules:
          - alert: DeadMansSwitch # アラート名
            expr: vector(1)
            for: 1m
            labels:
              severity: __severity__
            annotations:
              summary: テストアラート
              description: このアラートは常に発生します
```
- `prometheus.yaml` の `alerting` では通知先である alertmanager の設定、`rule_files` ではアラートルールの設定を行っているファイルのパス、そして `scrape_configs` ではメトリクスの取得対象の設定を行っています。また `rules.yaml` には具体的なアラートルールの設定が含まれています。

### alertmanager-secret
- `alertmanager-secret` の内容は以下のようになっています。
```yaml:alertmanager-secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: alertmanager-secret
type: Opaque
data:
  alertmanager.yaml: ${Base64 エンコードされた文字列}
```
- `alertmanager-secret` は Secret リソースなので `data` に設定する値は Base64 エンコードを行う必要があります。エンコード前の設定内容としては以下のようになります。
```yaml:エンコード前のalertmanager.yaml
global:
  slack_api_url: '${通知先 Slack チャンネルの Incoming Webhook URL }'

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

### grafana-config, grafana-secret の設定
- 最後に grafana です。まずは grafana で prometheus が集計しているデータを参照できるようにデータソースの設定を行う必要があります。この設定は `grafana-config` に含まれています。
```yaml:grafana-config.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: grafana-config
data:
  prometheus-datasource.yaml: |
    apiVersion: 1
    datasources:
      - name: Prometheus
        type: prometheus
        url: http://localhost:9090
        isDefault: true
```
- 今回 prometheus は同じ Pod の9090番ポートで起動しているので `http://localhost:9090` でアクセスすることが可能です。
- 続いて grafana にログインする際に必要なユーザ名とパスワードの設定を行います。こちらは情報の機密性から Secret リソースである `grafana-secret` で定義しています。
```yaml:grafana-secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: grafana-secret
type: Opaque
data:
  username: YWRtaW4=
  password: YWRtaW4=
```
- `username` と `password` に設定する値は自由です。今回はどちらも `admin` としています。

# 動作確認
- 設定が完了したので、全てのリソースをデプロイした後動作確認を行いました。今回 Ingress の設定を行っていなので動作確認は適宜ポートフォワードを行って行いました。

## prometheus
- まず prometheus でアプリケーションサーバのメトリクスが取得できるかどうかの確認を行いました。prometheus のUIからアプリケーション側で定義した `api_request_total` を検索したところ、メトリクスが取得できていることを確認しました。
![](https://storage.googleapis.com/zenn-user-upload/4960c24295e3e10c1986c943.png)

## alertmanager
- デプロイ後アラート通知が設定した Slack のチャンネルに投稿されることを確認しました。
![](https://storage.googleapis.com/zenn-user-upload/a0e3cc3e66dd9cf9438dea1a.png)

## grafana
- prometheus で集計しているメトリクスを grafana から参照できるかどうか確認しました。ダッシュボードの作成画面の Metrics の項目でアプリケーション側で定義した `api_request_total` を入力したところ、値が取得できていることを確認できました。
![](https://storage.googleapis.com/zenn-user-upload/37b02d27fae03e992a4e159b.png)

# まとめ
- kubernetes における prometheus, alertmanager, grafana の連携方法についてまとめました。実行環境が Docker Destop なので GKE や Amazon EKS で構築しているシステムに関してはこの通りにいかないかもしれませんが、とりあえず自分がやりたかったことはこの設定で実現することができました。今後は grafana のダッシュボードの設定方法などを調査してより有益な監視フローを構築していきたいと思います。