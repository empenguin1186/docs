# 背景
- GKE で Ingress と Service をデプロイしたところ、コンソールに以下のようなエラーが出力された。
![](https://storage.googleapis.com/zenn-user-upload/98l5e789yb1e2z590ninuq96er15)
- 今まで Ingress と Service は同じ Kubernetes クラスター内に存在するものだと思っていたので Service の type は ClusterIP としていたがその認識が間違っていたらしい。

# 調査
- [Google Cloud のドキュメント](https://cloud.google.com/kubernetes-engine/docs/concepts/network-overview?hl=ja) を確認すると、以下のような記述がされている。

> Ingress オブジェクトを作成すると、Ingress マニフェストと関連するサービス マニフェストのルールに従い、GKE Ingress コントローラによって Google Cloud HTTP(S) ロードバランサが構成されます。クライアントは HTTP(S) ロードバランサにリクエストを送信します。ロードバランサは実際のプロキシです。つまり、ノードを選択して、そのノードの NodeIP:NodePort の組み合わせにリクエストを転送します。ノードはその iptables NAT テーブルを使用してポッドを選択します。ノードの iptables ルールは、kube-proxy によって管理されます。

- ロードバランサは `NodeIP:NodePort` の組みあわせでリクエストを転送すると書かれているので Ingress に設定される Service の type は `NodePort` である必要がある。

# 参考文献
- https://cloud.google.com/kubernetes-engine/docs/how-to/load-balance-ingress?hl=ja
- https://cloud.google.com/kubernetes-engine/docs/concepts/network-overview?hl=ja#services
- https://cloud.google.com/kubernetes-engine/docs/concepts/network-overview?hl=ja
- https://christina04.hatenablog.com/entry/nginx-ingress-controller