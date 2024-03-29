
# VPC 移行における MAZ DB について
- DB インスタンスが MAZ である場合、VPC の変更は不可能。一旦 シングルAZに変換し、VPC 移行を行った後、MAZ に戻す必要がある。DB サブネットグループを MAZ 構成に変更することは不可能。したがって Amazon Aurora の VPC を変更することはできない。したがって Aurora を使用する場合は DB のクローンかスナップショットからの復元を行うことでVPC移行を行う。
  - https://aws.amazon.com/jp/premiumsupport/knowledge-center/change-vpc-rds-db-instance/
  - https://aws.amazon.com/jp/premiumsupport/knowledge-center/rds-vpc-aurora-cluster/
  - https://dev.classmethod.jp/articles/amazon-aurora-database-cloning/

- リンク
  - RDS (Relational Database Service)
    - https://aws.amazon.com/jp/rds/
    - Amazon Aurora Postgres
      - https://aws.amazon.com/jp/rds/aurora/postgresql-features/
  - RDS Proxy
    - https://aws.amazon.com/jp/rds/proxy/
      - 最新のサーバーレスアーキテクチャに構築されたアプリケーションなどのアプリケーションの多くは、データベースサーバーへの接続を多数開くことができます。このとき、データベース接続の開閉が高頻度で実行されて、データベースメモリやコンピューティングリソースを消耗する可能性があります。Amazon RDS Proxy では、アプリケーションがデータベースと確立した接続をプールおよび共有でき、データベースの効率とアプリケーションのスケーラビリティが向上します。
  - AWS Lambda
    - faas みたいなもの
    - https://aws.amazon.com/jp/lambda/
  - VPC S3 Endpoint
    - https://docs.aws.amazon.com/ja_jp/glue/latest/dg/vpc-endpoints-s3.html
  - Amazon S3
    - https://aws.amazon.com/jp/free/storage/?trk=768ff959-57c7-4520-8a42-06a6f1cd29e7&sc_channel=ps&sc_campaign=acquisition&sc_medium=ACQ-P|PS-GO|Brand|Desktop|SU|Storage|Product|JP|EN|Text|dx&s_kwcid=AL!4422!3!470061306391!e!!g!!amazon%20s3&ef_id=Cj0KCQjwjN-SBhCkARIsACsrBz62lIvuGPkMSZOP87gOHDEhl3G3zoUeJem8JzzCzlE7LUGDQg2eX10aAoOBEALw_wcB:G:s&s_kwcid=AL!4422!3!470061306391!e!!g!!amazon%20s3&gclid=Cj0KCQjwjN-SBhCkARIsACsrBz62lIvuGPkMSZOP87gOHDEhl3G3zoUeJem8JzzCzlE7LUGDQg2eX10aAoOBEALw_wcB
  - AWS Glue
    - https://docs.aws.amazon.com/ja_jp/glue/latest/dg/what-is-glue.html
    - https://dev.classmethod.jp/articles/relay_looking_back_aws_glue/
  - Internet Gateway
    - https://docs.aws.amazon.com/ja_jp/vpc/latest/userguide/VPC_Internet_Gateway.html
  - CloudFront
  - API Gateway
    - 役割
  - Cognito
  - VPC の作成方法
    - https://docs.aws.amazon.com/ja_jp/directoryservice/latest/admin-guide/gsg_create_vpc.html
  - RDS をマルチ A-Z のレプリカ構造にする方法
    - https://dev.classmethod.jp/articles/how-to-change-rds-multiaz-configuration/
  - RDS を VPC に移動する方法
    - https://aws.amazon.com/jp/premiumsupport/knowledge-center/change-vpc-rds-db-instance/
  - Lambda の VPC とセキュリティグループを新しい VPC に変更する方法
    - https://docs.aws.amazon.com/ja_jp/lambda/latest/dg/foundation-networking.html