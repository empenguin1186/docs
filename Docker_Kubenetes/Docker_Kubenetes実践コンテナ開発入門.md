<!-- TOC -->

- [Docker](#docker)
  - [Docker とは](#docker-とは)
  - [仮想コンテナ技術](#仮想コンテナ技術)
  - [Docker の利点](#docker-の利点)
  - [チュートリアル](#チュートリアル)
  - [Docker を使用する意義](#docker-を使用する意義)
    - [Infrastructure as code と Immutable Infrastructure](#infrastructure-as-code-と-immutable-infrastructure)
  - [Docker Compose](#docker-compose)
  - [Docker Swarm](#docker-swarm)
  - [Dockerfile について](#dockerfile-について)
  - [Docker ビルド](#docker-ビルド)
  - [Docker 起動 & 停止 & 再起動 & 破棄](#docker-起動--停止--再起動--破棄)
  - [ポートフォワーディング](#ポートフォワーディング)
  - [名前付きコンテナ](#名前付きコンテナ)
  - [よく使うコマンドオプション](#よく使うコマンドオプション)
  - [起動しているコンテナに対して実行できるコマンド](#起動しているコンテナに対して実行できるコマンド)
    - [exec](#exec)
    - [cp](#cp)
  - [使用していないコンテナ、イメージの一括破棄](#使用していないコンテナイメージの一括破棄)
- [Kubernetes](#kubernetes)
  - [Kubernetes の概念](#kubernetes-の概念)
  - [KubernetesクラスタとNode](#kubernetesクラスタとnode)
    - [用語整理](#用語整理)
    - [Kubernetesクラスタ](#kubernetesクラスタ)
    - [Node](#node)
    - [Masterサーバにデプロイされる管理コンポーネント](#masterサーバにデプロイされる管理コンポーネント)
  - [Namespace](#namespace)
  - [Pod](#pod)
  - [ReplicaSet](#replicaset)
  - [Deployment](#deployment)
- [Service について](#service-について)
  - [Cluster IP Service](#cluster-ip-service)
  - [NodePort Service](#nodeport-service)
  - [ExternalName Service](#externalname-service)

<!-- /TOC -->

# Docker
## Docker とは
コンテナ型仮想化技術を実現するために実行される常駐アプリケーション(dockerd)と、それを操作するためのCLIからなるプロダクトです。  
具体的な例を挙げると、従来では Apache や Nginx などの Webサーバを立ち上げる時には、動作環境と同じOSを検証環境にインストールし、パッケージマネージャで必要なパッケージをインストールしてさらにいくつかの手順を踏まなければなりませんでしたが、Docker を使用すると設定ファイルを書き換えてコマンド一つ実行するだけで環境構築を完了させることができます。さらに、他の仮想化ソフトウェアと比較すると、軽量に動作し、優れたポータビリティをもつ。すなわち自身のローカル環境で作成したコンテナを、別のホストのDocker環境にデプロイすることが可能です。

## 仮想コンテナ技術
Docker は仮想コンテナ技術を利用しています。仮想コンテナ技術は、ホストOSのリソースを隔離して仮想OSを作り出します(ゲストOS)。この仮想OSをコンテナと呼びます。

## Docker の利点
Docker は Dockerfile というファイルによって作成するコンテナの構成を定義することができるので再現性が高いです。さらに、Docker による環境構築は OS とアプリケーションをセットにして行うので、簡単に環境構築を行うことが可能となります。

## チュートリアル

簡単なチュートリアルをやってみましょう！以下のスクリプトを実行するコンテナをビルドします。
```
!#/bin/sh

echo "Hello, World!"
```

以下のDockerfileを作成してコンテナの元となるイメージを作成します。この作業のことを Docker イメージをビルドすると呼びます。
```
FROM ubuntu: 16.04

COPY helloworld /usr/local/bin
RUN chmod +x /usr/local/bin/helloworld

CMD ["helloworld"]
```
イメージをビルドします。
```
$ docker image build -t helloworld:latest .
```
ビルドが成功したら実際にアプリケーションを実行します。
```
$ docker container run helloworld:latest
```
まとめると、開発からアプリケーションの実行までの流れとしては以下のようになります。  
- アプリケーションコードや dockerfile を編集する  
- `docker image build` 実行  
- `docker container run` 実行  

## Docker を使用する意義
Docker を利用する意義としては以下のようなことが挙げられます。  
- 環境構築における冪等性を確保できる  
- アプリケーションコードだけでなく、システムの構成もコード化できる  
- 実行環境とアプリーケーションが一体になったことによってポータビリティが向上する  
- システムを構成するアプリケーションやミドルウェアの構成管理が容易となる  

### Infrastructure as code と Immutable Infrastructure
Infrastructure as code ... インフラ構築をコード化する考え方です。Chef や Ansible を使ったインフラ構築はこれに該当します。  
Immutable Infrastructure ... ある地点のサーバの状態を保存しておき、複製可能にしておく考え方です。サーバに変更を加える場合は、既存のサーバに対して変更を加えるのではなく、新しく作り直してイメージとして保存し複製できるようにします。これによって一度セットアップしたサーバは手をつけずに破棄されるため、冪等性をきにする必要はありません。

## Docker Compose
Docker によりアプリケーションのデプロイは容易になりましたが、ある一定の規模のシステムを機能させるには、複数のアプリケーションやミドルウェアを組み合わせることが必要となってきます。複数のコンテナの依存関係を解決して一つのアプリケーションを機能させることはたとえDockerであっても困難なことです。そういった課題を解決するために開発されたツールが Docker Compose です。Docker Compose は yaml 形式で実行するコンテナを定義したり依存関係を定義して起動順を制御できます。

## Docker Swarm
Docker や Docker Compose で容易にアプリケーションを対象のサーバにデプロイすることが可能となりました。しかし、これは単一のサーバに対しての作業です。業務では複数のサーバに対して同じアプリケーションをデプロイしたいという需要が生まれることがあります。その需要を満たすのが Docker Swarm  です。Docker Swarm は複数のコンテナの管理を容易にします。また、コンテナの管理だけでなく、コンテナの増減、ノードのリソースを効率的に利用するためのコンテナの配置や負荷分散機能等の実務的な機能が用意されています。また、ローリングアップデート(新旧のコンテナを用意しておいて、段階的にデプロイを行う)機能も用意されています。このように多くのコンテナ群を管理する手法をコンテナオーケストレーションと呼びます。  

## Dockerfile について

```docker
FROM golang:1.9

RUN mkdir /echo
COPY main.go /echo

CMD ["go", "run", "/echo/main.go"]
```
各用語の意味は以下の通りです。
| 用語 | 機能 |
|:----:|:------|
| RUN  | 作成するDockerイメージのベースとなるイメージを指定する。今回は Go のランタイムが必要となるのでランタイムがインストールされているイメージを指定している。1.9はタグと呼ばれ、イメージのバージョンを固定するときに使用する。 | 
| RUN | イメージをビルドする際にコンテナ内で実行するコマンドを定義する。 |
| COPY | Docker を起動させているホストマシンに存在するファイルやディレクトリをコンテナ内にコピーする。　｜
| CMD | コンテナ起動時に実行されるプロセスを指定する。RUNとの違いは、RUNはアプリケーションの更新や配置を行うのに対し、CMD はアプリケーション自体の実行を行う |

## Docker ビルド

ビルドを行う場合は以下のコマンドを実行します。
```
$ docker image build -t <名前空間>/<イメージ名>:<タグ名> <Dockerfile 配置ディレクトリのパス>
```
名前空間を指定するのはイメージ名が衝突することを避ける目的です。

## Docker 起動 & 停止 & 再起動 & 破棄

コンテナを起動するときは以下のコマンドを実行します。
```
$ docker container run -d <名前空間>/<イメージ名>:<タグ名> 
```
`-d`オプションをつけることによりコンテナをバックグラウンドで実行することができます。実行すると、標準出力にコンテナのIDが表示されます。  
コンテナを終了するときは以下のコマンドを実行します。
```
$ docker container stop <コンテナ名またはコンテナID>
```
コンテナを再起動するときは以下のコマンドを実行します。
```
$ docker container restart <コンテナ名またはコンテナID>
```

コンテナを破棄するときは以下のコマンドを実行します。
```
$ docker container rm <コンテナ名またはコンテナID>
```

## ポートフォワーディング

コンテナ内のアプリケーションが公開しているポート番号は、コンテナ内だけで完結しているために外部からのリクエストはそのポート番号に対しては受け付けていません。コンテナ内のアプリケーションが公開しているポート番号とホストOS側のポート番号を結びつけるには、コンテナ起動時に`-p`オプションをつける必要があります。
```
$ docker container run -d -p 9000:8080 <コンテナ名>
```
この場合、ホスト側の9000番ポートとコンテナ内の8080番ポートが結びついています。

## 名前付きコンテナ

コンテナを停止する時などはコンテナ名かコンテナIDを指定する必要がありますが、`--name`オプションをつけることにより実行するコンテナの名前を自分で設定することができます。以下はその例です。

```
$ docker container run -t -d --name hoge example/echo:latest
```
`docker container ls` で確認してみると、NAME のカラムに自分の設定した名前が表示されていることが確認できます。

## よく使うコマンドオプション

| オプション　| 意味 |
|:---------:|:----|
| -i | docker 起動後にコンテナ側の標準入力を繋ぎっぱなしにする。このためシェルに入ってコマンド実行などができる。|
| -t | 擬似端末を有効にする。 -i がつかないと擬似端末を有効にしても入力ができないので、 -it のセットで使われる。 |
| -rm | コンテナ終了時にコンテナを破棄する。 |
| -v | ホストとコンテナ間でディレクトリやファイルを共有する。 |

## 起動しているコンテナに対して実行できるコマンド

### exec

起動しているコンテナに対してコマンドを実行できます。
```
$ docker container exec <コンテナ名> <コマンド(例: pwd)>
```

### cp

起動しているコンテナとホストOS間でファイルの受け渡しができます。
```
$ docker container cp <コンテナIDまたはコンテナ名>:<コピーするファイルのパス> <ホストのコピー先のパス>
```
`<コンテナIDまたはコンテナ名>:<コピーするファイルのパス>` と `<ホストのコピー先のパス>` は入れ替えることもできます。その場合、ホストのファイルをコンテナにコピーするという挙動となります。

## 使用していないコンテナ、イメージの一括破棄

`docker container prune`コマンドを使用すると起動していないコンテナの一括破棄が可能です。
```
$ docker container prune <オプション>
```
また、コンテナだけでなくイメージも一括破棄が可能です。
```
$ docker image prune <オプション>
```
また、利用されていないコンテナやイメージ、ボリューム、ネットワークといった全てのリソースを一括で削除したい場合は `docker system prune` コマンドを実行します。

```
$ docker system prune
```

# Kubernetes

Kubernetesとは Google が主導で開発されたコンテナの運用を自動化するためのコンテナオーケストレーションシステムです。特徴としては以下の通りです。
- コンテナを用いたアプリケーションのデプロイ
- 様々な運用管理の自動化  
- Dockerホストの管理  
- サーバリソースの空き具合を考慮したコンテナ配置やスケーリング  
- 複数のコンテナ群へのアクセスの取りまとめ(ロードバランサー)  
- 死活監視  

Kubernetes の立ち位置としては Docker Swarm とほぼ同じである。


ダッシュボード: Kubernetesにデプロイされているコンテナ等を確認できる管理ツール

デプロイ方法

```
## デプロイ
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.8.3/src/deploy/recommended/kubernetes-dashboard.yaml
```

## Kubernetes の概念
Kubernetesにおけるリソースとはアプリケーションのデプロイに際に必要となる部品のようなもので、以下のような要素のことを指します。なお、コンテナとリソースは別の粒度となります。
- Node  
- Namespace  
- Pod
リソースは上記を含めて20種類以上存在します。

## KubernetesクラスタとNode

### 用語整理

|用語|意味|
|:---:|:---|
|Kubernetesクラスタ|Kubernetesの様々なリソースを管理する集合体|
|Node|Kubernetesのクラスタの管理下に登録されているDockerホスト。Kubernetesでコンテナをデプロイするときに使われる。全体を管理するMasterとそれ以外のNodeに分かれる。|

クラスタに配置されているNodeの数とマシンスペックによって配置できるコンテナの数は変わってきます。

```
## トークン取得
$ kubectl -n kube-system get secret | grep deployment-controller
$ kubectl -n kube-system describe secret deployment-controller-<xxxxxxxxx>
## 表示されるトークンをコピー
```
namespace が "kube-system"のresource"secret"を取得するコマンド
namespaceについて
https://cloud.google.com/blog/products/gcp/kubernetes-best-practices-organizing-with-namespaces にある記述は以下の通り

```
You can think of a Namespace as a virtual cluster inside your Kubernetes cluster.
You can have multiple namespaces inside a single Kubernetes cluster,
and they are all logically isolated from each other.
They can help you and your teams with organization, security, and even performance!
```

```
## プロキシサーバ立ち上げ
$ kubectl proxy
Starting to serve on 127.0.0.1:8001
```

http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/ にアクセス
トークンの入力を求められるのでコピーしておいたトークンを入力

### Kubernetesクラスタ
Kubernetesの様々なリソースを管理する集合体
全体を管理するMasterとNode群によって構成されている

### Node
クラスタがもつリソースでもっとも大きな概念。
Kubernetesのクラスタの管理下に登録されているDockerホスト
Kubernetesでコンテナにデプロイするために利用される

### Masterサーバにデプロイされる管理コンポーネント
kube-apiserver kubectlからのリソース操作を受け付ける
etcd Kubernetesクラスタのバッキングストア
kube-scheduler Nodeを監視。コンテナを配置する最適なNodeを選択する
kube-controller-manager リソースを制御するコントローラを実行する

## Namespace
$ kubectl get namespace

## Pod
コンテナの集合体の単位。少なくとも１つはコンテナを有する。
プロキシサーバとAPIサーバを一つのグループにしてデプロイするときに便利
PodはNodeに配置される

Podの粒度に関して
* リバースプロキシとしてのNginxと、その背後のアプリケーションで1つのPodにまとめる
* 同時にデプロイしないと整合性を保つことができない場合は一括りにしてしまう

simple-pod.yaml
```
apiVersion: v1
kind: Pod # リソースの種類
metadata:
    name: simple-echo
spec: # リソースを定義するための属性。Podの場合はPodを構成するコンテナ群を設定
    containers:
    - name: nginx
      image: gihyodocker/nginx-proxy:latest
      env:
      - name: BACKEND_HOST
        value: localhost:8080
        ports:
        - containerPort: 80
    - name: echo
      image: gihyodocker/echo:latest
      ports:
      - containerPort:8080
```

PodをKubenetesクラスタに反映＆確認
```
$ kubectl apply -f simple-pod.yaml
pod "simple-echo" created
$ kubectl get pod
NAME          READY     STATUS    RESTARTS   AGE
simple-echo   2/2       Running   0          40s
```

Docker同様コンテナに侵入することも可能
```
$ $kubectlexecitsimpleechoshcnginx
// 標準出力
$ kubectl logs -f simple-echo
// 削除(Podベース)
$ kubectl delete pod simple-echo
// 削除(マニフェストベース)
$ kubectl delete -f simple-pod.yaml
```


## ReplicaSet
同じ仕様のPodを複数生成・管理するためのリソース

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: echo
  labels:
    app: echo
spec:
  replicas: 3 # 作成するPod数
  selector:
    matchLabels:
      app: echo
  template:
    metadata:
      labels:
        app: echo
    spec:
      containers:
      - name: nginx
        image: gihyodocker/nginx-proxy:latest
        env:
        - name: BACKEND_HOST
          value: localhost:8080
        ports:
        - containerPort: 80
      - name: echo
        image: gihyodocker/echo:latest
        ports:
        - containerPort: 8080
```

Podの数を減らすと減らした分のPodは削除される。復元も不可能。

削除
```
$ kubectl delete -f simple-replicaset.yaml
replicaset.apps "echo" deleted
```

## Deployment
アプリケーションデプロイの単位となるリソース。ReplicaSetを管理するためのリソース。

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: echo
  labels:
    app: echo
spec:
  replicas: 3
  selector:
    matchLabels:
      app: echo
  template:
    metadata:
      labels:
        app: echo
    spec:
      containers:
      - name: nginx
        image: gihyodocker/nginx-proxy:latest
        env:
        - name: BACKEND_HOST
          value: localhost:8080
        ports:
        - containerPort: 80
      - name: echo
        image: gihyodocker/echo:latest
        ports:
        - containerPort: 8080
```

```
$ kubectl apply -f simple-deployment.yaml --record // 実行したコマンドを記録
deployment.apps "echo" created
```

```
$ kubectl get pod,replicaset,deployment --selector app=echo
NAME                        READY     STATUS    RESTARTS   AGE
pod/echo-567f778dbb-cm765   2/2       Running   0          8m
pod/echo-567f778dbb-fq4mf   2/2       Running   0          8m
pod/echo-567f778dbb-xfhj2   2/2       Running   0          8m

NAME                                    DESIRED   CURRENT   READY     AGE
replicaset.extensions/echo-567f778dbb   3         3         3         8m

NAME                         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.extensions/echo   3         3         3            3           8m
```

Deploymentのリビジョンを確認
```
$ kubectl rollout history deployment echo 
deployments "echo"
REVISION  CHANGE-CAUSE
1         kubectl apply --filename=simple-deployment.yaml --record=true
```

```引用
Deploymentが管理するReplicaSetは指定されたPod数の確保や、新しいバージョンのPodへの入れ替え、以前のバージョンへのPodのロールバックといった重要な役割を担っています。
アプリケーションのデプロイを正しく運用していくためには、（Deploymentの中で）ReplicaSetがどのような挙動をするかを把握しておくことが重要です。Deploymentの更新によってReplicaSetが新しく作成され、ReplicaSetの入れ替えが発生します。
```

Podを追加してもREVISIONは更新されない
コンテナの定義(yaml)を変更するとREVISIONは更新される

```
# 特定のREVISIONの内容を確認
$ kubectl rollout history deployment echo --revision=1
# 特定のREVISIONにロールバック
$ kubectl rollout undo deployment echo deployment "echo" rolled back
```

Deployment削除
```
kubectl delete -f simple-deployment.yaml
```

# Service について

## Cluster IP Service
- 内部IPアドレスに公開できるService。コンテナ間の通信を行いたい場合はこれを使用。

## NodePort Service
- 外部に公開できるサービス。クライアントはポートを指定してリクエストすることでクラスタ内部にアクセすることが可能。
- NodePort Service と Ingress の違いは、NodePort Service はポートを指定してリクエストのルーティングを行うL4レベルの制御しかできないのに対し、Ingress はパスによる制御などの L7レベルのルーティングが可能である点である。

## ExternalName Service
- Kubernetesクラスタ内のコンテナが外部に通信する際に、ホストを解決する際に使用されるService。例えば、内部では gihyo というドメインに対して gihyo.jp というドメインを返す ExternalName Service が存在した場合、コンテナはホストに gihyo と指定するだけで gihyo.jp にリクエストを行うことができる。
