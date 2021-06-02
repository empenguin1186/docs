# はじめに
- Dockerfile の `ENTRYPOINT` および `CMD` の設定を Kubernetes のマニフェストファイルで書き換える方法について学んだので内容をまとめます。

# Dockerfile における ENTRYPOINT, CMD
- Dockerfile における `ENTRYPOINT` および `CMD` には、どちらもコンテナ起動時に実行するコマンドを指定しますが、基本的にはコンテナ起動時に実行されるコマンドの基本形を `ENTRYPOINT` に定義し、`CMD` に `ENTRYPOINT` で定義したコマンドに付加するオプションを指定するといった使い方がされるようです。例えば、以下のような内容の Dockerfile があったとします。
```dockerfile:Dockerfile
FROM ubuntu
ENTRYPOINT ["top", "-b"]
CMD ["-c"]
```
- これをビルドして実行します。すると `top -b -c` というコマンドが実行されていることが確認できます。
```sh
$ docker build -t top:latest .
...

$ docker images
REPOSITORY                                  TAG                                                     IMAGE ID       CREATED          SIZE
top                                         latest                                                  87b9634a8f3b   45 seconds ago   72.7MB
...

$ docker run -itd --rm --name test  top:latest
3e18bd763af14b484536ca3a552ef184ea5675233d71b21706dc18f50271dbd0

$ docker exec -it test ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.3  0.1   5976  3148 pts/0    Ss+  15:38   0:00 top -b -c
root         9  0.0  0.1   5904  2792 pts/1    Rs+  15:38   0:00 ps aux
```
- `CMD` に指定したオプションはコンテナ起動時に変更することが可能です。例えば以下のようにオプションを指定すると、`top -b -H` というコマンドが実行されていることが確認できます。
```sh
$ docker run -itd --rm --name test  top:latest -H
0cda47656f04f127d66c1d9462f13ce989c77cd6bd7b1f49ecc80b60086058f1
$ docker exec -it test ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.3  0.1   5976  3060 pts/0    Ss+  15:40   0:00 top -b -H
root         7  0.0  0.1   5904  2744 pts/1    Rs+  15:40   0:00 ps aux
```
- また、`ENTRYPOINT` は基本的に書き換えが発生しないコマンドを指定しますが、書き換えを行いたい場合は `--entrypoint` オプションを使用することで書き換えを行うことができます。
```sh
$ docker run -itd --rm --name test --entrypoint top top:latest -H
732428876aa2f57388f1660baf92fc50236aadee66d449dbff3ce086808392af
$ docker exec -it test ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  1.5  0.1   6108  3192 pts/0    Ss+  15:44   0:00 top -H
root         7  0.0  0.1   5904  2772 pts/1    Rs+  15:44   0:00 ps aux
```

# Kubernetes のマニフェストファイルによる書き換え
- これまで Dockerfile の `ENTRYPOINT` と `CMD` について説明してきました。Kubernetes のマニフェストファイルではこれらの設定の書き換えを行うことが可能です。`ENTRYPOINT` は `command`, `CMD` は `args` という項目に対応しています。今回は `args` を設定して `CMD` で指定したオプションが書き換えられているかどうかを確認します。まず以下のような Dockerfile を作成します。

```dockerfile:Dockerfile
FROM openjdk:11

WORKDIR /app
COPY ./build/libs/knowledge-0.0.1-SNAPSHOT.jar /app

ENTRYPOINT ["java", "-jar", "knowledge-0.0.1-SNAPSHOT.jar"]
CMD ["-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=8082"]
```
- コンテナを起動すると、`CMD` に指定した設定がオプションとして追加されていることが確認できます。
```sh
$ docker build -t knowledge:latest .
...

$ docker run -itd --rm --name knowledge knowledge:latest                      
d0db59e36aff7f43b003f1c382f48dfd87cb39f3104086566d92e56478cebd33

$ docker exec -it knowledge ps aux                                             
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1 69.5 10.1 3132584 207312 pts/0  Ssl+ 16:15   0:15 java -jar knowledge-0.0.1-SNAPSHOT.jar -agentlib:jdwp=transport=dt_socket,server=y,susp
root        44  0.0  0.1   9396  2876 pts/1    Rs+  16:15   0:00 ps aux
```
- 次に下記のようなマニフェストファイルを作成します。`args` の項目を設定しています。
```yaml:deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: knowledge
  name: knowledge-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: knowledge
  template:
    metadata:
      labels:
        app: knowledge
    spec:
      containers:
      # ここ
      - args: ["-Xms256m", "-Xmx256m"]
        image: knowledge:latest
        # ローカルのイメージを使用するように設定
        imagePullPolicy: IfNotPresent
        name: knowledge
        ports:
        - containerPort: 8080
```
- `kubectl` コマンドで deployment をデプロイします。その後実行コンテナのシェルを取得し、コマンドオプションを確認します。結果、オプションが `-Xms256 -Xmx256m` となっており、`CMD` の設定が書き換わっていることが確認できました。
```sh
$ kubectl apply -f deployment.yaml          
deployment.apps/knowledge-deployment created

$ kubectl get pod
NAME                                    READY   STATUS    RESTARTS   AGE
knowledge-deployment-6c5f894f77-bzqnw   1/1     Running   0          44s

$ kubectl exec --stdin --tty knowledge-deployment-6c5f894f77-bzqnw -- /bin/bash
root@knowledge-deployment-6c5f894f77-bzqnw:/app# ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1 19.9  8.2 3072188 168108 ?      Ssl  16:08   0:13 java -jar knowledge-0.0.1-SNAPSHOT.jar -Xms256 -Xmx256m
root        36  0.0  0.1   5756  3492 pts/0    Ss   16:09   0:00 /bin/bash
root        42  0.0  0.1   9396  2908 pts/0    R+   16:09   0:00 ps aux
```

# まとめ
- Dockerfile における `ENTRYPOINT` と `CMD` の役割、そして対応する Kubernetes のマニフェストファイルの設定について学びました。個人的には Kubernetes でサービスを運用する場合は、docker のビルド回数を最低限に抑えるために、公式が紹介している方針と同じく `ENTRYPOINT` に基本的な実行コマンドを定義し、適宜環境に応じてマニフェストファイルの `args` の項目を設定するのが良いのかなと感じました。

# 資料
- [Dockerfile リファレンス — Docker-docs-ja 19.03 ドキュメント](https://docs.docker.jp/engine/reference/builder.html)
- [Define a Command and Arguments for a Container | Kubernetes](https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/)