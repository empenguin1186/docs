GitHub Actions でビルド実行結果をSlackに投稿する方法

# 経緯
- GitHub Actions で push をトリガーとして実行されるビルドの成否の通知先を e-mail ではなく Slack にしたいと思ったので、その方法について調べてみました。

# 設定方法
- 今回は Java(バージョンは11) の Gradle プロジェクトのビルド実行結果を [Slack Notify](https://github.com/marketplace/actions/slack-notify) という Action を使用して任意の Slack チャンネルに通知するように設定してみました。設定例としては以下になります。
```yaml
name: Push Workflow
on:
  push:
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      # ビルド実行
      - name: Checkout
        uses: actions/checkout@v2
      - name: Set up JDK 11
        uses: actions/setup-java@v1
        with:
          java-version: 11
      - name: Build with Gradle
        run: ./gradlew build
      
      # ビルドの実行結果を Slack に投稿する
      - name: Notificate Slack Channel
        uses: rtCamp/action-slack-notify@v2
        if: always() # (1)
        env:
          SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK_URL }} # (2)
          SLACK_COLOR: ${{ job.status }} # (3)
          SLACK_MESSAGE: "Job Result: ${{ job.status }}" # (4)
          SLACK_TITLE: Job Result
          SLACK_USERNAME: Job Result Bot
```
|  番号  |  説明  |
|:--|:---|
|  (1)  |  `always()` を指定することで、ビルドの成否にかかわらず Slack チャンネルに通知する。  |
|  (2)  |  通知先の Incoming Webhook URL の設定が必要。今回はリポジトリの Secrets に `SLACK_WEBHOOK_URL` という名前で URL を登録している。 |
|  (3)  |  ビルドの成否によってメッセージの色を変更する。成功時には緑色、失敗時には赤色でメッセージがチャンネルに投稿される。  |
|  (4)  |  ビルドの成否によってメッセージの内容を変更する。`job.status`の値は success、failure、cancelled のいずれかの値をとる。 |

# 実行結果
## ビルド成功時
![](https://storage.googleapis.com/zenn-user-upload/tu4u402y5uzr4kq6nvyrhq6o8ycw)

## ビルド失敗時
![](https://storage.googleapis.com/zenn-user-upload/ss1vgxdxzlg5b03l1snurfxfy9d7)

# まとめ
- ビルド実行結果を Slack に投稿する方法について紹介しました。他にも方法があれば教えていただけると嬉しいです。

# 参考文献
- [GitHub Actions のコンテキストおよび式の構文](https://docs.github.com/ja/actions/reference/context-and-expression-syntax-for-github-actions)
- [GitHub Actionsのワークフロー構文](https://docs.github.com/ja/actions/reference/workflow-syntax-for-github-actions)
- [暗号化されたシークレット](https://docs.github.com/ja/actions/reference/encrypted-secrets)
- [Slack Notify - GitHub Action](https://github.com/marketplace/actions/slack-notify)