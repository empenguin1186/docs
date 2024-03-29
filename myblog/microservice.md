# マイクロサービスアーキテクチャ

1. マイクロサービスのメリット
2. 進化的アーキテクトの責務
  - 進化的アーキテクトとは?
3. マイクロサービスへの分割のさせ方
4. マイクロサービス間の連携について
5. モノリスの分割する方法(優先度低め)
6. CD について(優先度低め)
7. テストについて(優先度低め)
  - コンシューマ駆動は何かを抑える
8. 監視について
9. セキュリティかどう
10. コンウェイの法則
11. 大規模なマイクロサービスについて
12. まとめ


## マイクロサービスの概要

### メリット
- 技術異質性
  - マイクロサービスを採用すると、小さなシステムを複数組み合わせることによってひとつのサービスを構築することになる。したがってシステムのとある部分を担当するサービスの性能を向上させたい場合に、技術スタックの切り替えを容易に行うことができるのがメリットである。
  - ただし使ったことのない技術を使用してシステムをリプレイスするのはリスクが伴うので使用する技術スタックを限定するなどしてバランスを考えた方が良い。
- 回復性
  - モノリシックなシステムとは異なり、一部のサービスで障害が発生したとしても、そのサービスとは関連がないサービスで構築されているシステムには影響はないので機能し続けることが可能。したがってマイクロサービスを採用する場合はこのサービス間の適切な境界を考慮する必要がある。
- スケーリング
  - マイクロサービスは複数のサービスで構成されているので部分的にスケーリングを行うことができ、モノリシックなシステムとは異なり柔軟性がある。
- 組織面の一致
  - マイクロサービスは一つ一つの規模が小さいので、少人数構成のチームで開発/運用することが可能である。また、少人数構成のチームの方が生産性が高くなるので効率の良い開発を進めることが可能。
- 合成可能性
  - マイクロサービスは機能を再利用することができる。
- 交換可能にするための最適化
  - マイクロサービスは個々のサービスの規模が小さいのでリプレイスのコストや削除コストが低い。

### 他の分割の方法
- 共有ライブラリを使用して機能を複数に分割することも考えられるが、この方法を採用すると各ライブラリに修正が行われた場合に逐次デプロイを行わなければならない、かつライブラリはおそらく同じ言語で書かれているので技術的な多様性が失われてしまうことが考えられる。

