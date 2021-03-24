Gradle のネイティブ Bom サポートについて

# 背景
- SpringBoot と Gradle で Web アプリケーションを開発している時、[spring-boot-project](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-project) のライブラリを複数使用するケースがある。この際ライブラリのバージョンを変更する際に、以下のように使用しているライブラリごとにバージョンを変更するのは、利用するライブラリが多ければ多いほど手間がかかる。
```kt:build.gradle.kts
dependencies {
	implementation("org.springframework.boot:spring-boot-starter-web:2.4.4")
	implementation("org.springframework.boot:spring-boot-actuator:2.4.4")
  ...
}
```
- この手間を極力減らすために、Gradle プロジェクトにおける BOM を使用した依存性管理の方法について調べた。

# BOMについて
- BOM とは Bill Of Materials の略で、プロジェクト全体の依存性を一括で管理できる仕組みである。BOMでバージョンを指定しさえすれば、関連するライブラリは特に指定せずとも BOM で指定されているバージョンのものが使用されることになる。以下は [Mavenのドキュメント](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html) で紹介されているBOMの例である。

```xml:pom.xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.test</groupId>
  <artifactId>use</artifactId>
  <version>1.0.0</version>
  <packaging>jar</packaging>
 
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>com.test</groupId>
        <artifactId>bom</artifactId>
        <version>1.0.0</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
    </dependencies>
  </dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>com.test</groupId>
      <artifactId>project1</artifactId>
    </dependency>
    <dependency>
      <groupId>com.test</groupId>
      <artifactId>project2</artifactId>
    </dependency>
  </dependencies>
</project>
```
出典: https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html

- 上記の例だと、`com.test` の BOM のバージョンに 1.0.0 が指定されているので、`com.test.project1` と `com.test.project2` のバージョンはどちらも 1.0.0 となる。

# Gradle プロジェクトでの実装例
- Gradle プロジェクトで BOM の機能を使用するためには、プラグインを適用する方法とネイティブの機能を利用する方法(Gradle のバージョンが6以降であることが必須)の2種類が存在する。ネイティブの機能を使用した方がビルドの実行時間を少なくできる可能性があるとのことなので、今回は後者の方法を採用した。build.gradle(.kts) に以下のような設定を追加する。

```kt:build.gradle.kts
dependencies {
	implementation(platform(org.springframework.boot.gradle.plugin.SpringBootPlugin.BOM_COORDINATES)) // Spring Boot の bom への依存関係を宣言
}

configurations.all {
    resolutionStrategy.eachDependency {
        if (requested.group == "org.springframework.boot") { // spring-boot-project のライブラリのバージョンを2.4.4に指定
            useVersion("2.4.4")
        }
    }
}
```

# 実行結果
## バージョンを "2.4.4" にした場合
```sh
$ ./gradlew build
...
+--- org.springframework.boot:spring-boot-starter-web -> 2.4.4
+--- org.springframework.boot:spring-boot-actuator -> 2.4.4
...
```

## バージョンを "2.4.3" にした場合
```sh
$ ./gradlew build
...
+--- org.springframework.boot:spring-boot-starter-web -> 2.4.3
+--- org.springframework.boot:spring-boot-actuator -> 2.4.3
...
```

# 参考資料
- https://docs.spring.io/dependency-management-plugin/docs/current/reference/html/
- https://spring.pleiades.io/spring-boot/docs/2.4.4/gradle-plugin/reference/htmlsingle/
- http://create-something.hatenadiary.jp/entry/2015/05/08/063000
- https://github.com/spring-projects/spring-boot/tree/master/spring-boot-project