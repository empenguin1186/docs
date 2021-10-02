# はじめに
kubernetes に locust をデプロイして負荷テストを実施する方法について調べたので手順についてまとめようと思います。また今回はローカルの docker-desktop でテストを実施することとします。

# シナリオ
今回 locust を使用して負荷テストを実行する構成としては以下の図のようになります。locust を使用して負荷をかける Pod は master/worker 構成とし、それぞれが Pod としてデプロイされているテスト対象の アプリケーションサーバに対してパス `/hello` でリクエストを実行します。

# 実装

## コード構成
負荷テストを実行するためのコード構成は以下となります。なお構成に関しては[こちら](https://github.com/GoogleCloudPlatform/distributed-load-testing-using-kubernetes)を参考にしています。
```sh
$ tree .
.
├── image
│   ├── Dockerfile
│   └── test
│       ├── requirements.txt
│       ├── run.sh
│       └── test.py
└── manifest.yaml
```
`image` ディレクトリは負荷テスト実行のための Docker Image を作成するためのディレクトリになります。また `manifest.yaml` に関しては master/worker を構成するための設定、また負荷をかける側の Pod がテスト対象の Pod にリクエストするための Service の設定が含まれています。

## Docker Image の作成
`image` ディレクトリに含まれているファイルの内容は以下です。
```Dockerfile
FROM python:3.7.2
ADD test /test
RUN python -m pip install -r /test/requirements.txt
RUN chmod 755 /test/run.sh

ENTRYPOINT ["/test/run.sh"] 
```
```requirements.txt
locust == 1.6.0
```
使用するライブラリは `locust` のみです。
```sh:run.sh
#!/bin/bash

LOCUST="/usr/local/bin/locust"
LOCUS_OPTS="-f /test/test.py --host=$TARGET_HOST"

if [[ "$LOCUST_MODE" = "master" ]]; then
    LOCUS_OPTS="$LOCUS_OPTS --master"
elif [[ "$LOCUST_MODE" = "worker" ]]; then
    LOCUS_OPTS="$LOCUS_OPTS --worker --master-host=$LOCUST_MASTER"
fi

$LOCUST $LOCUS_OPTS
```
master か worker かで実行時に指定するオプションを変更するようにしています。
```py:test/test.py
from locust import HttpUser, task, between

class TestUser(HttpUser):

    @task
    def hello(self):
        self.client.get("/hello")
```
テストについてはシナリオの項で触れたように単に負荷対象の API に対してパス `/hello` で GET リクエストを実行するといったような内容となってます。<br>
最後に docker コマンドでイメージを作成しておきます。
```sh
$ docker build -t locust:latest ./image
```

## manifest.yaml の内容
manifest.yaml の内容は以下のようになっています。
```yaml:manifest.yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: locust-service
  name: locust-service
spec:
  ports:
  - name: master-web
    nodePort: 30060
    port: 8089
    protocol: TCP
    targetPort: master-web
  - name: master-p1
    nodePort: 30061
    port: 5557
    protocol: TCP
    targetPort: master-p1
  - name: master-p2
    nodePort: 30062
    port: 5558
    protocol: TCP
    targetPort: master-p2
  selector:
    app: locust-master
  type: NodePort
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    name: locust-master
  name: locust-master
spec:
  replicas: 1
  selector:
    matchLabels:
      app: locust-master
  template:
    metadata:
      labels:
        app: locust-master
    spec:
      containers:
      - env:
        - name: LOCUST_MODE
          value: master
        - name: TARGET_HOST
          value: http://knowledge-service
        image: locust:latest
        imagePullPolicy: IfNotPresent
        name: locust-master
        ports:
        - containerPort: 8089
          name: master-web
          protocol: TCP
        - containerPort: 5557
          name: master-p1
          protocol: TCP
        - containerPort: 5558
          name: master-p2
          protocol: TCP
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    name: locust-worker
  name: locust-worker
spec:
  replicas: 4
  selector:
    matchLabels:
      app: locust-worker
  template:
    metadata:
      labels:
        app: locust-worker
    spec:
      containers:
      - env:
        - name: LOCUST_MODE
          value: worker
        - name: LOCUST_MASTER
          value: locust-master
        - name: TARGET_HOST
          value: http://knowledge-service
        image: locust:latest
        imagePullPolicy: IfNotPresent
        name: locust-worker
```
`locust-master` に関してですが、master/worker 構成で負荷テストを実施する場合は 5557, 5558 ポートを使用することになります。8089 番ポートに関しては Web UI にアクセスするためのポートになります。また今回テスト対象の Pod にリクエストを実行するために `knowledge-service` という Service を別途デプロイしています。<br>
最後に `manifest.yaml` をデプロイします。
```sh
$ kubectl apply -f manifest.yaml
```

# 動作確認
`localhost:30060` にブラウザでアクセスすると Web UI が表示されます。適当に設定を行ってテストを実行していきます。

テスト実行中「Statistics」の項でリクエスト数やリクエストにかかった平均秒数、RPS などの実測値などをリアルタイムで確認することができます。

「Charts」では RPS の実測値、1秒あたりの失敗リクエスト数などをグラフで確認することができます。
