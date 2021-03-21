NodePort Service における Node Port について

# 背景
- 事の発端は誤って ClusterIP Service ではなく NodePort Service を作成してしまったことだった。kubernetes クラスター内通信を行いたいだけなのに、以下のような設定で NodePort Service リソースを作成してしまった。
```yaml
apiVersion: v1
kind: Service
metadata:
  name: knowledge-service
spec:
  selector:
    app: knowledge
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: NodePort
```
- Service の type を NodePort から ClusterIP に変更しようと、マニフェストファイルの `spec.type` の値を `NodePort` から `ClusterIP` に変更してリソースの再作成を行おうとしたが、以下のようなエラーメッセージが出力されリソースの作成に失敗した。

```sh
$ kubectl apply -f service.yaml
The Service "knowledge-service" is invalid: spec.ports[0].nodePort: Forbidden: may not be used when `type` is 'ClusterIP'
```

- どうやら ClusterIP Service では `nodePort` の設定は禁止されているためにリソースの再作成に失敗した模様。なぜ `nodePort` の設定がなされていると ClusterIP Service の生成に失敗するのか公式のドキュメントを読んで考えてみた。

# NodePort
- まず NodePort に対する知識が曖昧だったので [Service の公式ドキュメント](https://kubernetes.io/ja/docs/concepts/services-networking/service/) を読んでみた。NodePort Service が作成されると、Kubernetes クラスター内の各 Node にその NodePort Service へ接続するためのポートが解放される。この時解放されたポートのことを `NodePort` と呼ぶ。したがって NodePort Service を作成した場合、`${NodeのIP}:${nodePort}` でその NodePort Service にアクセスすることが可能である。
- 今回 NodePort Service から ClusterIP Service への変更に失敗したのは、ClusterIP Service の用途は主に Kubernetes クラスター内部間通信の実現であり、外部に公開する用途を想定していないため、`NodePort` が設定されている場合にリソースの生成に失敗したのではと考えられる。

# まとめ
- NodePort Service から ClusterIP Service への変更に失敗した理由について自分なりの考察をしてみた。考察についてご意見があれば教えていただけると嬉しいです。

# 参考文献
- https://kubernetes.io/ja/docs/concepts/services-networking/service/
- https://kubernetes.io/ja/docs/tutorials/kubernetes-basics/create-cluster/cluster-intro/
- https://kubernetes.io/ja/docs/tutorials/kubernetes-basics/explore/explore-intro/
- https://oxynotes.com/?p=6361