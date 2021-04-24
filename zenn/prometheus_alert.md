# はじめに
- Prometheus でサービスの監視を行う際、アラートが発生した場合にその内容を Slack に通知する手順について学んだので内容をまとめます。

# 登場人物
- 今回 Prometheus による監視を行うにあたりどのようなプラットフォームを使用するのかを以下の図に示します。まず監視したいサービスやマシンのメトリクスを取得する役割を Prometheus が担います。それに加え、監視ルールの設定や監視結果を通知するといったことも行っています。実際にサービスを運用しているメンバーにアラートを通知する役割は Alertmanager が担っており、Alertmanager は Prometheus から送られてきた監視結果の通知を受け取り、設定したルールに基づいて通知するメンバーを決定し、メールや Slack に通知するといったことを行っています。その他の関連プラットフォームとして、監視対象のマシンの CPU 使用率、メモリといったある特定の種類のメトリクスを Prometheus に提供してくれる Exporter や、Prometheus が取得したメトリクスを可視化するツールである Grafana などが存在しています。[Prometheus 公式ドキュメント](https://prometheus.io/docs/introduction/overview/) にもシステム構成についての説明が記載されているので、具体的な内容を学びたい方は見てみてください。
![](https://storage.googleapis.com/zenn-user-upload/jfbakjfi5zzcnqql9vcslg15k4qi)

# 手順
## シナリオ
- 今回は Prometheus 側で必ずアラートが発生するような監視ルールを設定し、そのアラートが Alertmanager のUIから確認できるのか、さらに指定した Slack のチャンネルにアラート内容が投稿されるのかの検証を行いました。また前提として Prometheus および Alertmanager はローカルの PC(Mac) で起動することとします。

## ダウンロード
- Prometheus、および Alertmanager は[こちらのリンク](https://prometheus.io/download/)から最新版をダウンロードすることができます。以降の説明でダウンロードを行う際にはこのサイトからダウンロードを行います。

## Prometheus の設定
- まずはじめに Prometheus の設定を行います。Prometheus をダウンロード(今回自分がダウンロードしたのは prometheus-2.26.0.darwin-amd64.tar.gz)して解凍した後、生成されたディレクトリに移動すると `prometheus.yml` という yaml ファイルが存在しているので、以下のように記述します。
```yaml:prometheus.yml
global: # (1)
  scrape_interval:     15s
  evaluation_interval: 15s

alerting: # (2)
  alertmanagers:
  - static_configs:
    - targets:
      - localhost:9093

rule_files: # (3)
  - rules.yml

scrape_configs: # (4)
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090']
```
- 設定は大まかに4つのパートから構成されています。各パートの設定内容については以下となります。

|  番号  |  説明  |
|:--:|:---|
|  (1)  |  共通設定。`scrape_interval` は監視対象からメトリクスを取得(スクレイプ)する間隔で、`evaluation_interval` は監視ルールを適用する間隔を表す。 |
|  (2)  |  Alertmanager の設定。今回 Alertmanager は `localhost:9093` で起動するので、`targets`フィールドにその設定を記述する。  |
|  (3)  |  監視ルールの設定。今回は同じディレクトリ階層の `rules.yaml` に含まれる監視ルールを適用する。 |
|  (4)  |  監視対象の設定。今回は Prometheus 自身を監視対象とするので、起動している `localhost:9000` を設定している。 |

- 続いて監視ルールの設定を行います。`prometheus.yml` と同じディレクトリ階層に以下の内容の `rules.yml` というファイルを作成します。

```yaml:rules.yml
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
- 主な項目の説明は以下となります。

|  項目  |  説明  |
|:--:|:---|
|  expr  | 具体的に監視ルールを表現する評価式。この評価式の評価結果が true か false かでアラートが発生するかどうかが決定される。今回は必ずアラートが発生するような設定を行っている。 |
|  for  |  どれくらいの期間アラートが発生し続けた場合に通知を行うかという設定。今回は1分間アラートが発生し続けた場合に通知が行われる。 |
|  labels  |  アラートに付与するラベル。 |
|  annotations  |  発生したアラートの説明。今回は Slack に通知する際にここで設定された値を使用する。|

- 上記の設定を行った後、以下のコマンドを実行します。
```sh
$ ./prometheus
```

## Alertmanager の設定
- 次に Alertmanager の設定を行います。Alertmanager をダウンロード(今回自分がダウンロードしたのは alertmanager-0.21.0.darwin-amd64.tar.gz)して解凍した後、生成されたディレクトリに移動すると `alertmanager.yml` という yaml ファイルが存在しているので、以下のように記述します。

```yaml:alertmanager.yml
global:
  slack_api_url: '${通知先 Slack チャンネルの Incoming Webhook URL }'

route:
  receiver: 'slack-notifications' # (1)

receivers:
- name: 'slack-notifications' # (2)
  slack_configs:
  - channel: '${通知先の Slack チャンネル名}'
    title: "{{ range .Alerts }}{{ .Annotations.summary }}\n{{ end }}" # (3)
    text: "{{ range .Alerts }}{{ .Annotations.description }}\n{{ end }}"
```
- (1) の `route` 句でデフォルトの通知先を設定しています。今回は (2) で設定している `slack-notifications` という通知先をデフォルトとしています。また、Prometheus から複数のアラートが通知されることを想定して、(3)では `range` を使用して通知されたアラートの内容を全て投稿するような設定を行っています。
- 上記の設定を行った後、以下のコマンドを実行します。
```sh
$ ./alertmanager
```

# 実行結果
- まずは UI でアラートが発生しているかどうかの確認を行います。`http://localhost:9093/` にブラウザでアクセスすると、以下のような画面が表示され、`rules.yml` で設定したアラートが通知されていることが確認できます。
![](https://storage.googleapis.com/zenn-user-upload/x6g3op0b55b30lia99ghgy9l9qj4)

- 次に Slack チャンネルにアラート内容が通知されているかどうかの確認を行いました。結果通知先に設定したチャンネルに以下のような投稿がされていることを確認できました。
![](https://storage.googleapis.com/zenn-user-upload/5mpadb0qb3f1vgq8mkusib7bh5cm)

