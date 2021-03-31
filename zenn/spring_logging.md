
# 概要
- Spring Boot を使用した Web アプリケーションで、アクセスログとビジネスロジック実行時の処理の内容を含むサーバログを出力するための設定を紹介します。言語は Kotlin となります。

# 要件
- 今回全てのログはコンソールに出力することとします。また、各ログに対して下記の内容を出力することとします。

|  ログ  |  出力する内容  |
|:--:|:---|
|  アクセスログ  |  アクセス日時 <br> ステータスコード <br> リクエストURL <br> レイテンシ(ミリ秒) <br> UUID(アプリケーション側で生成)  |
|  サーバログ  |  ロギングイベント発生日時 <br> ログレベル <br> ロギングイベント発生クラス <br> UUID(アプリケーション側で生成) <br> ログメッセージ <br> スタックトレース(例外発生時のみ)  |

- アクセスログとサーバログを紐付けるために、UUIDを生成して各ログに含めることとしています。


# 実装

## logback の設定
- 上記の要件を満たす logback.xml の設定を下記に示します。

```xml:logback.xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <appender name="ACCESS_LOG" class="ch.qos.logback.core.ConsoleAppender"> <!-- (1) -->
        <encoder>
            <pattern>date=%d{yyyy-MM-dd HH:mm:ss.SSS}\tstatus_code=%X{status_code}\trequest_url=%X{request_url}\tlatency=%X{latency}\tuuid=%X{uuid}%n</pattern> <!-- (2) -->
        </encoder>
    </appender>

    <appender name="SERVER_LOG" class="ch.qos.logback.core.ConsoleAppender"> <!-- (1) -->
        <encoder>
            <pattern>date=%d{yyyy-MM-dd HH:mm:ss.SSS}\tlevel=%level\tlogger=%logger{0}\tuuid=%X{uuid}\tmessage="%m"\t%ex{short}%n</pattern> <!-- (2) -->
        </encoder>
    </appender>

    <logger name="jp.co.emperor.penguin.spring.filters.AccessLoggingFilter" additivity="false" level="INFO"> <!-- (3) -->
        <appender-ref ref="ACCESS_LOG" />
    </logger>

    <root level="INFO"> <!-- (4) -->
        <appender-ref ref="SERVER_LOG" />
    </root>
</configuration>
```
- (1) の `appender` プロパティが実際にログを出力するための設定となります。`class` フィールドはログの出力先を設定する設定となります。`ch.qos.logback.core.ConsoleAppender` が指定されていると、ログの出力先はコンソール(標準出力 or 標準エラー出力)となります。他の `Appender` については [logbackの公式ドキュメント](http://logback.qos.ch/manual/appenders_ja.html) で紹介されています。
- (2) の `pattern` プロパティではログフォーマットの設定を行うことが可能です。各フォーマットで使用されているパラメータの説明は以下となります。

|  パラメータ  |  説明  |
|:--:|:---|
|  %d{yyyy-MM-dd HH:mm:ss.SSS}  |  ロギングイベントの日時。引数として日時のパターン文字列を指定できる。  |
|  %X{key}  |  引数にキー名を指定することで、アプリケーションが登録したそのキーに対応した値を Context から取得する。  |
|  %level  |  ログレベル  |
|  %logger{0}  |  ロギングイベント発生クラス。引数に0を指定することでクラス名のみ出力することができる。 |
|  %m  |  ログメッセージ  |
|  %ex  |  例外発生時のスタックトレース。引数に short を指定した場合はスタックトレースの１行目のみ出力する。|
|  %n  |  改行  |

- (3) の `logger` プロパティでは個別にどのクラスにどの `Appender` を適用するかの設定を行っています。ここでは `jp.co.emperor.penguin.spring.filters.AccessLoggingFilter` というクラスに `ACCESS_LOG` という名前の `Appender` を適用するようにしています。また、`additivity` フィールドに `false` を設定していると、他の `Appender` が適用されなくなります。最後に `level` フィールドで出力するログレベルについて設定を行っています。この場合は `INFO` 以上のログレベルのログを出力するようにしています。
- (4) の `root` プロパティで全てのクラスに `SERVER_LOG` という名前の `Appender` を適用するように設定を行っています。

## Filter クラス実装
- 次にアクセスログを出力するための実装を行います。今回はアクセスログの出力に spring framework の [OncePerRequestFilter](https://spring.pleiades.io/spring-framework/docs/current/javadoc-api/org/springframework/web/filter/OncePerRequestFilter.html) を使用します。OncePerRequestFilter を継承したクラスは一回のリクエストにつき一回呼び出されることを保証する Filter となります。
- 実装クラスを以下に示します。クラス名は logback.xml の設定時に指定した `jp.co.emperor.penguin.spring.filters.AccessLoggingFilter` とします。

```kt:jp.co.emperor.penguin.spring.filters.AccessLoggingFilter
package jp.co.emperor.penguin.spring.filters

import org.slf4j.MDC
import org.springframework.web.filter.OncePerRequestFilter
import java.util.UUID
import javax.servlet.FilterChain
import javax.servlet.http.HttpServletRequest
import javax.servlet.http.HttpServletResponse

class AccessLoggingFilter : OncePerRequestFilter() {

    companion object {
        private const val STATUS_CODE = "status_code"
        private const val REQUEST_URL = "request_url"
        private const val LATENCY = "latency"
        private const val UNIQUE_ID = "uuid"
        private  val logger = LoggerFactory.getLogger(AccessLoggingFilter::class.java)
    }

    override fun doFilterInternal(
        request: HttpServletRequest,
        response: HttpServletResponse,
        filterChain: FilterChain
    ) {
        try {
            // レイテンシ計測
            val start = System.currentTimeMillis()

            MDC.put(UNIQUE_ID, UUID.randomUUID().toString())
            MDC.put(REQUEST_URL, request.servletPath)

            filterChain.doFilter(request, response)

            MDC.put(STATUS_CODE, response.status.toString())
            MDC.put(LATENCY, (System.currentTimeMillis() - start).toString())
            logger.info("")
        } finally {
            // 確実に Context から削除する
            MDC.remove(STATUS_CODE)
            MDC.remove(REQUEST_URL)
            MDC.remove(LATENCY)
            MDC.remove(UNIQUE_ID)
        }
    }
}
```
- `doFilterInternal()` をオーバーライドしてアクセスログを出力する処理を実装しています。レイテンシの値に関しては `filterChain.doFilter()` で実際にビジネスロジックが実行されているので、その前後で現在時刻を取得してその差分を計算してレイテンシとしています。また `MDC.put()` でそれぞれのキーに対する値を登録しています。最後に finally 句でログ出力が行われた後も Context に値が残らないように、 Context に登録されている値を削除しています。

## Filter の Bean 定義
- Filter を実装しただけではアクセスログは出力されません。Filter を有効にするには、[FilterRegistrationBean](https://spring.pleiades.io/spring-boot/docs/current/api/org/springframework/boot/web/servlet/FilterRegistrationBean.html) を Bean 定義する必要があります。以下に実装例を示します。

```kt
package jp.co.emperor.penguin.spring.config

import jp.co.emperor.penguin.spring.filters.AccessLoggingFilter
import org.springframework.boot.web.servlet.FilterRegistrationBean
import org.springframework.context.annotation.Bean
import org.springframework.context.annotation.Configuration

@Configuration
class FilterConfig {

    @Bean
    fun accessLoggingFilter() = FilterRegistrationBean(AccessLoggingFilter()).also {
        it.addUrlPatterns("*")
        it.order = Integer.MIN_VALUE
    }
}
```
- `addUrlPatterns()` で Filter を適用するリクエスト URL を指定することができます。今回は全てのリクエストのアクセスログを出力することとしています。`it.order = Integer.MIN_VALUE` では Filter の適用順序の設定を行っています。値が小さい Filter ほど先に適用されることになります。

# 挙動確認
- 以下の Controller クラスを作成して実際にログを確認してみます。
```kt
@RestController
@RequestMapping("/main", "/")
class MainController {

    companion object {
        private val logger = LoggerFactory.getLogger(MainController::class.java)
    }

    @GetMapping
    fun get(): String {
        logger.info("info")
        logger.error("error", Exception("exception"))
        return "Hello, World"
    }
}
```
- あらゆる URL に対してアクセスログが出力されているかどうかを確認するために、localhost:8080 でサーバを起動して `localhost:8080/`, `localhost:8080/main` に対して GET リクエストを実行してみました。結果、以下のようなログが出力され、無事あらゆる URL に対してアクセスログが出力されていることを確認しました。ログの内容も過不足なしです。
```sh
date=2021-04-01 00:25:51.365	level=INFO	logger=MainController	uuid=a37f358f-4728-42f2-a876-fb0d4b9da523	message="info"	
date=2021-04-01 00:25:51.370	level=ERROR	logger=MainController	uuid=a37f358f-4728-42f2-a876-fb0d4b9da523	message="error"	java.lang.Exception: exception
	at jp.co.emperor.penguin.controller.MainController.get(MainController.kt:19)

date=2021-04-01 00:25:51.403	status_code=200	request_url=/	latency=74	uuid=a37f358f-4728-42f2-a876-fb0d4b9da523
date=2021-04-01 00:25:55.071	level=INFO	logger=MainController	uuid=9c9920c6-d5a0-4dec-8083-e5276548c85c	message="info"	
date=2021-04-01 00:25:55.072	level=ERROR	logger=MainController	uuid=9c9920c6-d5a0-4dec-8083-e5276548c85c	message="error"	java.lang.Exception: exception
	at jp.co.emperor.penguin.controller.MainController.get(MainController.kt:19)

date=2021-04-01 00:25:55.074	status_code=200	request_url=/main	latency=3	uuid=9c9920c6-d5a0-4dec-8083-e5276548c85c
```

# まとめ
- Spring Boot と logback でアクセスログとサーバログの出力方法についてまとめました。余裕があればログのローテートなどについても今後まとめていきたいですね。

# 参考資料
- [第3章 logbackの設定](http://logback.qos.ch/manual/configuration_ja.html)
- [第4章 アペンダー](http://logback.qos.ch/manual/appenders_ja.html)
- [第6章 レイアウト](http://logback.qos.ch/manual/layouts_ja.html)
- [OncePerRequestFilter (Spring Framework 5.3.4 API) - Javadoc](https://spring.pleiades.io/spring-framework/docs/current/javadoc-api/org/springframework/web/filter/OncePerRequestFilter.html)
- [FilterRegistrationBean (Spring Boot 2.4.4 API) - Javadoc](https://spring.pleiades.io/spring-boot/docs/current/api/org/springframework/boot/web/servlet/FilterRegistrationBean.html)
- [2.2. Spring MVCアーキテクチャ概要 — TERASOLUNA Global Framework Development Guideline 1.0.0.publicreview documentation](https://terasolunaorg.github.io/guideline/public_review/Overview/SpringMVCOverview.html)