## Graph QL について

https://dev.to/sambenskin/howto-build-graphql-services-in-java-with-spring-boot---part-1-38b2 のチュートリアルを進め、「Building the API」の項まで進めアプリケーションを起動したら以下のようなエラーが発生した。

```
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'schemaParser' defined in class path resource [com/oembedler/moon/graphql/boot/GraphQLJavaToolsAutoConfiguration.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [com.coxautodev.graphql.tools.SchemaParser]: Factory method 'schemaParser' threw exception; nested exception is java.lang.IllegalStateException: No *.graphqls files found on classpath.  Please add a graphql schema to the classpath or add a SchemaParser bean to your application context.
	at org.springframework.beans.factory.support.ConstructorResolver.instantiate(ConstructorResolver.java:627) ~[spring-beans-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:607) ~[spring-beans-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1305) ~[spring-beans-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1144) ~[spring-beans-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:555) ~[spring-beans-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:515) ~[spring-beans-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:320) ~[spring-beans-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:222) ~[spring-beans-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:318) ~[spring-beans-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:199) ~[spring-beans-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:849) ~[spring-beans-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:877) ~[spring-context-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:549) ~[spring-context-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:142) ~[spring-boot-2.1.3.RELEASE.jar:2.1.3.RELEASE]
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:775) [spring-boot-2.1.3.RELEASE.jar:2.1.3.RELEASE]
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:397) [spring-boot-2.1.3.RELEASE.jar:2.1.3.RELEASE]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:316) [spring-boot-2.1.3.RELEASE.jar:2.1.3.RELEASE]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1260) [spring-boot-2.1.3.RELEASE.jar:2.1.3.RELEASE]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1248) [spring-boot-2.1.3.RELEASE.jar:2.1.3.RELEASE]
	at jp.co.penguin.graphql.GraphqlApplication.main(GraphqlApplication.java:10) [main/:na]
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:1.8.0_171]
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[na:1.8.0_171]
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[na:1.8.0_171]
	at java.lang.reflect.Method.invoke(Method.java:498) ~[na:1.8.0_171]
	at org.springframework.boot.devtools.restart.RestartLauncher.run(RestartLauncher.java:49) [spring-boot-devtools-2.1.3.RELEASE.jar:2.1.3.RELEASE]
Caused by: org.springframework.beans.BeanInstantiationException: Failed to instantiate [com.coxautodev.graphql.tools.SchemaParser]: Factory method 'schemaParser' threw exception; nested exception is java.lang.IllegalStateException: No *.graphqls files found on classpath.  Please add a graphql schema to the classpath or add a SchemaParser bean to your application context.
	at org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiate(SimpleInstantiationStrategy.java:185) ~[spring-beans-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.beans.factory.support.ConstructorResolver.instantiate(ConstructorResolver.java:622) ~[spring-beans-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	... 24 common frames omitted
Caused by: java.lang.IllegalStateException: No *.graphqls files found on classpath.  Please add a graphql schema to the classpath or add a SchemaParser bean to your application context.
	at com.oembedler.moon.graphql.boot.GraphQLJavaToolsAutoConfiguration.schemaParser(GraphQLJavaToolsAutoConfiguration.java:53) ~[graphql-spring-boot-autoconfigure-3.10.0.jar:na]
	at com.oembedler.moon.graphql.boot.GraphQLJavaToolsAutoConfiguration$$EnhancerBySpringCGLIB$$a670d55e.CGLIB$schemaParser$0(<generated>) ~[graphql-spring-boot-autoconfigure-3.10.0.jar:na]
	at com.oembedler.moon.graphql.boot.GraphQLJavaToolsAutoConfiguration$$EnhancerBySpringCGLIB$$a670d55e$$FastClassBySpringCGLIB$$3d4e3147.invoke(<generated>) ~[graphql-spring-boot-autoconfigure-3.10.0.jar:na]
	at org.springframework.cglib.proxy.MethodProxy.invokeSuper(MethodProxy.java:244) ~[spring-core-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.context.annotation.ConfigurationClassEnhancer$BeanMethodInterceptor.intercept(ConfigurationClassEnhancer.java:363) ~[spring-context-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at com.oembedler.moon.graphql.boot.GraphQLJavaToolsAutoConfiguration$$EnhancerBySpringCGLIB$$a670d55e.schemaParser(<generated>) ~[graphql-spring-boot-autoconfigure-3.10.0.jar:na]
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:1.8.0_171]
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[na:1.8.0_171]
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[na:1.8.0_171]
	at java.lang.reflect.Method.invoke(Method.java:498) ~[na:1.8.0_171]
	at org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiate(SimpleInstantiationStrategy.java:154) ~[spring-beans-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	... 25 common frames omitted


Process finished with exit code 0
```

