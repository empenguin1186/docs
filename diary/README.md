<!-- TOC -->

- [2019 /08/26](#2019-0826)
    - [basename コマンド](#basename-コマンド)
    - [makeについて](#makeについて)
        - [参考文献](#参考文献)
    - [OpenSSL について](#openssl-について)
    - [デジタル署名とは](#デジタル署名とは)
    - [デジタル証明書](#デジタル証明書)
    - [zlib-devel について](#zlib-devel-について)
    - [pcre-devel について](#pcre-devel-について)
    - [登録されているサービス一覧を取得するコマンド](#登録されているサービス一覧を取得するコマンド)
    - [マークダウンで目次を自動生成する拡張機能](#マークダウンで目次を自動生成する拡張機能)
    - [rpm コマンド](#rpm-コマンド)
        - [インストールしているパッケージを表示](#インストールしているパッケージを表示)
        - [特定のパッケージの詳細を確認する](#特定のパッケージの詳細を確認する)
        - [パッケージに含まれるファイルの一覧を表示](#パッケージに含まれるファイルの一覧を表示)
        - [参考文献](#参考文献)
    - [rpm パッケージ作成の流れ](#rpm-パッケージ作成の流れ)
        - [参考文献](#参考文献)
- [2019 /08/27](#2019-0827)
    - [JMX について](#jmx-について)
- [2019 /08/28](#2019-0828)
    - [Markdown で表を自動生成してくれる拡張機能](#markdown-で表を自動生成してくれる拡張機能)
    - [直前のコミット操作を取り消す](#直前のコミット操作を取り消す)
    - [Apache Zookeeper](#apache-zookeeper)
- [2019 /08/29](#2019-0829)
    - [TCP について](#tcp-について)
    - [logback について](#logback-について)
        - [公式サイト](#公式サイト)
        - [ログレベルの重要度の順番](#ログレベルの重要度の順番)
        - [logback.xml の <root>タグ について](#logbackxml-の-rootタグ-について)
- [2019 /09/02](#2019-0902)
    - [nginx のエラーログ](#nginx-のエラーログ)
    - [Windows のhostsファイル](#windows-のhostsファイル)
    - [Iterm2 の画面分割](#iterm2-の画面分割)
- [2019 /09/03](#2019-0903)
- [2019 /09/04](#2019-0904)
    - [JavaにおけるJSONのパース](#javaにおけるjsonのパース)
    - [yum コマンド](#yum-コマンド)
        - [レポジトリの一覧を確認したい時](#レポジトリの一覧を確認したい時)
        - [yum update で指定したレポジトリを更新したい or したくない時](#yum-update-で指定したレポジトリを更新したい-or-したくない時)
- [2019 /09/17](#2019-0917)
    - [iterm2 で最大化 ON/OFF](#iterm2-で最大化-onoff)
- [2019 /09/20](#2019-0920)
    - [CNAMEについて](#cnameについて)
- [2019 /09/25](#2019-0925)
    - [yum パッケージの公開鍵における検証](#yum-パッケージの公開鍵における検証)
    - [127.0.0.1 アドレスってなんなの？](#127001-アドレスってなんなの)
    - [ls コマンドの -l オプション](#ls-コマンドの--l-オプション)
    - [ssh に失敗した時のログ](#ssh-に失敗した時のログ)
- [2019 /09/27](#2019-0927)
    - [td-agent について](#td-agent-について)
- [2019 /09/29](#2019-0929)
    - [git reflog](#git-reflog)
- [2019 /09/30](#2019-0930)
    - [Java の interface について](#java-の-interface-について)
- [2019 /10/01](#2019-1001)
    - [curl でレスポンスタイムを計測したい場合](#curl-でレスポンスタイムを計測したい場合)
    - [chrome でブックマークバーを表示するショートカット](#chrome-でブックマークバーを表示するショートカット)
    - [ssh の finger print について](#ssh-の-finger-print-について)
    - [ifconfig で自分のIPアドレスを確認する](#ifconfig-で自分のipアドレスを確認する)
    - [SSH で 「REMOTE HOST IDENTIFICATION HAS CHANGED!」 が出力された時の対処方法](#ssh-で-remote-host-identification-has-changed-が出力された時の対処方法)
- [2019 /10/02](#2019-1002)
    - [SSH接続時のセッションについて](#ssh接続時のセッションについて)
    - [どのプロセスがどのポートを使用しているかを確認する](#どのプロセスがどのポートを使用しているかを確認する)
    - [nginx 起動時の注意点](#nginx-起動時の注意点)
        - [参考文献](#参考文献)
- [2019 /10/03](#2019-1003)
    - [pid ファイルについて](#pid-ファイルについて)
- [2019 /10/07](#2019-1007)
    - [ITP(Intelligent Tracking Prevention)](#itpintelligent-tracking-prevention)
- [2019 /10/08](#2019-1008)
    - [pid ファイル](#pid-ファイル)
- [2019 /10/09](#2019-1009)
    - [クライアント証明書とサーバ証明書について](#クライアント証明書とサーバ証明書について)
- [2019 /10/09](#2019-1009)
    - [SIGSEGV について](#sigsegv-について)
    - [GDPR(General Data Protection Regulation)](#gdprgeneral-data-protection-regulation)
- [2019 /10/16](#2019-1016)
    - [トリアージ](#トリアージ)
    - [journald](#journald)
    - [UNIX ソケット](#unix-ソケット)
    - [rsyslog におけるモジュールについて](#rsyslog-におけるモジュールについて)
    - [rsyslog の ログローテートの設定](#rsyslog-の-ログローテートの設定)
        - [rsyslog による ログローテート の設定ファイルと設定確認方法](#rsyslog-による-ログローテート-の設定ファイルと設定確認方法)
    - [systemd コマンド一覧](#systemd-コマンド一覧)
    - [/etc ディレクトリについて](#etc-ディレクトリについて)
    - [go の time パッケージの Unix について](#go-の-time-パッケージの-unix-について)
- [2019 /10/17](#2019-1017)
    - [rsyslog.conf について](#rsyslogconf-について)
    - [systemd のファシリティについて](#systemd-のファシリティについて)
- [2019 /10/18](#2019-1018)
    - [VsCode でタブを開くたびに上書きされる問題](#vscode-でタブを開くたびに上書きされる問題)
- [2019 /10/21](#2019-1021)
    - [循環的(サイクロマティック)複雑度](#循環的サイクロマティック複雑度)
    - [クラス図に関して](#クラス図に関して)
- [2019 /10/24](#2019-1024)
    - [データレース(データ競合)について](#データレースデータ競合について)
    - [308 Permanent Redirect](#308-permanent-redirect)
    - [MAC アドレスと IP アドレスの違い](#mac-アドレスと-ip-アドレスの違い)
    - [ネットワークスイッチ](#ネットワークスイッチ)
    - [コアスイッチ](#コアスイッチ)
- [2019 /10/28](#2019-1028)
    - [syslog が使用しているポート](#syslog-が使用しているポート)
    - [rsyslog の設定を有効にするには](#rsyslog-の設定を有効にするには)
    - [tcpdump で所定のポートにリクエストが飛んでいるかどうかを確認する。](#tcpdump-で所定のポートにリクエストが飛んでいるかどうかを確認する)
- [2019 /10/29](#2019-1029)
    - [grep コマンド](#grep-コマンド)
    - [syslog で ファイルに出力されたログの内容を syslog で扱えるようなストリームに変換するには？](#syslog-で-ファイルに出力されたログの内容を-syslog-で扱えるようなストリームに変換するには)
- [2019 /11/1](#2019-111)
    - [ロードバランサーを経由したリクエストの送信元 IP アドレスはどこに格納される?](#ロードバランサーを経由したリクエストの送信元-ip-アドレスはどこに格納される)
- [2019 /11/5](#2019-115)
    - [kill -HUP <プロセス>](#kill--hup-プロセス)
    - [systemd 各種コマンド](#systemd-各種コマンド)
- [設定値反映](#設定値反映)
- [設定値反映](#設定値反映)
- [ステータス表示](#ステータス表示)
- [service 一覧表示](#service-一覧表示)
- [サービス定義ファイル編集](#サービス定義ファイル編集)
    - [intelliJ で特定のインターフェースを実装したクラスの一覧を表示するショートカット](#intellij-で特定のインターフェースを実装したクラスの一覧を表示するショートカット)
- [2019 /11/6](#2019-116)
    - [シェルスクリプトでファイルの存在確認について](#シェルスクリプトでファイルの存在確認について)
- [2019 /11/15](#2019-1115)
    - [MIT ライセンスはどのようなライセンスか。](#mit-ライセンスはどのようなライセンスか)
    - [git add したファイルのHEADとの差分を確認するコマンドを答えよ](#git-add-したファイルのheadとの差分を確認するコマンドを答えよ)
- [2019 /11/17](#2019-1117)
    - [html において br は何の略か](#html-において-br-は何の略か)
    - [vscode でタブのスペース数を変更する](#vscode-でタブのスペース数を変更する)
- [2019 /11/23](#2019-1123)
    - [JJUG](#jjug)
        - [資料一覧](#資料一覧)
        - [Java プログラマのための頑張らない Go 入門](#java-プログラマのための頑張らない-go-入門)
        - [Java によるクラウドネイティブの実現に向けて](#java-によるクラウドネイティブの実現に向けて)
        - [開けドメイン駆動設計の扉](#開けドメイン駆動設計の扉)
            - [モチベーション](#モチベーション)
            - [ドメインとは](#ドメインとは)
        - [関連リンク](#関連リンク)
- [2019 /11/27](#2019-1127)
    - [シューマイ](#シューマイ)
        - [Kotlin × Spring 5.2](#kotlin-×-spring-52)
            - [リアクティブとは](#リアクティブとは)
            - [ライブコーディング](#ライブコーディング)
        - [Kotlin Croutines Flows](#kotlin-croutines-flows)
            - [Hot Stream Cold Stream](#hot-stream-cold-stream)
            - [Flow の　利点](#flow-の　利点)
        - [Kotlin で書く Gradle Custom Tasks](#kotlin-で書く-gradle-custom-tasks)
- [NEXT ACTION](#next-action)

<!-- /TOC -->

# 2019/08/26

## basename コマンド

ファイルの名前のみを切り出すコマンド。
```
$ basename /home/kodaira/nginx-1.16.1.tar.gz
nginx-1.16.1.tar.gz
```

## makeについて

Makefileに定義されている処理を実行することができる。以下は全てのオブジェクトファイルを削除するルールを定義している。
```Makefile
clean:
    rm -f *.o
```

特定のファイルに対して実行する場合は以下のような書き方も可能。
```Makefile
print: *.c
    lpr -p $?
    touch print
```

```Makefile
clean:
    make -C src $@
```
`-C <dir>` オプションは指定したディレクトリに移動してからルールを実行するという意味。 `$@` はターゲット名を表す変数。

### 参考文献
[GNU make 日本語訳(Coop編) - 目次](https://www.ecoop.net/coop/translated/GNUMake3.77/make_toc.jp.html)

## OpenSSL について
OpenSSL とはクライアントとサーバの間で行われる通信を暗号化するプロトコルである SSLおよびTLS のオープンソースのライブラリのことを指す。

## デジタル署名とは

- メッセージの送信者はメッセージを送る相手に公開鍵を送る。  
- 続けてメッセージの送信者はメッセージとメッセージのハッシュ値を秘密鍵で暗号化したものを相手に送る。  
- 受信した相手側は暗号化されたハッシュ値を公開鍵で復号して、送られてきたメッセージのハッシュ値を計算し値が一致しているかを確認する。  
デジタル署名の利点としては送信元のみが秘密鍵を保持しているために送信元を一意に特定できること、加えて署名(ハッシュ値)を作成できるのは送信元のみであるから、受信元が署名を捏造するという可能性がなくなるという点である。

## デジタル証明書

デジタル署名では送信元を一意に特定できるという利点があったが、それでも送信元が想定していた相手だと証明できない欠点が存在する。その欠点を克服したのがデジタル証明書である。

流れとしては以下のようになる。
- 送信元は認証局へメールアドレスやドメインを含んだ証明書を発行してもらう。この証明書は認証局自身の秘密鍵で暗号化されている。  
- 送信元は受信元に証明書を送る。  
- 証明書を受信した相手は認証局から公開鍵を取得し、証明書を復号する。正しく復号できればこれは認証局から発行された証明書であることが証明される。  
- あとは復号されたデータから送信元本当に通信を行いたい相手なのかどうかを確認(メールアドレスやドメインから判断)し、そうであれば通信を行う。

## zlib-devel について

> zlibは、データの圧縮および伸張を行うためのフリーのライブラリである

【引用】[zlib - Wikipedia](https://ja.wikipedia.org/wiki/Zlib)

## pcre-devel について

perl の正規表現を利用できるパッケージ？

> Perl Compatible Regular Expressions (PCRE) is a library written in C, which implements a regular expression engine, 

【引用】：[Perl互換の正規表現-ウィキペディア](https://en.wikipedia.org/wiki/Perl_Compatible_Regular_Expressions)

## 登録されているサービス一覧を取得するコマンド

```
$ systemctl list-unit-files --type=service
```

## マークダウンで目次を自動生成する拡張機能

- Markdown TOC をインストール。
- 以下を参考にして設定を行う。  
[【VSCode】Markdownで書いたブログに目次を付ける！（Markdown TOC） ｜ DevelopersIO](https://dev.classmethod.jp/tool/vscode-markdown-toc-for-blog/)

## rpm コマンド

### インストールしているパッケージを表示

```
$ rpm -qa
```
grep と組み合わせるのが鉄板

### 特定のパッケージの詳細を確認する

```
$ rpm -qi <パッケージ名>
```

### パッケージに含まれるファイルの一覧を表示

```
$ rpm -ql <パッケージ名>
```

### 参考文献
[おしえて、rpmコマンド! - Qiita](https://qiita.com/hijili/items/899522b9cd2fd1140ad2)

## rpm パッケージ作成の流れ

- ソースコードを作成する。  
- spec ファイルを作成する。  
- rpmbuild コマンドを実行して.rpm ファイルを生成する。
- `sudo rpm -ivh <package>`を実行してパッケージをインストールする。

### 参考文献
[こわくない、rpmbuild - Qiita](https://qiita.com/hijili/items/ab2e162acbe05f86b7b0)

# 2019/08/27

## JMX について

> Java Management Extensions (JMX) は、Java アプリケーションをモニタおよび管理するための仕様です。JMX を使用すると、汎用管理システムでアプリケーションをモニタし、注意が必要なときに通知を生成し、アプリケーションの状態を変更して問題を解決できます。

アプリケーションの状態を記録している Bean である。自作することが可能。jolokia というツールを使用すると、HTTP で MBean の状態を取得することができる。

参考：[JMX について](http://otndnld.oracle.co.jp/document/products/wls/docs103/jmxinst/understanding.html)

# 2019/08/28

## Markdown で表を自動生成してくれる拡張機能
[Markdown のテーブルを直感的に生成できる VSCode の拡張機能を作った - Qiita](https://qiita.com/7ma7X/items/d044e64918fa9bd4c92a)

## 直前のコミット操作を取り消す
```
q
```

## Apache Zookeeper
> Apache ZooKeeper（アパッチ ズーキーパー）は Apacheソフトウェア財団のオープンソースプロジェクトで、大規模分散システムでよく利用される、設定情報の集中管理や名前付けなどのサービスを提供するソフトウェアである。最初はHadoopのサブプロジェクトの一つであったが、現在はトップレベルプロジェクトの一つになっている。  

引用：[Apache ZooKeeper - Wikipedia](https://ja.wikipedia.org/wiki/Apache_ZooKeeper)  
キューみたいに使用できるか？

# 2019/08/29

## TCP について

>　TCP (Transmission Control Protocol) は、IPと同様にインターネットにおいて標準的に利用されている
　プロトコルです。TCPは、IPの上位プロトコルでトランスポート層で動作するプロトコル。ネットワーク
　層のIPとセッション層以上のプロトコル（例：HTTP、FTP、Telnet) の橋渡しをする形で動作しています。  

【参考文献】：[TCP/IP - TCPとは](https://www.infraexpert.com/study/tcpip7.html)

サーバにHTTPでリクエストする場合は以下の手順を踏む。  
- DNS サーバに問い合わせてホスト名からIPアドレスを取得する  
- IPアドレスを取得したら、そのIPアドレスに向けてTCPで接続を確立する。 
- 接続を確立したら、HTTPでリクエストを行う。

## logback について

### 公式サイト
[Logbackマニュアル](https://logback.qos.ch/manual/index_ja.html)

### ログレベルの重要度の順番

```
TRACE < DEBUG < INFO < WARN < ERROR
```

### logback.xml の <root>タグ について
logback.xml の必須要素。 `level` 属性でログレベルを指定することでそれより重要度が低いログの出力を抑制することができる。また子要素に `<appender-ref>` を挿入することで `<root>` タグの設定を指定した appender に反映することができる。  
【参考文献】：[logback.xmlを設定する | Java好き](https://javazuki.com/articles/slf4j-logback-usage.html)

# 2019/09/02

## nginx のエラーログ
/var/log/nginx/error.log

## Windows のhostsファイル
C:¥Windows¥System32¥drivers¥etc¥hosts

## Iterm2 の画面分割

|  アクション  |  ショートカット  |
|:--:|:--:|
|  縦分割  |  cmd + d  |
|  横分割  |  cmd + shift + d  |
|  前のペインに移動  |  cmd + [  |
|  次のペインに移動  |  cmd + ]  |

# 2019/09/03

# 2019/09/04

## JavaにおけるJSONのパース
JSONのnumber型を読み込むときはすべてDoubleになる。

## yum コマンド

### レポジトリの一覧を確認したい時
```
$ yum repolist [all|enabled|disabled]
```

### yum update で指定したレポジトリを更新したい or したくない時
```
$ sudo yum --disablerepo "hoge" --enablerepo "fuga" update -x "piyo"
```
|  オプション  |  内容  |
|:--:|:--:|
|  --disablerepo  |  アップデートを行わないレポジトリを指定。*で一括指定も可能  |
|  --enablerepo  |  アップデートを行うレポジトリを指定。  |
|  -x  |  レポジトリに含まれる指定したパッケージのアップデートを行わない  |

# 2019/09/17

## iterm2 で最大化 ON/OFF
Command + Enter

# 2019/09/20
## CNAMEについて
[CNAMEレコードとは｜「分かりそう」で「分からない」でも「分かった」気になれるIT用語辞典](https://wa3.i-3-i.info/word12290.html)

# 2019/09/25

## yum パッケージの公開鍵における検証
[1.5.2. 署名パッケージの検証 Red Hat Enterprise Linux 6 | Red Hat Customer Portal](https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/6/html/security_guide/sect-security_guide-updating_packages-verifying_signed_packages)  

## 127.0.0.1 アドレスってなんなの？
ループバックアドレスと呼ばれ、自身を表すIPアドレス。

## ls コマンドの -l オプション

```
$ ls -la
合計 ***
drwxr-xr-x    1 hogehoge    hogehoge        4096  Jun  5 19:13 foo
-rw-r--r--    1 fugafuga    fugafuga        1234  Jun 14 17:15 bar
```
左からパーミッション、所有者、所属グループ、ファイルサイズ、更新日。

## ssh に失敗した時のログ
サーバー側の `/var/log/secure` を確認してどんなエラーが発生しているかを確認する。
```
$ sudo less /var/log/secure
Sep 25 11:00:05 <xxxx> sshd[22803]: Authentication refused: bad ownership or modes fo
r directory <directory path>
```
今回はディレクトリの権限関連でSSHが失敗しているとみられる。SSH 先のカレントディレクトリが自身のアカウントの所有になっていないとSSHが失敗する。

# 2019/09/27

## td-agent について
td-agent は Output に　Chunk が複数存在する場合は、ある Chunk のリクエストに失敗した時にはリトライまでのインターバルの間に他の Chunk のリクエストが行われる。

# 2019/09/29

## git reflog
`git reset --soft ^HEAD` でコミットの取り消しをしようとした時に、間違って取り消しをする必要のないコミットも取り消してしまった。そういう時は、 `git reflog` で HEAD がどのように変化したかが表示されるので、それを元に任意の状態に戻すことができる。

```
$ git reflog
718654b (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
3dbc7bf HEAD@{1}: reset: moving to HEAD~
22bb5a3 (origin/master) HEAD@{2}: reset: moving to HEAD~
ae73b10 (origin/feature/vr-tutorial, feature/vr-tutorial) HEAD@{3}: checkout: moving from feature/vr-tutorial to master
ae73b10 (origin/feature/vr-tutorial, feature/vr-tutorial) HEAD@{4}: checkout: moving from master to feature/vr-tutorial
ae73b10 (origin/feature/vr-tutorial, feature/vr-tutorial) HEAD@{5}: commit: :+1: VR アプリ作成でつまづいた点を記録
22bb5a3 (origin/master) HEAD@{6}: pull origin mast
```
HEAD@{戻りたい番号} を指定して以下のコマンドを実行する。
```
$ git reset --hard HEAD@{3}
```

# 2019/09/30

## Java の interface について
interface でフィールドを定義すると、自動的に `public static final` 修飾子が付与される。

# 2019/10/01

## curl でレスポンスタイムを計測したい場合
[curl でレスポンスタイムを計測 - Please Sleep](http://please-sleep.cou929.nu/curl-write-out-option.html)

## chrome でブックマークバーを表示するショートカット
cmd + shift + B

## ssh の finger print について
finger print とはまさしく SSH先のサーバの指紋のことをさす。具体的には SSH先サーバの公開鍵である。初めてSSHを実行する場合はこの finger print を保存するかどうかのメッセージが表示され、yes を選択すると、`~/.ssh/known_hosts`に finger print が保存され、次回以降 SSH を実行しても finger print は表示されない。

## ifconfig で自分のIPアドレスを確認する

```
$ ifconfig
eth0: flags=...
        inet <IP_ADDRESS>  netmask <xxx>  broadcast <xxx>
        ...
```

## SSH で 「REMOTE HOST IDENTIFICATION HAS CHANGED!」 が出力された時の対処方法
[SSH接続で WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED! って言われて接続を拒否られるとき - Qiita](https://qiita.com/wnoguchi/items/690f3f4651f8f11e4ed3)

# 2019/10/02

## SSH接続時のセッションについて
SSH を実行するたびにセッションが貼られる。

## どのプロセスがどのポートを使用しているかを確認する

```
$ sudo lsof -nPi
ntpd      958     ntp   16u  IPv4  20596      0t0  UDP *:123
ntpd      958     ntp   17u  IPv6  20597      0t0  UDP *:123
...
```
ちなみに `lsof` は `list open file` の略。プロセスなんかが開いているファイルをリストアップするコマンド。

## nginx 起動時の注意点
設定ファイルを指定するときは絶対パスで指定すること。

### 参考文献
Contextを制するの者はnginxを制する!! - Qiita
https://qiita.com/sakajunquality/items/04325532f67312a99ad9

# 2019/10/03
## pid ファイルについて
プロセス識別子 - Wikipedia
https://ja.wikipedia.org/wiki/%E3%83%97%E3%83%AD%E3%82%BB%E3%82%B9%E8%AD%98%E5%88%A5%E5%AD%90  

> 長時間動作し続けるプロセス（例えば MySQL デーモンなど）は、自身のPIDをファイルに書いておき、他のプロセスが参照できるようにしていることがある。

# 2019/10/07
## ITP(Intelligent Tracking Prevention)
ユーザの望まないような広告などを出さないために、Appleが開発したクッキーによるトラッキングを機械学習によって規制する技術

# 2019/10/08
## pid ファイル
プロセス番号を格納するファイル。他のプロセスはこのファイルを参照してどのプロセスがどのプロセス番号を使用しているのかを確認することができる。大体 /var/run/<アプリケーション> 配下に設置される。

# 2019/10/09
## クライアント証明書とサーバ証明書について
クライアント証明書は、クライアントの正当性を証明するための証明書。例えば社内でしか利用できないWebサイトを製作したい場合、IDとパスワードだけでは外部からでもアクセスできてしまう。しかしクライアント証明書を用いると、そのクライアント証明書をインストールしている端末のみからアクセスすることができるような状況を作り出すことができる。  
一方サーバ証明書はサーバの正当性を証明するためのものである。具体的にいうと証明書を発行している認証局が正規のものなのか、その証明書がアクセスしようとしているサーバのものなのかを確認するためのものである。

# 2019/10/09
## SIGSEGV について
ソフトウェアの実行時にアクセスが許可されていないメモリにアクセスしようとしてセグメンテーション違反が起こった場合にそのプロセスが受け取る信号のことである。不正なメモリにアクセスするケースとしては、システムで仕様しているメモリにアクセスした場合や、Read Only のメモリにアクセスした場合などが考えられる。  

## GDPR(General Data Protection Regulation)


# 2019/10/16

## トリアージ
大事故・災害などで同時に多数の患者が出た時に、手当ての緊急度に従って優先順をつけること。  
例) インフルエンザの予防接種を優先的に医者が受けられる。

## journald 
サービスプロセスの 標準出力、エラー出力、sys ログメッセージなどを収集しているデーモン。sysログメッセージに関しては rsyslog にもログを転送している。

## UNIX ソケット
プロセス間で通信を行う時に使用できるソケット(通信の接続口)。  
【参考】: [Unixドメインソケットの確認 - Qiita](https://qiita.com/nk_yohn3301/items/7aec184e290940052ed2)

## rsyslog におけるモジュールについて

|  モジュール名  |  説明  |
|:--:|:---|
|  imuxsock  |  UNIX　ソケットを経由してログを受け取る。  |
|  imjournal  |  journald からのログを受け取る。  |

その他のモジュールに関しては以下を参照  
[多機能なログ管理システム「rsyslog」の基本的な設定 | さくらのナレッジ](https://knowledge.sakura.ad.jp/8969/)

## rsyslog の ログローテートの設定
- [任意のログをlogrotateを使って管理する - Qiita](https://qiita.com/Esfahan/items/a8058f1eb593170855a1)  
- [rsyslogを利用したログファイル作成と、logrotateを利用したログのローテーション | OXY NOTES](https://oxynotes.com/?p=6493)

### rsyslog による ログローテート の設定ファイルと設定確認方法

/etc/logrotate.conf がログローテートの設定で、以下のコマンドを実行すると、設定が正しいかどうかを確認できる。
```
logrotate -dv /etc/logrotate.conf
```
-d は dryrun モードで実行することを指す。 -v はコマンドの詳細な内容が表示される。

## systemd コマンド一覧
[はじめてのsystemdサービス管理ガイド ｜ DevelopersIO](https://dev.classmethod.jp/cloud/aws/service-control-use-systemd/)

## /etc ディレクトリについて
システムの設定ファイルを格納するためのディレクトリ。

## go の time パッケージの Unix について
https://golang.org/pkg/time/#Unix  
UNIXTIME から time に変換する関数。  

```go
Unix(sec int64, nsec int64)  
```
sec は UNIXTIME(秒), nsec は ナノ秒

# 2019/10/17

## rsyslog.conf について
- [22.2. Rsyslog の基本設定 Red Hat Enterprise Linux 7 | Red Hat Customer Portal](https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/7/html/system_administrators_guide/s1-basic_configuration_of_rsyslog)　　
- [CentOS7のrsyslog.confの設定値を解剖してみよう - Qiita](https://qiita.com/pa_pa_paper/items/e2c886fa65d32a7442a1)

## systemd のファシリティについて
- [システム管理の基礎 syslogdの設定をマスターしよう (2/3)：Linux管理者への道（3） - ＠IT](https://www.atmarkit.co.jp/ait/articles/0209/07/news002_2.html)

# 2019/10/18

## VsCode でタブを開くたびに上書きされる問題

setting -> workbench -> enablePreview を OFF にする。  
【対処方法】  
https://qiita.com/kgoto/items/265e3eb8a82778e33cfe

# 2019/10/21

## 循環的(サイクロマティック)複雑度
[循環的複雑度について - Qiita](https://qiita.com/yut_arrows/items/16749e02313109071338)

## クラス図に関して

線の特徴については以下の通り  

|  線  |  説明  |
|:--:|:---|
|  実線  |  通称リンク。基本的にオブジェクト間に何かしらの関係がある場合に使われる。  |
|  矢印付き実線  |  通称関連。リンクの一種で、オブジェクト間に has a  の関係が存在するときに記述。 |
|  ひし形付き実線  |  通称集約。関連の一種で、オブジェクト間に a part of の関係が存在するときに記述。  |

# 2019/10/24

## データレース(データ競合)について
あるデータの処理中に別の処理が割り込んでくること。これは回避すべき案件。  
https://qiita.com/yohhoy/items/00c6911aa045ef5729c6

## 308 Permanent Redirect
要求したリソースが `Location` ヘッダに記載されている URL に完全に移行したことを示す。したがって `Location`ヘッダに記載されている URL にリクエストを実行する。  
【参考文献】 https://developer.mozilla.org/ja/docs/Web/HTTP/Status/308  

## MAC アドレスと IP アドレスの違い
MAC アドレスは、個々のデバイスを一意に特定するためのアドレスを指す。IP アドレスはデータの送信先を特定するためのアドレスのことをさす。MAC アドレスは変更できないが、IPアドレスは変更することができる。  
【参考文献】：[MACアドレスとIPアドレスの違い - Qiita](https://qiita.com/cironaga/items/6a0c909e31f986c9f825)

## ネットワークスイッチ
流れてきたデータの情報を元に送信元を特定して送信する機器。プロトコル階層によって種類が異なり、IP アドレスを元に送信先を特定するスイッチは L3 スイッチと呼ばれる。  
【参考文献】：[ネットワークスイッチとは - IT用語辞典 e-Words](http://e-words.jp/w/%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF%E3%82%B9%E3%82%A4%E3%83%83%E3%83%81.html)

## コアスイッチ
主要拠点間をつなぐコアネットワークで用いられるスイッチ製品。

# 2019/10/28

## syslog が使用しているポート
514 番ポート。ちなみに UDP と TCP どちらも受け付けている。

## rsyslog の設定を有効にするには
rsyslog は systemd が受け付けたログに対して動作するので、ちゃんとログが吐き出されるかを確認したい場合は、`systemctl restart <サービス名>` コマンドを打ち込まないとちゃんと吐き出されない。

## tcpdump で所定のポートにリクエストが飛んでいるかどうかを確認する。
```
$ sudo tcpdump src port <任意のポート番号>
```

# 2019/10/29

## grep コマンド
grep で　ignore case で引っ掛ける
```
$ grep -i ~
```

## syslog で ファイルに出力されたログの内容を syslog で扱えるようなストリームに変換するには？
`imfile`モジュールを使用する。
```conf
module(load="imfile") # (1)
 
input(type="imfile" 
      File="/var/log/sample/hoge.log" # (2)
      Tag="mua-gated" # (3)
      Facility="local0"# (4)
      Severity="info") # (5)
local0.info      @@<syslog サーバの IP アドレス>:514 # (6)
```

|  項目  |  説明  |
|:--:|:---|
|  (1)  |  ファイルのログを読み込むために必要なモジュール  |
|  (2)  |  取得先のファイルを定義する  |
|  (3)  |  タグを付与する  |
|  (4)  |  Facility を設定する  |
|  (5)  |  Serverity を設定する  |
|  (6)  |  出力先を定義する。「@@」はTCP で送信を意味する  |

Facility と Serverity の値は [ここ](https://knowledge.sakura.ad.jp/8969/) を参照。Facility に関しては　`local0~7`を使用するのが無難か。  
他の設定値は [ここ](http://www.l3jane.net/doc/rsyslog/configuration/modules/imfile.html?highlight=inputfileseverity) を参照。

# 2019/11/1

## ロードバランサーを経由したリクエストの送信元 IP アドレスはどこに格納される?
IP アドレスを格納するヘッダには「REMOTE_ADDR」と「X-Forwarded-For」が存在する。ロードバランサーを介したリクエストは「REMOTE_ADDR」ヘッダにロードバランサーのIPアドレスが格納され、「X-Forwarded-For」ヘッダにはクライアントのIPアドレスが格納される。  
【参考文献】http://3.1415.jp/kc47fh1k/

# 2019/11/5

## kill -HUP <プロセス>
プロセスに設定の変更や再起動、停止などといった命令はシグナルと呼ばれている。このシグナルは、命令の種類によって決められており、設定の変更をプロセスに教えるシグナルは HUP と呼ばれる。使い方は以下。  
```
$ kill -HUP <proccess>
```
シグナルは他にも色々な種類が存在する。  
https://www.itmedia.co.jp/help/tips/linux/l0689.html

## systemd 各種コマンド

```
# 設定値反映
$ systemctl daemon-reload

# ステータス表示
$ systemctl status hoge.service

# service 一覧表示
$ systemctl list-units --type=service

# サービス定義ファイル編集
$ vim /etc/systemd/system/hoge.service
```

## intelliJ で特定のインターフェースを実装したクラスの一覧を表示するショートカット
Command + Option + B

# 2019/11/6

## シェルスクリプトでファイルの存在確認について

```
if [-d /etc/hoge]; then
    <command>
else
    <command>
fi
```
【参考文献】https://qiita.com/nii_yan/items/73f4caacdc2ca6f45135

# 2019/11/15

## MIT ライセンスはどのようなライセンスか。

このソフトウェアを誰でも(1)で(2)に扱って良い。ただし、(3)および(4)をソフトウェアのすべての複製または重要な部分に記載しなければならない。
作者または著作権者は、ソフトウェアに関してなんら(5)を負わない。

1. 無償
2. 無制限
3. 著作権表示
4. 本許諾表示
5. 責任

## git add したファイルのHEADとの差分を確認するコマンドを答えよ

git diff --cached ファイル名

# 2019/11/17
## html において br は何の略か
Bbreak. 改行を表す。

## vscode でタブのスペース数を変更する
cmd + , => editor.tabSize の値を任意の値にする

# 2019/11/23
## JJUG

### 資料一覧
- https://www.slideshare.net/masuda220/java-objectoriented-programming-primer  
- https://speakerdeck.com/saiya_moebius/distributed-tracing-case-study  
- https://www.slideshare.net/yyyank/javago  
- https://qiita.com/opengl-8080/items/c482998fa15ce738e2ba  
- 

### Java プログラマのための頑張らない Go 入門

https://www.slideshare.net/yyyank/javago

【紹介文献】  
- https://qiita.com/tenntenn/items/0e33a4959250d1a55045  
- https://golang.org/doc/faq  
- https://www.lambdanote.com/products/go  
- http://go.shibu.jp/effective_go.html  
- http://tmrts.com/go-patterns/#creational-patterns  
- https://github.com/avelino/awesome-go  
- https://github.com/knsh14/uber-style-guide-ja  
- https://qiita.com/sonatard/items/9c9faf79ac03c20f4ae1  

トリコロールマーク & スイープ GC  
Java は Google Java format 

### Java によるクラウドネイティブの実現に向けて

https://jjug-cfp.cfapps.io/submissions/d7ea35d2-a4a2-4516-b516-832f289f8adc

【ナレッジ】  
- https://alpinelinux.org/  
- https://qiita.com/tshk_mtsys/items/5a027fe00b3bff009b17  
- https://qiita.com/koduki/items/5a1b5e5da95a21935d18

### 開けドメイン駆動設計の扉

#### モチベーション
コードが仕様を体現していること。プロジェクトの初期段階で仕様を固めたコードが数年経過しても保守性の高いものとなる。

#### ドメインとは
ソフトウェアを適用する対象領域のこと

### 関連リンク
- http://www.springframework.jp/  
- https://quarkus.io/  
- https://qiita.com/mr-hisa-child/items/92e0417460adc328596a  
- https://micronaut.io/  
- https://speakerdeck.com/nrslib

# 2019/11/27

## シューマイ
yenta : コミュニティでコンタクトすることが可能

### Kotlin × Spring 5.2
kotlin 1.3
Boot 2.2 -> kotlin BOM
Coroutine を使ってかけるようになった

#### リアクティブとは
効率よくリクエストを捌くことができること.

WebFlux  : 一つのスレッドで 複数のリクエストを処理できるようにする  
R2DBC, ADBA : スレッドを専有せずに処理できるデータベースドライバ。

Coroutine : Kotlin らしくリアクティブな処理をかけるようになった。

#### ライブコーディング
spring data r2dbc  
mysql driver  

@Id => AutoIncrement したいフィールドに付与する  
@JsonIgnore => JSON にフィールドの含めなくする  

ReactiveCrudRepository<ItemEntity, Int>  
第二引数の Int は Id の型を指定する。  

.asFlow() : Croutine の型に変換する。  
https://www.slideshare.net/HayatoKihara/kotlin-fest-2019-spring-kotlin-166057015

### Kotlin Croutines Flows
Flow => Croutine で Cold Stream を取り扱うための API
@ExperimentalCoroutinesApi => 試験段階のAPI  

#### Hot Stream Cold Stream

Cold Stream => 複数のスレッドで同一のFlowを呼び出しても状態を共有しない

#### Flow の　利点

### Kotlin で書く Gradle Custom Tasks
buildSrc : このディレクトリにスクリプトを設定すると、そこにタスクを書いていく。
openclass D
DefaultTask を継承

@TaskAction をつけたメソッドの処理が実装される

https://guides.gradle.org/writing-gradle-tasks/

Jgit : Github の API を叩くライブラリ 

https://www.slideshare.net/HayatoKihara/kotlin-fest-2019-spring-kotlin-166057015
https://speakerdeck.com/keithyokoma/kotlin-deshu-ku-gradle-custom-tasks
https://speakerdeck.com/kr9ly/kotlincoroutinesflowkotohazime?slide=8

# 2019/12/4
## golang.tokyo

### Yapli とは
アプリ開発クラウド。  
ブラウザだけでアプリが開発できる。  
Yahoo も導入している  

### wire.go 
DI ツール

### 便利なTUIツール
ゴリラさん  「vim が好きになる本」  
lazygit : インタラクティブに git 操作ができる
docui : イメージの検索、コンテナの起動などを非同期で実行できる  
pst : プロセスビューア, プロセス名で検索することができる。さらに子プロセスも検索できる
ff : 簡易ファイラー  
tson : json を解析できる  

### twilter
twilter : twitter をフィルタリングするサービス

### インラインフィルター
objdump : 逆アセンブリするツール  
pragma

### MarkDown
blackfriday : MarkDown を解析するツール
go/format Source 関数でフォーマットを解析できる  

### grpc api 
grpc

### 初めてのGo開発
gopher 道場

# 2019/12/16

## シェルスクリプトで繰り返し処理を実装するときのテンプレート

```sh
#!/usr/bin/bash
#echo "test"が10回実行される。
for i in `seq 10`
do
echo "test"
done
```

# 2020/01/05

## ドットインストール html

- img タグにおける alt 属性は、画像が表示されなかった場合に出力される文章である。  
- p タグの p はパラグラフの p である  
- タグや属性に全角文字を使用すると、html がうまく表示されなくなるので、注意する。  
- タグを含めた文章全体にもタグを付与する必要がある。それが、<!DOCTYPE html> である。  
- <html lang="ja"></html> -> この文章が html であるということを宣言する。lang 属性で日本語のページであることを示す。  
- あとは <head> と <body> タグを付与してページを作成していく。

# NEXT ACTION
- rpmdev-setuptreeについて調べる  
- Makefile の `type` 句は何か調べる
- jolokia 使ったアプリケーションの作成
- ZooKeeper についてできることを調べてみる
- smtp のリクエスト方法を調べる
- logaback.xml の append について調べる
- SSH先のサーバはどこに公開鍵の情報を保持しているのか？