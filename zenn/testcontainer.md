# Testcontainer を使用してデータアクセス層の結合テストを実装する

# はじめに
最近データアクセス層の結合テストをどう実装しようか悩んでいたところ、調べていたら [Testcontainers](https://www.testcontainers.org/) というライブラリを見つけたので実装してみました。Testcontainers とは MySQL や PostgreSQL などがインストールされているコンテナを使用してデータアクセス層のテストを比較的容易に実装するためのライブラリです。今回はデータベースに MySQL を採用し、OR Mapper は MyBatis を使用することにしました。なお言語は Kotlin です。

# サンプルコード
以下にサンプルコードを以下に Push しているので、詳細について知りたい場合はご確認していただけたらと思います。
[empenguin1186/spring-demo](https://github.com/empenguin1186/spring-demo)

# 実装

## プロダクトコード
まずはプロダクトコードについて紹介します。今回はテスト用に `TaskMapper` という Mapper インターフェースを定義しました。実装は以下となります。

```kt
@Mapper
@Component
interface TaskMapper {

    /**
     * タスク名と担当者を指定してタスクを作成する
     */
    @Insert("""
       INSERT INTO Tasks (
           task_name,
           assignee
       ) VALUES (
           #{taskName},
           #{assignee}
       )
    """)
    fun insert(task: Task)

    /**
     * 担当者を指定してタスクを取得する
     */
    @Select("""SELECT * FROM Tasks WHERE assignee = #{assignee}""")
    fun findByAssignee(assignee: String): List<Task>

    /**
     * タスク名と担当者を指定してタスクを取得する
     */
    @Select("""SELECT * FROM Tasks WHERE task_name = #{taskName} AND assignee = #{assignee}""")
    fun findByTaskNameAndAssignee(taskName: String, assignee: String): Task?

    /**
     * タスク名と担当者が合致したタスクを削除する
     */
    @Delete("""DELETE FROM Tasks WHERE task_name = #{taskName} AND assignee = #{assignee}""")
    fun deleteByTaskNameAndAssignee(taskName: String, assignee: String)
}
```

## テストコード
続いてテストコードを以下に示します。
```kt
@MybatisTest // (1)
@Testcontainers // (2)
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE) // (3)
@EnabledIfEnvironmentVariable(named = "DB_TEST_ENABLE", matches = "true") // (4)
internal class TaskMapperTest {

    @Autowired
    private lateinit var taskMapper: TaskMapper

    /** (5) */
    companion object {
        @Container
        @JvmStatic
        val mysqlContainer = MySQLContainer<Nothing>(DockerImageName.parse("mysql")).apply {
            withUsername("user")
            withPassword("mysql")
            withDatabaseName("testdb")
            withInitScript("initdb/schema.sql") // (6)
        }

        @DynamicPropertySource
        @JvmStatic
        fun setUp(registry: DynamicPropertyRegistry) {
            registry.add("spring.datasource.url", mysqlContainer::getJdbcUrl)
            registry.add("spring.datasource.username", mysqlContainer::getUsername)
            registry.add("spring.datasource.password", mysqlContainer::getPassword)
        }
    }

    @Nested
    inner class FindByAssignee {
        @Test
        fun `Taskを作成および取得できることを確認するテスト`() {
            // given
            val taskName = "task1"
            val assignee = "assignee1"
            val task = Task.create(taskName, assignee)

            // when
            taskMapper.insert(task)
            val tasks = taskMapper.findByAssignee(assignee)

            // then
            SoftAssertions().apply {
                assertThat(tasks.size).isEqualByComparingTo(1)
                assertThat(tasks[0].taskName).isEqualTo(taskName)
                assertThat(tasks[0].assignee).isEqualTo(assignee)
            }.assertAll()
        }

        @Test
        fun `作成していないTaskを取得できないことを確認するテスト`() {
            // given
            val assignee = "assignee1"

            // when
            val tasks = taskMapper.findByAssignee(assignee)

            // then
            SoftAssertions().apply {
                assertThat(tasks.size).isEqualTo(0)
            }.assertAll()
        }
    }
}
```

|  番号  |  説明  |
|:--:|:---|
|  (1)  |  MyBatis の機能を使用したテストを実装する場合はこのアノテーションを付与する  |
|  (2)  |  Testcontainers の機能を使用したテストを実装する場合はこのアノテーションを付与する  |
|  (3)  |  @MybatisTest アノテーションを付与すると、デフォルトでは組み込みのデータベースを使用してテストが実行される。今回はコンテナ内に起動した MySQL データベースに対してテストを行うので、このアノテーションを付与する |
|  (4)  |  DB_TEST_ENABLE 環境変数に true が設定されている場合にのみテストを実施する。理由については後述  |
|  (5)  |  MySQL コンテナ起動時の設定について記載している。またデータベースのURLなどテスト実行時に使用するプロパティについて設定を行っている  |
|  (6)  |  データベース初期化時に実行されるスクリプトを設定している。実行内容については [こちら](https://github.com/empenguin1186/spring-demo/blob/main/src/test/resources/initdb/schema.sql) を参照 |
実行すると実際にコンテナが起動され、テストが成功することが確認できます。

# 実装時に気になった点
## テスト時のログレベルが DEBUG になっている
テスト実行時のログレベルがデフォルトだと DEBUG になっていて大量にログが出力されてテスト結果が埋もれてしまったので、テスト用の logback の設定ファイルを配置してログレベルを INFO にしました。設定ファイルの内容については [こちら](https://www.testcontainers.org/supported_docker_environment/logging_config/) を参考にしました。サンプルコードでは [こちら](https://github.com/empenguin1186/spring-demo/blob/main/src/test/resources/logback-test.xml) に配置しています。

## ローカルでのテスト実行速度が遅い
これは自分のPCのスペックの問題でもあるかと思いますが、IntelliJ や gradle コマンドでテストを実行した場合、完了するまでにかなり時間がかかっていたので、データアクセス層のテストの実行は最小限に抑えたいと考えました。したがって自分のPCのようなローカルの環境ではデフォルトでデータアクセス層のテストはスキップし、Github でデータアクセス層のコードを修正する PR が作成された場合のみ Github Actions でデータアクセス層のテストを実行するように設定を行いました。Github Actions の設定については以下になります。

```yaml
name: CI

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    name: Test changed-files
    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: 0

      - name: Get changed files # (1)
        id: changed-files
        uses: tj-actions/changed-files@v11.9 

      - name: Set DB_TEST_ENABLE Variable # (2)
        run: |
          if echo "${{ steps.changed-files.outputs.all_modified_files }}" |
          grep -q -e "src/main/kotlin/com/example/demo/infra/repository/mapper" \
          -e "src/test/kotlin/com/example/demo/infra/repository/mapper"; then
            echo "DB_TEST_ENABLE=true" >> $GITHUB_ENV
            echo "Data Access Test will be executed."
          else
            echo "DB_TEST_ENABLE=false" >> $GITHUB_ENV
            echo "Data Access Test will be not executed."
          fi

      - name: Set up JDK 17
        uses: actions/setup-java@v1
        with:
          java-version: 17

      - name: Build with Gradle
        run: ./gradlew build -DDB_TEST_ENABLE=$DB_TEST_ENABLE  # (3)
```
|  番号  |  説明  |
|:--:|:---|
|  (1)  |  この Action により修正したファイルをリストアップする  |
|  (2)  |  (1) でリストアップしたファイルが指定されたパス配下に存在する場合は `DB_TEST_ENABLE` 環境変数に true を設定する |
|  (3)  |  ビルド実行時に環境変数を指定してテストを行う。DB_TEST_ENABLE=$DB_TEST_ENABLEに true がセットされている場合にのみデータアクセス層のテストが実行される |
正直 Github Actions では常時データアクセス層のテストを実行するように設定しても良いと思いましたが、せっかくなので上記のような実装を試してみました。

# 参考になったサイト
- [Testcontainers](https://www.testcontainers.org/)
- [TestContainersを使って、SpringBoot + MyBatis + PostgreSQL 環境のDB単体テストを実行する - Qiita](https://qiita.com/kazokmr/items/18959c5eeffa5c8a5ad7)
- [Testcontainersを利用したSpring Bootアプリのテスト - Qiita](https://qiita.com/kazuki43zoo/items/588bbfab3db9d6c0199f#postgresql%E7%B7%A8)
- [\[GitHub Actions\]ファイルの差分や更新状態を元にStepの実施を切り分けてみる | DevelopersIO](https://dev.classmethod.jp/articles/switch-step-by-file-conditions/)
- [Changed files · Actions · GitHub Marketplace](https://github.com/marketplace/actions/changed-files)