SchemaParserの初期化に失敗したというエラーメッセージであるが、調べてみると、クラスパスに *.graphqlsというschemaを定義しているファイルが存在しないために初期化に失敗していることが判明した。
クラスパスとは：https://ja.wikipedia.org/wiki/%E3%82%AF%E3%83%A9%E3%82%B9%E3%83%91%E3%82%B9
src/main/resources/ 配下に petshop.graphqls　を配置していたが読み込まれていないらしい。

解決方法を探したところ、以下のページでかかれてあることを実行したら↑のエラーは解消された

Failed to start SpringBoot Application after addition of GraphQLQueryResolver · Issue #157 · graphql-java-kickstart/graphql-java-tools
https://github.com/graphql-java-kickstart/graphql-java-tools/issues/157

```
Try removing your dependency on graphql-spring-boot and only add the graphql-java-tools dependency and see if that works.
```

GraphiQLのUIは開けたのだが、リクエストをするとエラーレスポンスが返ってくる

リクエスト
```
{
    pets {
        name
        age
        type
    }
}
```

レスポンス
```
{
  "timestamp": "2019-04-04T00:21:08.145+0000",
  "status": 404,
  "error": "Not Found",
  "message": "No message available",
  "path": "/graphql"
}
```

## 解決策
SpringBootのバージョンを 2.0.3.RELEASE にすると動作するようになった。


# MongoDB連携

## package install
使用するライブラリを取り込む

```:build.gradle
compile group: 'org.mongodb', name: 'mongo-java-driver', version: '3.11.0-beta2'
```

## 注意点

### 1
* collection.find()を使用する時にはメソッドバインド先のPOJOにはNoArgsConstructorを実装すること。実装していないと以下のような例外が発生する。

```
org.bson.codecs.configuration.CodecConfigurationException: An exception occurred when decoding using the AutomaticPojoCodec.
Decoding into a '<POJO>' failed with the following exception:

Cannot find a public constructor for '<POJO>'.
```

### 2
* collectionにはユニークインデックスを設定しておくこと。設定していないと複数のドキュメントを挿入することができない。あらかじめcollectionを作成して設定する場合はmongodbを起動して設定を行う。

参考：[MongoDB入門 - indexを作成してみよう - Qiita](https://qiita.com/soudai_s/items/d53d0c4a2021fb955dd4)


* 例えばmydbのusersというcollectionのusernameというキーをユニークにしたい場合

```
$ mongo
> use mydb
switched to db mydb
// あらかじめ違う設定のusersが存在する場合は削除する
> db.users.drop()
true

// ユニークインデックス指定
> db.users.createIndex({username: 1}, {unique: true})
```

とする必要がある

* ドキュメント挿入前にこの設定をしておくのが望ましい（設定前のドキュメントのusernameはユニークではないから）。

### 3
* POJO にはフィール名が id のフィールドを定義しないようにする。MongoDBのドキュメントは挿入された時点で_idというキーが自動で割り振られるのだが、このキーと突合して処理が失敗し、以下のような例外が発生する。

```
exception: E11000 duplicate key error index:
```

【参考文献】

* [POJOs](http://mongodb.github.io/mongo-java-driver/3.5/bson/pojos/)
* [Quick Start](http://mongodb.github.io/mongo-java-driver/3.5/driver/getting-started/quick-start/)
* [Writing GraphQL mutations with Spring boot - g00glen00b](https://g00glen00b.be/graphql-mutations-spring/)

