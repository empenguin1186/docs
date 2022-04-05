# Jackson で Enum のフィールドの値をJSONに出力する
Jackson で Enum の JSON シリアライズ結果をカスタマイズする - Qiita
https://qiita.com/t-otsuka/items/ef7cd9cac84b2c1915c1

# フィールドとプロパティの違い
プロパティ = フィールド + アクセサメソッド

# Spring によるトランザクション管理
Springでトランザクション管理 - Qiita
https://qiita.com/NagaokaKenichi/items/a279857cc2d22a35d0dd

# Kotlin における internal について
修飾子。Javaにおけるパッケージプライベートがこれに近い。違いとしては、Javaのパッケージプライベートは他のプロジェクトで同じパッケージが存在すると、そこからでもアクセスできてしまうが、Kotlinのinternal はこれを防ぐ。  
Kotlin - 可視性 - 覚えたら書く  
https://blog.y-yuki.net/entry/2019/05/22/090000

# Kotlin の Triple について
一気に値を3つ返すことができる関数  
https://qiita.com/sdkei/items/2d5dab51b53975286945

# Mockk の relaxed について
https://mockk.io/

# コードでDbのセットアップを行いたいとき
DbSetup, from Ninja Squad - User guide  
http://dbsetup.ninja-squad.com/user-guide.html

# テストでDIコンテナに登録されているBeanを置き換えたい時どうするか
- @MockBean アノテーションを使用する
  - https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-testing  
- Spring BootでAutowiredされるクラスをMockitoでモックする - Qiita  
  - https://qiita.com/Plemling138/items/c48b3805cafa9dec7675

# Runwith(SpringTest::class) は何のために付与する？
AutowiredなどのDIの機能をテストで使用したい場合に付与する。
https://qiita.com/Plemling138/items/c48b3805cafa9dec7675

# PlatformTransactionManagerの役割について
トランザクションを管理しているオブジェクト。デフォルトでDIされている。

# Spring Boot と Spring Framework の違い

アプリ開発を行うときに使用するのがSpringBootで、アプリ開発で使用するデータベースのトランザクション管理やメッセージング機能を提供しているのがSpringFrameworkである。  
https://spring.io/projects

# style の継承
CSSにおいてどのプロパティが継承されるかはMDNのサイトで確認できる
https://developer.mozilla.org/ja/

# Kotlin の @Throws アノテーションについて

Kotlin のコードをJavaで使用するときに、Kotlinは検査例外と非検査例外の区別がついていないために、Javaではcatchすることができない。catchするためには、Kotlin側で@Throwsアノテーションを付与する必要がある。したがって、呼び出し側で例外のハンドリングが必要な場合には@Throwsアノテーションを付与するのが望ましいと考える。    
JavaのプロダクトをKotlinに移行してみた話 - Qiita  
https://qiita.com/noripi/items/0649415c84b08d1de3bb  

# @Transactional アノテーションは同一クラスでは機能しない
Spring Boot におけるトランザクション処理 (MyBatis/MySQL) - Qoosky  
https://www.qoosky.io/techs/400f6b6f09

# Spring の Pointcut について

- Spring AOP Pointcutについて - Qiita  
  - https://qiita.com/mtsu724/items/6f5bb5c4a70af432df31  
- Spring AOP ポイントカット指定子の書き方について - Qiita  
  - https://qiita.com/rubytomato@github/items/de1019aeaaab51c8784d  
 
# KotlinのTODO関数について
未実装のメソッドについては、TODO関数を使うと、Exceptionを投げてくれる。  
TODOタスクをソースに残す - 逆引きKotlin  
http://kotlin-rev-solution.herokuapp.com/site/todo/

# DDL について
Data Definition Language の略。データ構造を定義するときに使用する言語。SQLにおけるCREATE文がこれに当たる。  
他にも、権限周りを定義するデータ制御言語(DCL)、データを操作言語(DML)というものが存在する。  
データ定義言語 (DDL)とは｜「分かりそう」で「分からない」でも「分かった」気になれるIT用語辞典  
https://wa3.i-3-i.info/word15639.html

# DataIntegrityViolationException について
- データベースの更新や挿入を行う際に、一意性制約に違反した場合に投げられる例外。ちなみにRuntimeExceptionのサブクラスなので、非検査例外である。Exception のサブクラスは検査例外、RuntimeExceptionのサブクラスは非検査例外である。非検査例外はtry catchによる例外処理は行わなくとも良い。
- DataIntegrityViolationException (Spring Framework 5.2.3.RELEASE API)
  - https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/dao/DataIntegrityViolationException.html


# 楽観ロックと悲観ロックの違い
- 楽観ロックはデータ取得時のデータのバージョンと、データ更新時のデータのバージョンを突合し、バージョンが一致していた場合に更新処理を行う方法のことをさす。バージョンの確認に使用されるデータはデータベースに定義しておく必要があり、そのデータのことをロックキーと呼ぶ。それに対して悲観ロックとは、データ取得時にそのデータに対するロックを取得して、他のトランザクションから更新されないようにする方法のことをさす。Mybatisには楽観ロックを行う機能は提供されていない。
- 参考
  - https://qiita.com/NagaokaKenichi/items/73040df85b7bd4e9ecfc
  - https://terasolunaorg.github.io/guideline/public_review/ArchitectureInDetail/DataAccessCommon.html
  
# @TransactionalはInterfaceにつけるのではなく、実装クラスに付与するのが推奨されている

- 4.1. ドメイン層の実装 — TERASOLUNA Global Framework Development Guideline 1.0.6.RELEASE documentation
  - https://terasolunaorg.github.io/guideline/1.0.6.RELEASE/ja/ImplementationAtEachLayer/DomainLayer.html#transactional
- 4.1. ドメイン層の実装 — TERASOLUNA Global Framework Development Guideline 1.0.6.RELEASE documentation
  - https://terasolunaorg.github.io/guideline/1.0.6.RELEASE/ja/ImplementationAtEachLayer/DomainLayer.html#transaction-management-declare-transaction-info-label
- 12. Transaction Management
  - https://docs.spring.io/spring/docs/3.2.18.RELEASE/spring-framework-reference/html/transaction.html#transaction-declarative-annotations
- Springのproxyとfinalメソッド、それからnull — 裏紙
  - https://backpaper0.github.io/2018/02/22/spring_proxy.html

# @ConditionalOnPropertyアノテーションについて
jar 実行時に実行されるRunnerを切り替えるために使用する。  例えば、以下のように設定されたクラスを実行したい場合は、`java -jar ~.jar --type hoge`とすれば実行される。
```java
@ConditionalOnProperty(value = ["type"], havingValue = "hoge")
class HogeRunner implements ApplicationRunner {
}
```
- Spring Bootの実行可能jarに複数のCommandLineRunner/ApplicationRunner実装クラスを入れて、起動時のプロパティで実行するクラスを切り替える - Qiita
  - https://qiita.com/ksby/items/ef21b717e75b7c5b9988
  
# margin の相殺
css で margin を設定した場合、複数の要素のマージンが被る場合、マージンの大きい方の設定が反映される。

# html のディスプレイプロパティについて

|  プロパティ  |  配置ルール  |  サイズの操作  |
|:--:|:--:|:--:|
|  block  |  下に追加される  |  可能  |
|  inline  |  左づめで配置される  |  不可能  |
|  inline-block  |  左づめで配置される  |  可能  |

# HTTPのヘッダおよびボディの情報を確認する
```
sudo tcpdump -A port <port番号> -vv -i <インターフェース名(lo0等)>
```

# ヘッダ名について
- ヘッダ名をキャメルケースで指定すると、最初の文字だけ大文字で、それ以外は小文字になる
  - 例) UserHash -> Userhash

# DNS について
- 一般的なドメインのCNAMEにロードバランサのDNSを設定し、そのロードバランサのAレコードにそれぞれのVIPのIPを紐付けている。

# github でファイル名検索をしたい場合
`filename:{file名}` で検索

# systemd の reload と restart の違い
- reload -> 設定ファイル再読み込み restart -> Unit を停止して開始させる。
  - http://equj65.net/tech/systemd-manage/

# ページキャッシュとは
- Linux の機能の一つで、一度読み込んだデータをキャッシュとしてメモリに保持しておくことができる。どこでキャッシュを保持するかで名称が変わっており、メモリに保持する場合はページキャッシュ、カーネル内のメモリに保持する場合はSlabキャッシュと呼ぶ。

## 解放方法
```
$sudo sh -c "echo 1 > /proc/sys/vm/drop_caches"
```
- echo <number> の number が1の時はページキャッシュ、2の時はSlabキャッシュ、3の時はどちらのキャッシュも解放する。

## 参考文献
- [Linuxでページキャッシュを確認・解放してみた - Qiita](https://qiita.com/toshihirock/items/e2d187e91ee5446c7a69)

# anacron について
- cron の親戚みたいなもの。cronとの違いは、遅延実行される点と最低でも1日単位での実行しか定義できない点である。`/etc/anacrontab` に設定が記述されている。
- 例えば、以下のような設定があったとする。
```
1 5 cron.daily run-parts /etc/cron.daily
```
- これは「1日おきにコンピュータ起動時には5分待ってから`run-parts /etc/cron.daily`を実行する。その時の実行日時は /var/spool/anacron/cron.daily」に記録する」という意味である。
- 参考文献
  - http://piyopiyocs.blog115.fc2.com/blog-entry-674.html

# logrotate
- 全体の設定は `/etc/logrotate.conf` にある
- /etc/logrotate.d/syslog は syslog のローテート設定を格納している
```
/var/log/cron
/var/log/maillog
/var/log/messages
/var/log/boot.log
/var/log/spooler
{
    sharedscripts
    postrotate
        /bin/kill -HUP `cat /var/run/syslogd.pid 2> /dev/null` 2> /dev/null || true
    endscript
}

/var/log/secure
{
    create 0600 root
}
```
- cron実行時に設定ファイルを読み込むので、設定変更後の再起動は必要ない
- 参考文献
  - https://www.atmarkit.co.jp/ait/articles/0209/07/news002_3.html
  - http://ossfan.net/ssetup/logrotate-01.html
  - https://milestone-of-se.nesuke.com/sv-basic/linux-basic/logrotate/

# logwatch によるログ監視
- logwatch は各サーバのサービスログを集計してレポートにしてくれるソフト。
  - https://www.server-memo.net/tips/server-operation/logwatch.html

# 証明書配置場所
- `/etc/pki/CA/`

# 他の cookbook のレシピを使用したい場合
- トップレベルディレクトリに Berksfile というファイルを作成し、そこに使用したい cookbook を記述する。Berksfile とは cookbook の依存管理を行うファイルである。
- 公式ドキュメント
  - https://docs.chef.io/workstation/berkshelf/

# タイムアウトの種類

|  種別  |  説明  |
|:---|:---|
|  セッションタイムアウト  |  セッションが有効である時間  |
|  読み込みタイムアウト  |  リクエスト先のサーバからのレスポンスを読み込む際の制限時間  |
|  書き込みタイムアウト  |  リクエスト先のサーバへデータを送信する際の制限時間  |

- 資料
  - https://www.ipa.go.jp/security/awareness/vendor/programmingv1/a05_03.html

# nginx
- mail モジュール
  - http://nginx.org/en/docs/mail/ngx_mail_proxy_module.html

# Common Name とは
- サーバ証明書のサブドメインのことを表す。クライアントから指定されたURLとこの Common Name を比較して、異なっている場合は警告が出される。
  - 例) https://www.example.com の Common Name は www.example.com となる。
- 資料
  - [Common Name（コモンネーム）とは何ですか？ | SSLストア](https://www.ssl-store.jp/common-name%EF%BC%88%E3%82%B3%E3%83%A2%E3%83%B3%E3%83%8D%E3%83%BC%E3%83%A0%EF%BC%89%E3%81%A8%E3%81%AF%E4%BD%95%E3%81%A7%E3%81%99%E3%81%8B%EF%BC%9F/#:~:text=Common%20Name(%E3%82%B3%E3%83%A2%E3%83%B3%E3%83%8D%E3%83%BC%E3%83%A0)%E3%81%A8,%E3%81%A6%E3%81%84%E3%82%8B%E5%BF%85%E8%A6%81%E3%81%8C%E3%81%82%E3%82%8A%E3%81%BE%E3%81%99%E3%80%82)

# 複数の踏み台サーバを経由して目的のサーバにログインする

- `~/.ssh/config` に以下の設定を追加。以下の例は`踏み台サーバ1` -> `踏み台サーバ2` -> `目的のサーバ`という経路でログインする場合の設定である。
```
Host <踏み台サーバ2>
    ProxyCommand ssh <踏み台サーバ1> nc %h %p
    IdentityFile ~/.ssh/id_rsa
Host <目的のサーバ>
    ProxyCommand ssh -W %h:%p <踏み台サーバ2>
    IdentityFile ~/.ssh/id_rsa
```
- `ProxyCommand` で `nc %h %p` を使用している箇所とそうでない箇所が存在するが、これは Proxy するサーバに `/bin/bash` が備わっているかどうか(nc がインストールされているかどうか)で変更する必要がある。備わっていなければ`ProxyCommand`は `ssh` コマンドで完結させる必要ある(目的のサーバの設定参照)。
- また、プレースホルダの `%h`, `%p`に入る値は、それぞれ config ファイルの `HostName`, `Port`に設定されているものである。
- 参考資料
  - [ProxyCommandによるsshの多段接続について -- ぺけみさお](https://www.xmisao.com/2013/10/08/ssh-proxy-command.html)
  - [[ssh] 複数の踏み台を経由したssh接続 - Qiita](https://qiita.com/sengoku/items/ef03b33ca0aece1c3cf3)

# 証明書と秘密鍵のペアが同じかどうかを確認する

```
$ openssl x509 -noout -modulus -in hoge.crt | openssl md5
(stdin)= a24fad02a2748573625a737a25ae842e
$ openssl rsa -noout -modulus -in hoge.key | openssl md5
(stdin)= a24fad02a2748573625a737a25ae842e
```

# chef-client -W について
- `sudo chef-client -W`は必ずしも`sudo chef-client`と同じ結果とはならない`sudo chef-client -W`が失敗したとしても `sudo chef-client`が成功することがあるので、検証時にはどちらも試して確認すること。

# yum パッケージのインストールがうまくいかない場合
- 以下の3点を試す

1. cookbook_file に `not_if` を追加する
```
cookbook_file '/etc/yum.repos.d/hoge.repo' do
  not_if { File.exist? '/etc/yum.repos.d/hoge.repo' }
  source 'hoge.repo'
  owner 'root'
  group 'root'
  mode '0644'
end
```

2. cookbook_file (上のスクリプト)の後に yumレポジトリのローカルキャッシュの更新を追加
```
execute 'yum makecache fast' do
  command 'yum makecache fast'
end
```

3. `sudo chef-client`実行

# yum レポジトリの配置場所

```
/etc/yum.repos.d
```

# logrotate に関して
- 設定ファイルは `/etc/logrotate.conf`であり、このファイルは `/etc/logroate.d/` 配下の設定ファイルの内容をinclude している。設定変更後は再起動は行わなくてよい。
- 基本 logrotate は daily でうごているので、一時間ごとのローテートを行う際には `/etc/cron.hourly/` に `/etc/cron.daily/logrotate` をコピーする必要がある。
- 参考資料
  - 動作について
    - https://milestone-of-se.nesuke.com/sv-basic/linux-basic/logrotate/
  - 設定項目について
    - https://oxynotes.com/?p=6493
    - https://hackers-high.com/linux/man-jp-logrotate/
  - 1時間ごとの設定を行う
    - https://qiita.com/toshihirock/items/a5e33c5a89e8e9592177

# rpm と yum の違い
- rpm によるインストールは依存関係を考慮しないで行われる。したがって単体のパッケージしかインストールできない。これに対し yum は rpm を管理するツールなので、依存関係も含めてインストールが行われる。
  - https://eng-entrance.com/linux-package-rpm-yum-def

# yum のアンインストール

```
$ yum remove <package>
```

# spec ファイルについて

- 資料
  - https://vinelinux.org/docs/vine6/making-rpm/vine-making-rpm.html
  - https://qiita.com/mypaceshun/items/5067561d6739cc9e5199

# git revert について
- 間違えて master に push してしまった時には、`git revert`を使用して push を取り消す操作を行う(厳密には新しいコミットになる)。
```
# 直前のコミットのみを取り消す
$ git revert HEAD

# 直前のコミットから数コミットを取り消す
$ git revert HEAD~n

# push
$ git push origin master
```
- 資料
  - http://www-creators.com/archives/2020

# 特定のファイルを任意の commit hash まで戻したい時

- https://qiita.com/ritukiii/items/5bc8f74dbf4dc5d1384c
```
$ git checkout <hash> <ファイルのパス>
```

# サービスアウト後もアクセスが来る要因
- 以下の2点
  1. クライアントがDNSキャッシュを持っている場合
  2. IP直指定でアクセスしている場合

# 「VIP を GSLB から外す」 と 「VIP down」 の違い
- 「VIP を GSLB から外す」を行うと、完全に外部からのアクセスをシャットダウンすることになる。
  - キャッシュを持っているクライアントはキャッシュが削除されるまで、サービスを使用することができない。
- 「VIP down」の場合は、新規のユーザがそのVIPにアクセスすることはないが、キャッシュを持っているクライアントのアクセスは来る。

# VIP に追加されているかどうかを確認する
- `ifconfig` コマンドでVIPに追加されているかどうかを確認することができる
```
$ ifconfig
...

lo:1: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet <VIPのIPアドレス>  netmask 255.255.255.255 // ここを確認
        loop  txqueuelen 1000  (Local Loopback)

```

# find -mtime について
- `-mtime` オプションの付け方は n 日以上更新されていないファイルを検索したい場合は、`-mtime n`とする必要がある。`-mmin`オプションと違って`+`はいらない。
```
sudo find ./ -maxdepth 1 -name 'access.log.*' -not -name '*.gz' -mtime 1 -exec gzip {} \; > /dev/null 2>&1
```

# > /dev/null 2>&1
- 標準出力は /dev/null に出力し、標準エラー出力は標準出力とマージするという意味。つまり、全ての出力が /dev/null にいき、実行結果は握り潰される。

# tmux 操作
```
# プレフィックスキー(以後 pk)
ctl + b

# tmux 開始
$ tmux

# 縦に画面分割
${pk} + %

# 横に画面分割
${pk} + "

# 次のペインに移動
${pk} + o

# syncronized pane 開始
${pk} + :set-(ここら辺でTAB) sync(ここら辺でTAB) on

# syncronized pane 終了
${pk} + :set-(ここら辺でTAB) sync(ここら辺でTAB) off

# tmux 終了
$ tmux kill-server
```

# VLAN について
- 一つのネットワークを論理的に内部で分割したネットワークのことを指す。
  - https://wa3.i-3-i.info/word12084.html

# ファイルの所有者、グループを変更

```
$ ls -lh /var/log/td-agent
total 8.0M
drwxr-xr-x 2      956      956    6 Jun  8 16:35 buffer
drwxr-xr-x 2      956      956   34 Jun  8 16:35 pos
-rw-r----- 1 td-agent td-agent 6.2M Jun 23 16:55 td-agent.log
-rw-r--r-- 1      956      956  55K Jun 10 13:01 td-agent.log-20200617

# 所有者、グループを td-agent に変更する
$ sudo chown -hR td-agent:td-agent /var/log/td-agent
```

# [Chef] environments ファイルのアップロードを行う場合
- `/environments` 直下のファイルを chef server の environments に反映したい場合は、以下のように実行する。
```
$ knife upload /environments
```
- knife upload
  - https://docs.chef.io/workstation/knife_upload/

# [Chef] bootstrap でエラー

- bootstrap コマンドを実行したら以下のエラーが発生
```
ERROR: NotImplementedError: OpenSSH keys only supported if ED25519 is available
net-ssh requires the following gems for ed25519 support:
 * rbnacl (>= 3.2, < 5.0)
 * rbnacl-libsodium, if your system doesn't have libsodium installed.
 * bcrypt_pbkdf (>= 1.0, < 2.0)
See https://github.com/net-ssh/net-ssh/issues/478 for more information
Gem::MissingSpecError : "Could not find 'rbnacl' (< 5.0, >= 3.2.0) among 283 total gem(s)
```

- issues のコメントに記載されてあるコマンドを実行したら正常に動作するようになった。
```
$ ssh-add -K ~/.ssh/id_rsa
```
- `ssh-add -K`は、keychain に 秘密鍵を登録するコマンドである。`-K`なしだと、ターミナルを立ち上げる度に秘密鍵の登録がリセットされてしまう。
- 今回はPCを立ち上げた後他のサーバにログインせずに(すなわち秘密鍵の登録を行わなかった)bootstrap コマンドを実行したためにエラーが発生したと思われる。

# /etc/sudoers
- サーバでパスワードなしでコマンドを実行するためには、以下のような記述を参考にする。
```
# wheelグループのユーザはどこでも、誰としてでもパスワード無しで何でもできる
%wheel ALL=(ALL:ALL) NOPASSWD: ALL
```
- 参考資料
  - https://qiita.com/progrhyme/items/6f936033b9d23efb1741

# telnet の自動化

- パイプで渡してやることで telnet のような対話型のコマンドを自動で実行できるようになる。telnet 以外でも他の対話型のコマンドならなんでも利用可能。

```
(sleep 1;echo quit) | telnet <host> 110
```
- 参考資料
  - https://qiita.com/gyoon/items/deb7ee62fbe4e9c1a907

# FIDO(ファイド)認証
- Fast IDentity Online の略。パスワードによる認証ではなく、指紋などの生体情報を用いて行う認証のこと。デバイスごとに秘密鍵と公開鍵を生成し、サーバ側に公開鍵を登録して暗号化を行い認証を行う
- 資料
  - https://it.impress.co.jp/articles/-/15739

# iptables について
- 外部からのリクエストを受け付けるポートを設定できるパケットフィルタのことを指す。
  - https://knowledge.sakura.ad.jp/4048/


# 暗号化で出てくる用語

- SHA256
  - https://ja.wikipedia.org/wiki/SHA-2
- MAC(Message Authentication Code)
  - https://ja.wikipedia.org/wiki/%E3%83%A1%E3%83%83%E3%82%BB%E3%83%BC%E3%82%B8%E8%AA%8D%E8%A8%BC%E7%AC%A6%E5%8F%B7
- HKDF
  - http://blog.bruwbird.com/hkdf-2/
- AES
  - https://ja.wikipedia.org/wiki/Advanced_Encryption_Standard
- GCM
  - https://ja.wikipedia.org/wiki/Galois/Counter_Mode

# RS256 と HS256について
- RS256は公開鍵と秘密鍵を使用して暗号化を行う方式。 HS256は共通鍵を使用して暗号化を行う方式
  - https://qiita.com/satour/items/e68afe3de6267cebcfea

# graceful shutdown
- サーバが以下のような処理を行い、安全にクライアントとの接続を切りプロセスを停止する方法。
  - 新規のリクエストを受け付けない
  - 処理中のリクエストを全て捌き切ってからプロセスを停止する。
- 資料
  - https://qiita.com/megmogmog1965/items/86da1dcb42cb5c6a4d14

# Java の static イニシャライザ
- クラスのロードが行われる際に一度だけ呼ばれるメソッドのことをさす
- 参考文献
  - https://qiita.com/b1ueskydragon/items/2a6e0812a9cee3fc255f
```java
public class Hoge {

    private static final String foo;

    // static イニシャライザ
    static {
        this.foo = "aaa";
    }
}
```

# JMockit の @Mocked アノテーション
- モック化したいフィールドに対して付与する。
- @Mocked(stubOutClassInitialization=true) とすることでイニシャライザで行なっている処理をスキップすることができる。
- 資料
  - https://qiita.com/opengl-8080/items/a49d4dae9067413ccdd6

# go test でカレントディレクトリ配下の全てのテストを実行する

```
$ go test ./...
```

# go linux 環境で実行できるバイナリを生成する

```
$ GOOS=linux GOARCH=amd64 go build hello.go
```

# VSCode で生成したPlantUMLの画像のサイズを大きくする

- コマンドパレットで Preferences Open Settings (JSON) を選択して Settings.json を開き、以下を追加。
```
{
    ...
    "plantuml.commandArgs": ["-DPLANTUML_LIMIT_SIZE=8192"]
}
```

# kubernetes の Service について
- 一つのクラスタに対応するResource。クライアントからリクエストが行われた時、クライアント -> Service -> Pod という経路をたどる

# kubernetes の Ingress について
- 複数のマイクロサービスをまとめ、外部との単一のアクセス点となりうる簡単なフロントエンド

# Kubernetes 資料
- https://kubernetes.io/ja/docs/concezpts/services-networking/ingress/
- https://kubernetes.io/ja/docs/concepts/services-networking/connect-applications-service/
- https://kubernetes.io/ja/docs/concepts/services-networking/dns-pod-service/
- https://kubernetes.io/ja/docs/concepts/services-networking/service/
- https://kubernetes.io/ja/docs/concepts/overview/working-with-objects/kubernetes-objects/
- https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md#metadata
- https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/
- https://kubernetes.io/ja/docs/concepts/overview/working-with-objects/labels/


# Kubernetes の Service について
- DNSみたいなもの。要求に対して Pod の集合を返すリソースのことを指す。
  - https://kubernetes.io/ja/docs/concepts/services-networking/service/

# k8s でJDKをインストールする場合
- Dockerfile に JDK をインストールするスクリプトを記述する必要がある。

# Dockerfile の CMD と ENTRYPOINT について
- CMD はプロセスを実行する時に使用するインストラクション。ENTRYPOINTはCMDによるプロセス実行を工夫するためのもの。例えば、ENTRYPOINT に `/usr/bin/java` が設定されている場合は、CMD は java 起動時のオプションだけ記述すればよくなる。
- https://docs.docker.jp/engine/reference/builder.html#entrypoint

# Ubuntu と CentOS の違い
- ベースとなっているOSが異なる。Ubuntu は　Debian がベースになっているのに対して、 CentOS は RHEL がベースとなっている。それに伴い、Ubuntu はパッケージマネージャーに dep, apt を使用し、CentOS は rpm, yum を使用する。

# Spring Boot アプリケーションを起動した時に発生した Exception
```
java.lang.NoSuchMethodError: javax.servlet.ServletContext.getVirtualServerName()Ljava/lang/String;
```
- インストールしていた javax.servlet-api のバージョンが古かったことが原因。バージョンを 2.5 から 3.1 にあげたら直った。

# Prometheus の PromQL について
- `=~` は正規表現でマッチしたものを抽出する

# kubectl コマンド

```
# ConfigMap 一覧取得(NameSpace まで指定する必要あり。Podを指定する場合は pods とする)
$ kubectl get configMaps

# Resource の詳細を確認
$ kubectl describe configMaps hoge

# Pod にログインしてシェルを実行する
$ kubectl exec -it <pod name> /bin/bash

# deployment (ここでは、myapp) を削除
RESOURCE_NAME="myapp"
kubectl delete deploy ${RESOURCE_NAME}
```

# 下請法について学んだこと
- 下請代金は、納品日から起算して60日以内に全額支払わなければならない。また、正当な理由のない下請代金の減額、給付内容の変更、完成品の受取拒否、不当な返品の強要など、下請事業者の利益を害する行為を行ってはならない。
- 下請法に違反した場合の影響
  - 公正取引委員会に違法行為を取りやめるよう勧告され、企業名や違反事実などが公表される。
  - ネガティブ報道によりブランドイメージが損なわれる
  - 担当社員に最高50万円の罰金が課せられる。会社にも別途請求される
- 下請法が適用されるケース
  - 情報成果物(プログラム)を委託する場合は、取引先の資本金が3億円以下の場合に適用される
    - アプリやソフトウェアの企画、設計、開発の全部または一部を委託する場合
  - 情報成果物(プログラム)を委託する場合は、取引先の資本金が5千万円以下の場合に適用される
    - デザインや映像作品など
- 義務行為
  - 発注書を直ちに交付すること
  - 取引履歴を作成、保存すること(2年間)
  - 納品から60日以内に下請代金を支払うこと
  - 支払期日に遅れた場合は、年率14.6%の遅延損害金を支払うこと
- 禁止行為
  - 受領拒否
  - 支払遅延
  - 下請代金の減額
  - 返品
  - 買いたたき
  - 不当な給付内容の変更、やり直し

# pem と rsa について
- pem とは暗号鍵などのフォーマットを表した拡張子。rsa は一種の暗号化方式
  - https://qiita.com/kunichiko/items/12cbccaadcbf41c72735

# blackbox-exporter の設定について

```yaml
- job_name: blackbox-http # To get metrics about the exporter’s targets
  metrics_path: /probe
  params:
    module: [http_2xx]
  static_configs:
    - targets:
      - http://prometheus.io    # Target to probe with http
      - https://prometheus.io   # Target to probe with https
      - http://example.com:8080 # Target to probe with http on port 8080
  relabel_configs:
    - source_labels: [__address__] # (1)
      target_label: __param_target 
    - source_labels: [__param_target] # (2)
      target_label: instance
    - target_label: __address__ # (3)
      replacement: localhost:9115
```
- `source_labels` フィールドを定義することによりValueの値を取得し、`target_label` で取得した値を特定のフィールドにセットする。
- (1): `__address__` フィールド(static_configs.targets の値が格納されている)の値を`__param_target`にセットすることにより、blackbox-exporter へのリクエストに target パラメータを付与する。
- (2): `__param_target`の値を `instance` フィールドに格納することにより、メトリクスをPromQLで取得する際に、'instance=<probe先のhost>'でひっかけることができる
- (3): `__address__`フィールドを`localhost:9115`に変更することで blackbox-exporter へのリクエストホストを `localhost:9115` に変更する。

# JNDI について
- Java Naming and Directory Interface の略で、オブジェクトに名前をつけてオブジェクトの検索ができるような機能を提供するAPI。DNS みたいなもの。
  - https://developer.ibm.com/jp/articles/was-jndi-1/

# RabbitMQ について
- 新人プログラマに知ってもらいたいRabbitMQ初心者の入門の入門 - Qiita
  - https://qiita.com/gambaray/items/3cc02b419c860a96bc94
- チュートリアル
  - https://spring.pleiades.io/guides/gs/messaging-rabbitmq/

# git revert について
- git revert は reset と異なり、変更の打ち消しを新しいcommitとしてcommitすることを指す。
```
// 特定のcommitを打ち消す
$ git revert <commit id>

// 打ち消すcommitを範囲指定する
$ git revert <commit id>..HEAD

// merge commit が行われた場合に、merge された branch の状態に戻す。m オプションが1だとマージされる branch の状態に戻す
$ git revert <commit id> -m 1
```

# kubectl サービスアカウントコマンドについて

```
// サービスアカウント確認
$ SERVICEACCOUNT=sandbox-deploy-test-$USER
$ kubectl describe serviceaccount ${SERVICEACCOUNT}

// secret 詳細表示
$ kubectl describe secret sandbox-deploy-test-hoge-token-47h2j

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     2241 bytes
namespace:  7 bytes
token:      <TOKEN>

```

# Spring Retry について
- Bean 定義したクラスしかRetryが実行されない。

# Intellij の Projects にフォーカスする
- ⌘1 を押すと、プロジェクトウィンドウが開いてそこにフォーカスが移る。

# jq コマンドでJSONを１行に変換したい
```sh
jq -c . input(file)
```

# リクエストパラメータを複数指定したい場合
- URLをシングルクォーテーションで囲む
```sh
curl -v -X GET 'http://localhost:8080/v2/proto/country?ip=127.0.0.1&requestid=hoge'
```

# メッセージ付き git stash 

```sh
# 名前付きstash
$ git stash save "hoge"

# リストアップ
$ git stash list
```

# kubernetes の Ingress で使用する Secret の設定
- kustomize を使用して Ingress で使用する Secret を生成する場合には、以下のような kustomization.yaml を記述する。
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
secretGenerator:
  - name: tls-secret
    type: "kubernetes.io/tls"
    files:
      - tls.key
      - tls.crt
generatorOptions:
  disableNameSuffixHash: true
```
- このファイルの secretGenerator.type フィールドに"kubernetes.io/tls"が指定されていると、このSescretは tls.crt, tls.key を内包していなければならない。
- 参考資料
  - https://kubernetes.io/ja/docs/concepts/services-networking/ingress/
  - TLSのSecretはtls.crtとtls.keyというキーを含む必要があり、TLSを使用するための証明書と秘密鍵を含む値となります。

# 要求と要件の違い
- 要求はビジネスで何が必要かを表したもの。要件は要求を満たすためにシステムが何をしなければならないのかを表したもの。
  - https://qiita.com/sunstripe2011/items/61df719fb1f6178b2605

# JDWP (Java Debug Wire Protocol)
- リモートデバッグを行うためのプロトコル。以下のように Java　のプロセス起動時にオプションを指定することでリモートデバッグを可能とする。
```sh
$ /usr/bin/java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=8082 -jar sample.jar 
```
- [JPDA の接続および呼び出し](https://docs.oracle.com/javase/jp/6/technotes/guides/jpda/conninv.html)

# git tag によるロールバック方法
```
// 更新自体をなかったことにする
$ git reset --hard <タグ名>

// 更新の打ち消しのコミットを行う
$ git revert <タグ名>..HEAD -m 1
```

# 一致した次の行の置換を行うコマンド
- `{n;~}`で置換を行うことができる
```
// -i コマンドでファイルをそのまま書き換える
$ sed -i -e "/example.com:4443\/hoge\/fuga/{n;s/newTag: \([0-9]\{8\}-[0-9]\{6\}\)/newTag: hoge/;}" kustomization.yaml

// 最終形
$ sed -e "/example.com:4443\/hoge\/fuga/{n;s/newTag.*/newTag: $TAG/;}" kustomization.yaml
```

# Kubernetes にデプロイしたアプリで default backend 404 が返ってくる場合
- `default backend 404` は nginx が返すエラーで、パスのマッチングがうまくいっていない場合に返される。
- tls-secret が正しいものを参照していなかったために発生したエラーだった模様

# ImagePullBackOff について
- `ImagePullBackOff` は docker-registry から image を取得できない時に発生する状態なので、URLが正しく設定されているかどうかを確認する
```
$ kubectl describe pods <pod>
NAME                                            READY   STATUS             RESTARTS   AGE
...
  
  Warning  Failed     3m30s                       kubelet            Failed to pull image "docker-registry.com:4443/hoge:latest": rpc error: code = Unknown desc = Error response from daemon: manifest for docker-registry.com:4443/hoge:latest not found: manifest unknown: manifest unknown
```

# Pod の ログを確認する
```
$ kubectl logs <pod> -c <container>
```

# kustomize でエラー発生
- 以下のようなエラーが発生した場合は、複数の子の kustomization.yaml で同じ親の kustomization.yaml を見にいっている可能性があるので、重複する箇所を削除する。
```
$ kubectl kustomize ovarlays/dev/stable > output/dev_stable.yaml
...
Error: ../../../base/: id '"~G_v1_Service|~X|~P|auth-service|~S"' already used
```

# リモートのタグの取得について
- リモートのタグの取得については、`git pull`ではなく、`git fetch`でないとできない。

# Pods が起動しない原因を探る
- `kubectl get pod <Pod Name> -o yaml` で表示された `lastState` フィールドを確認する。
  - https://kubernetes.io/ja/docs/tasks/debug-application-cluster/determine-reason-pod-failure/
- lastState フィールドに値が入っているコンテナが起動しておらず Pods の起動に失敗しているので、`kubectl logs <Pod Name> -c <Container Name>`で原因を探る

# Ingress から Empty reply が返ってくる事象について
- 動作確認を行なっていたところ、以下のようなエラーが発生した。
```sh
* TLSv1.2 (IN), TLS alert, close notify (256):
* Empty reply from server
* Connection #0 to host <host> left intact
curl: (52) Empty reply from server
* Closing connection 0
```
- Ingressのログを確認したところ、400系が返ってきていた。Ingress のログについては、Splunkでアクセスログを確認することができる。
- これは Ingress のルーティングがおかしいと発生する事象なので、Ingressの設定周りで何か不備がないかを確認する。今回は　Ingress の metadata フィールドにおかしな値が設定されていたので、それを修正したら直った。

# Pod が起動しない問題
- kubectl apply でデプロイを実行したら、以下のようなエラーが発生した。これは Node の CPU リソースが不足しているためにPodを起動することができなかったことが原因である。
- https://kubernetes.io/ja/docs/tasks/configure-pod-container/assign-cpu-resource/

```
$ kubectl describe pod <POD>

Events:
  Type     Reason                        Age                    From               Message
  ----     ------                        ----                   ----               -------
  Warning  FailedScheduling              4m12s                  default-scheduler  0/19 nodes are available: 15 Insufficient cpu, 4 node(s) had taints that the pod didn't tolerate.
  Warning  FailedScheduling              4m12s                  default-scheduler  0/19 nodes are available: 15 Insufficient cpu, 4 node(s) had taints that the pod didn't tolerate.
  Warning  MultiplePodDisruptionBudgets  4m12s (x2 over 4m12s)  controllermanager  Pod "auth"/"<POD>" matches multiple PodDisruptionBudgets.  Chose "<Pod Distribution Budget>" arbitrarily.
```

# Spring Boot のバージョンアップについて
- Spring Boot のバージョンアップを行ったら、./gradlew bootRun 実行で 2.4.0 で起動されていることを確認できたが、IntelliJ のタスクランナーで起動したら 2.3.6.RELEASE のままだった。一度プロジェクトを閉じて再起動したら 2.4.0 になった。したがって、バージョンアップを行なった場合は一度プロジェクトを閉じる必要がある。もしかしたら dependencies のタスクを実行しても解決するかもしれない。

# Java プロジェクトの依存性の直しかた

- ./gradlew build を実行したところ、以下のようなエラーが発生。
```
$ ./gradlew build
> Task :compileJava FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':compileJava'.
> Could not resolve all files for configuration ':compileClasspath'.
   > Could not find ${A}.
     Required by:
         project : >${Bを使用しているライブラリC} > ${Aを使用しているライブラリB} > > ${A}
```
- `Could not find`とあるので、ライブラリが見つからなかったことを考えられる。この場合は build.gradle の repository 句に不備がないかを確認する。

# IntelliJ ショートカット
- Project Pain にフォーカス(cmd + 1)
- Project Pain -> エディタにフォーカス (ESC)

# Mockito による final メソッドのモックについて
- mockito は final メソッドについてはデフォルトではモック化できない。

# Spring の RestTemplate が投げる例外
- 指定したContent-Typeでレスポンスが返ってこなかった
  - org.springframework.web.client.UnknownContentTypeException: Could not extract response: no suitable HttpMessageConverter found for response type [class hoge] and content type [text/plain;charset=utf-8]
- ステータスコード400系が返ってきた(400, 500で検証)
  - org.springframework.web.client.HttpClientErrorException$BadRequest: 400 Bad Request: [{"token": "v=Z1;d=domain.p", "expiryTime": 1528860825}]
- ステータスコード500系が返ってきた(500で検証)
  - org.springframework.web.client.HttpServerErrorException$InternalServerError: 500 Internal Server Error: [{"token": "v=Z1;d=domain.p", "expiryTime": 1528860825}]
- ReadTimeOutが起こった場合
  - org.springframework.web.client.ResourceAccessException: I/O error on POST request for "http://localhost:10080/roletoken": Read timed out; nested exception is java.net.SocketTimeoutException: Read timed out
- リクエスト先のポートが閉じられている場合
  - org.springframework.web.client.ResourceAccessException: I/O error on POST request for "http://localhost:10081/roletoken": Connect to localhost:10081 [localhost/127.0.0.1] failed: Connection refused (Connection refused); nested exception is org.apache.http.conn.HttpHostConnectException: Connect to localhost:10081 [localhost/127.0.0.1] failed: Connection refused (Connection refused)

# TCP_NODELAY について
- TCP_NODELAYはNagleアルゴリズムを無効にする設定のことを指す。Nagleアルゴリズムとは、パケット通信においてリアルタイム性を犠牲にして通信効率を上げるために使用されるアルゴリズムのことを指す。1バイトなどの小さいパケットを送信し続ける場合は、IPV4で20バイト、TCPヘッダで20バイト付与されるので、1バイトを送信するのにも40バイトも追加しなければならなくなり、オーバヘッドが大きい。それを回避するため、1バイトを送信する際はバッファリングを行い、バッファが一定数に達したら通信を行うという戦略を取る。これをNagleアルゴリズムとよぶ。しかし、このアルゴリズムを使用すると、リアルタイム性を失うことになるので大きなパケットを送信する場合は Nagile アルゴリズムは使用しない方が効率的である。

# HTTP1.1のボディ送信の確認について
- クライアントからサーバに一度でデータを送信せずに、一旦受入可能かどうかを確認してからボディ本体を送信することがHTTP1.1から可能になった。クライアント側は Expect: 100-continue というヘッダを追加し送信を行う。サーバ側から 100-continue が返ってきた場合はボディつきで送信を行い、サーバがその機能に対応指定なければ 417 Expectation Failed が返ってくる。

# RestTemplate でレスポンスをMapで受け取りたい場合
- ParametarizedReferenceを使用してレスポンスをMapとして受け取ることが可能
```
final ResponseEntity<Map<String, String>> response =
    restOperations.exchange(requestEntity, new ParameterizedTypeReference<>() {});
```
# WebSocketについて
- WebSocketとはクライアントとサーバ間で双方向通信を行う際に使用できるプロトコルである。
- WebSocketはサーバ上にデータをオンメモリで管理するケースが多い。これは、オンラインゲームなどリアルタイム性が高いコンテンツで使用されることが多いためである。オンメモリで管理するため、接続が切断された場合は同じサーバに対して再び接続しなければならないためHTTPベースのロードバランサは使用できない。直接クライアントが再接続先のサーバを指定してリクエストするか、TCPベースのロードバランサを使用するかの対策を講じる必要がある。
- WebSocketを用いた通信はHTTPの機能であるプロトコルのアップグレードを使用することで行う。Upgradeヘッダにwebsocketを指定してリクエストを実行する。

# WebRTCについて
- WebRTCはクライアントとサーバ間だけでなく、クライアントとクライアント間のP2Pの通信を実現するプロトコルである。主にSkypeなどのオンラインでのビデオ会話ツールなどで使用されている。

# HTTPウェブプッシュ
- HTTPウェブプッシュはスマートフォンのアプリの通知機能のようなものをWebアプリケーションに提供する機能である。
- 処理の流れは以下。登場人物としてエンドユーザが使用するブラウザ、Push通知を送るプッシュサービス、ユーザが利用するWebアプリケーションが存在する。
  - ブラウザ起動時にプッシュ通知機能を利用するかをユーザから許可を得る。許可を得た際には Service Worker で登録したPushイベントで通知が受け取れるようになる。Serivice Worker はフロントエンドとWebサーバの中間に存在するプロキシのようなものである。
  - アプリケーションサーバがプッシュ通知をクライアントに送るためには、プッシュサービスのAPIを利用する。Webアプリケーションの通知を受信するかどうかをブラウザがクライアントに許可を取り、許可を得た場合にプッシュサービスに登録を行う。この際に通知を送信するブラウザを特定する鍵を作成する
  - その後ブラウザはプッシュ通知に必要な鍵をアプリケーションに送信する。アプリケーションはこの鍵情報を含めてプッシュサービスにリクエストを行うことで特定のブラウザに通知を送ることができる。

# オープングラフプロトコル
- TwitterやFacebookなどでリンクを貼ると画像や説明文が表示される機能。HTMLにメタタグを設定することによりこの機能を使用することができる。よく使用される基本要素は以下。
  - og:title : タイトル
  - og:type : 種類(ex: article)
  - og:url : サイトのURL
  - og:image : 画像

# AMP
- モバイル高速化のための仕組み。使用できるJavaScriptのライブラリが制限されていたり、静的なコンテンツしか表示できないなど、AMP対応のWebページを作成するためには様々な制約が存在する。
- AMPが高速なのは、サイトの作り方に加えて配信部分にも理由がある。検索エンジンのための情報を収集するクローラー(スパイダー)はAMPを見つけたらそれをキャッシュサーバ(CDN)にコピーしてキャッシュを行う。したがってAMP対応のWebページはHTMLをダウンロードされた瞬間にページを表示することができる。

# SSRF(サーバーサイド・リクエスト・フォージェリ)
- 外部からリクエストを受け付けるWebアプリケーションが処理の途中でさらに任意のサーバに対してリクエストを実行する場合に、攻撃者がそのリクエスト先を指定することができてしまう脆弱性のこと。内部のサーバを指定されることで内部のサーバの情報を盗まれてしまう。
- チャットアプリケーションをケースに説明する
  - まず、ユーザが何かしらのWebページの内容を他のユーザと共有するためにそのサイトのURLをメッセージで送信する。
  - その後チャットアプリを提供しているサーバはそのURLにリクエストを行い、サイトの情報を入手してそれをユーザに見せるような処理を実行する(オープングラフプロトコルみたいなもの?)
  - この場合、URLに内部のサーバのURLを指定されてしまうと、内部情報が漏れてしまい、情報の漏洩につながってしまう。
- 対策としては、接続できるURLを予めサーバで定義しておき、それ以外のURLが指定された際にはリクエストを拒否することと、iptablesを用いて接続先のアドレスをネットワークレベルで制限することがあげられる。

# Spring Boot で resources 配下のファイルを読み込む場合
- ルートからの相対パスで読み込む(例: src/main/resources)

# ThreadPoolExecutor(ExecutorService)の終了時の挙動
- shutdown() メソッドでシャットダウンを開始する。この時すでに送信されたタスクは依然として実行中だが、新規のタスクは受け付けなくなる。
- 実行中のタスクを終了させるために、awaitTermination() メソッドを呼び出す。実行中のタスクが全て正常終了した場合はtrue,そうでない場合はfalseが返ってくる
- 正常終了しなかったタスクが存在した場合は、shutdownNow()メソッドで実行中のタスクを停止させ、加えて待機中のタスクの処理も停止させる。
- shutdownNow()メソッドでタスクが停止するまである程度待つ必要があるため、再びawaitTermination() メソッドを呼び出す。

# Go でパッケージごとのカバレッジを取得する
```
// カバレッジレポート生成
$ go test -coverprofile=cover.out <パッケージディレクトリ>

// カバレッジレポート表示
$ go tool cover -html=cover.out
```

# (kubernetes) IntelliJで JVM が起動しているコンテナのリモートデバッグを行う方法
- Dockerfileにおいて、JVM 起動時にjdwpオプションを追加してリモートデバッグが行えるように設定する。以下の例だと、8082番ポートがデバッグポートになる。
```
CMD [ "/usr/bin/java", "-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=8082", "-jar", "hoge-0.0.1-SNAPSHOT.jar" ]
```
- `kubectl get pods` コマンドで、リモートデバッグを行いたいコンテナを含むPodを探し出す。
- 以下のコマンドを入力し、localhostのポートを上記のリモートデバッグを行いたいコンテナのデバッグポートへポートフォワードする。
```
$ kubectl port-forward <Pod名> <localhostのポート>:<デバッグポート(上記では8082)>
```
- IntelliJでRemoteの設定において、Hostをlocalhost, Portをlocalhost側のポート、`Use module class path` にリモートデバッグを行うJavaのプロジェクト名のクラスパスを指定して実行する。
- 実際にHTTPリクエストを投げるとデバッグを行うことができる

# Authorization ヘッダの Bearer スキームについて
- RFC 6750 は OAuth 2.0 の認可機構として設計されたスキームのようだが、 OAuth に限らず汎用的な HTTP 認可に使ってよいと書いてある

# Go のバージョンアップ方法
- [Managing Go installations - The Go Programming Language](https://golang.org/doc/manage-install)
- インストール手順は以下
```
$ go get golang.org/dl/go1.15.8
$ go1.15.8 download
```
- あとは以下のコマンドを叩いて、GOROOT環境変数に出力された値をセットする
```
// GOROOT確認
$ go1.15.8 env GOROOT
// 環境変数セット
$ vim ~/.bashrc
// 反映
$ source ~/.bashrc
```

# goimports
- import文の自動挿入やコードのフォーマットを自動で実行してくれるツール
```
// フォーマットが不正なファイルをリストアップするコマンド
$ goimports -l .
// フォーマットの整備のみを実行する
$ goimports -format-only <ファイルパス>
```

# sdk manコマンド
```
$ sdk list java
$ sdk default java <jdk名>
```

# Secret の中身を確認したい場合
- 以下のコマンドを叩く
  - kubectl get secret ${Secret名} -o json | jq -r '.data["${取得したいデータ名}"]' | base64 -d
```
$ kubectl get secret hoge-secret -o json | jq -r '.data["tls.key"]' | base64 -d
```
- また、証明書のSANとCNを確認したい場合は、以下のコマンドを実行する
```
$ kubectl get secret ${Secret名} -o json | jq -r '.data["tls.crt"]' | base64 -d | openssl x509 -text | less
...
        Subject: ..., CN=${Common Nameの値}
...
            X509v3 Subject Alternative Name:
                DNS:${Subject Alternative Nameの値}
```

# go test で全てのテストを実行する
- go test ./...

# cookbook と attributes
- cookbookのアップロードを行う際には、attributesの更新は行わない。更新を行う場合は別途行う必要がある

# submodule のコードをローカルに反映するコマンド
```
# (初回のみ)
$ git submodule init
$ git submodule add git@hoge:fuga/piyo.git ${submodule_directory}

# (submoduleのコードの更新)
$ git submodule update
```

# settings.gradle
- マルチプロジェクトの場合は settings.gradle の設定は必須(どのディレクトリをビルド対象にするかの設定を追加する必要がある)
  - http://gradle.monochromeroad.com/docs/userguide/build_lifecycle.html

# OpenSSL でRSA方式の秘密鍵/公開鍵を作成する
```
$ openssl genrsa ${鍵長} > private.key
$ openssl rsa -pubout < private.key > public.key
```

# IntelliJでライブラリが読み込まれない
- 一旦閉じて開き直すと読み込まれるかも

# OSSの著作権について
- オープンソースソフトウェア（OSS）はライセンスに従い自由に使うことができるが、著作権自体は作者に帰属している。

# Spring framework のライブラリのバージョン指定について

- `build.gradle`に以下の記述をしてバージョンを指定すると、`org.springframework.boot` 配下のライブラリを利用する場合に、インストールするバージョンを`springBootVersion`に固定することができる。
```
ext {
    set('springBootVersion', "2.4.3")
}
```

# Ingress で 404 が返ってくる
- Ingress に対してリクエストを実行したところ、以下のようなレスポンスが返ってきた。
```
curl https://${IngressのURL}/
default backend - 404
```
- 確認したところ、Ingressのマニフェストファイルの `spec.tls.secretName` に間違った値が設定されていることが原因だった。

# Ingress で 502 Bad Gateway が返ってくる
- Ingress に対してリクエストを実行したところ、以下のようなレスポンスが返ってきた。
```
curl -k https://${IngressのURL}/
<!DOCTYPE html><html lang="en"><title>502 Bad Gateway</title><body><h1>502 Bad Gateway</h1><footer>nghttpx</footer></body></html>
```
- 確認したところ、ServiceのtargetPortの設定と実際にバックポストするコンテナで起動するサーバのポート番号があっていなかったのでこのレスポンスが返ってきているようだった。

# Ingress のタイムアウトの設定
- ingressのタイムアウトの設定
  - https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/annotations/#custom-timeouts
  - https://github.com/kubernetes/ingress-nginx/issues/2007
  - https://kubernetes.io/ja/docs/concepts/services-networking/ingress/

# stern を使用したPodの横断的なログ確認
- [wercker/stern: ⎈ Multi pod and container log tailing for Kubernetes](https://github.com/wercker/stern)
```sh
$ stern hoge-deployment --container fuga
```

# vim で行末に移動するには
- $ を入力する。

# Go で依存モジュールをインストールする
- `go build` を実行することで依存モジュールをインストールすることができる。

# kustomize の任意のバージョンをインストールする
- gvm でバージョン管理している環境で以下を実行。
```sh
# Go のバージョン確認
$ go version
go version go1.15.8 darwin/amd64
$ git clone https://github.com/kubernetes-sigs/kustomize.git
$ cd kustomize
$ git checkout kustomize/v3.8.9
$ cd kustomize; go install .
$ which kustomize
/Users/${USER}/.gvm/pkgsets/go1.15.8/global/bin/kustomize
```

# 特定の条件に合致するファイルを全削除する
- カレントディレクトリ配下の private.key という名前のファイルを全て削除する
```sh
$ find . -type f -name private.key | xargs rm -f
```

# etcdについて
- クラスタ情報を保管するためのキーバリューストア。主にクラスタ情報のレプリケーションを行う。kubernetes api server のみから呼び出される。
  - https://thinkit.co.jp/article/17453

# CGO_ENABLED=0 について
- 雑な理解だとクロスコンパイル時にはCGO_ENABLED=1、そうでない場合はCGO_ENABLED=0とする模様
  - https://christina04.hatenablog.com/entry/installsuffix-cgo-is-no-longer-required

# Dockerfile の ENTRYPOINT と マニフェストファイルの spec.tamplate.spec.containers.[*].command の関係
- `command` フィールドはDockerfileのENTRYPOINT句を上書きする。
- 例えば Dockerfile で以下のような記述があったとする。
```
ENTRYPOINT ["/usr/local/bin/hoge"]
```
- `docker run <Image名>` を実行した場合、`/usr/local/bin/hoge`が実行される(`docker run` 実行時にはオプションを引き渡せる)。ただ、以下のような設定がマニフェストファイルに存在すると、`/usr/local/bin/hoge -h` が実行される。
```yaml
spec:
  template:
    spec:
      containers:
      - image: ${イメージ}
        command: ["/usr/local/bin/hoge", "-h"]
```

# コンテナ単位のメモリの使用率
- kube_pod_container_resource_limits_memory_bytes: コンテナが使用するメモリの制限 (バイト単位)。

# awk コマンド
- `-v 'OFS=,'` オプションは出力する際に区切り文字をカンマにするというもの。`-F'[,]'` オプションはファイルを読み込む際にカンマで値を分割するというもの。

```sh
cat sample.csv | awk -v 'OFS=,' -F'[,]' '{print $1,$2}'
```

# 1行ごとに処理を実行する
- 以下のコマンドは `list.txt` の各行が `hoge.txt` に存在するかどうかを確認するコマンドである。
```sh
$ cat list.txt | while read line
> do
> cat hoge.txt | grep $line | wc -l
> done
       0
       0
       0
       0
       0
       0
       0
       0
       0
```

# 特定のプロセスのPIDを特定し削除する
```sh
$ PORT=`sudo lsof -nPi:1099 | grep java | awk '{print $2}'`
$ kill -9 $PORT
```

# github で organize を絞って検索
- `org:${organization}` と記述する。

# ssh ログアウト時にプロセスがkillされるのを防ぐ
- https://qiita.com/toshihirock/items/4a6b17a38f9b6e5e7116
```
$ nohup ${コマンド} &
```

# Kubernetes API サーバ URL 確認方法
```
$ kubectl config view --minify | grep server | cut -f 2- -d ":" | tr -d " "
```

# vim で一括置換
$ `:%s/${置換したい文字列}/${置換後の文字列}/`

# resources.limits について
- limitsを設定するとrequestsの値も指定する必要がある。
- limits の上限値は、cpu: 2500m, memory: 1024Mi となる

# git について
- デフォルトブランチには push 不可
- revert すると差分が表れなくなるので、差分を残しておきたい場合はcommit自体を削除する必要がある。
```sh
git commit --allow-empty -m "空commit"
git reset --hard HEAD^^
```

# DB の通信プロトコルについて
- DB の接続はそれぞれ独自のプロトコルで接続している。HTTP と同じ7層で実装しているものもあれば、5,6層で実装しているものも存在する。
 - https://jp.quora.com/DB-he-ha-soketto-tsuushin-wo-shi-te-iru-to-gen-ware-ma-shita-soketto-tsuushin-to-ha-saishuu-teki-ni-nani-toiu-purotokoru-no-tsuushin-shudan-nanode-shou-ka-saito-wo-miru-tokini-saiyou-sa-reru-http-purotokoru-to-nani

# Spring Boot のエラーハンドリング
- Spring Boot アプリケーションで Exception が発生した場合、Dispatcher Servlet は Exception をキャッチしたのち、デフォルトでは /error にマッピングを行う。
  - https://spring.io/blog/2013/11/01/exception-handling-in-spring-mvc

# git clone --recursive
- `git clone --recursive` とすると submodule も clone してくれる
  - https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E3%81%95%E3%81%BE%E3%81%96%E3%81%BE%E3%81%AA%E3%83%84%E3%83%BC%E3%83%AB-%E3%82%B5%E3%83%96%E3%83%A2%E3%82%B8%E3%83%A5%E3%83%BC%E3%83%AB

# TLS について

## TLS 1.2 のハンドシェイク

### Client Hello
- server 側に現在時刻の unix_time と random_bytes(client_secret) を渡す。
- オプションで session_id も渡すことが可能。値は以前のハンドシェイク時に使用した値や、現在のハンドシェイク時に使用した値、また他のコネクションで使用している値などを使用する。新規での接続の場合はこの値は空になる。

## TLS 1.3 のハンドシェイク

### Phase
- Key Exchange: 共有鍵を作成する際に必要となるパラメータや、暗号化のパラメータを交換するフェーズ。このフェーズの後通信は暗号化される。
- Server Parameters: 他に必要となるハンドシェイクのパラメータを決定する(クライアントの認証が必要か、アプリケーションレイヤでのサポートが必要かなど)。
- Authentication: サーバを認証し、キーを確認してハンドシェイクを確立させる。

### Client Hello
- Client Hello は以下を含む
  - ClientHello.random
  - protocol versions
  - a lilst of symmetric chpher/HKDF hash pairs
  - key_share, pre_shared_key, もしくは両方
    - key_share は DH 鍵を使用する場合に付与
    - pre_shared_key は PSK 鍵を使用する場合に付与。
- 以下の情報をServerに与える
  - cipher suites: AEADアルゴリズム/HKDF のハッシュペアの一覧
    - (Hashアルゴリズムの)
  - supported_groups: Client がサポートする (EC)DHEの種類。またその共有鍵
  - signature_algorithms: Client がサポートしているsignatureアルゴリズムの種類。
  - pre_shared_key: 事前共有鍵での通信で使用する共有鍵のリスト。


## 資料
- https://tex2e.github.io/rfc-translater/html/rfc5246.html

# LinkedList と ArrayList の違い
- ランダムなアクセスを行う場合は ArrayList の方が高速に処理できる。しかし要素の追加や削除といったような操作は遅い。LinkedList はランダムなアクセスを行う場合に遅くなる一方要素の追加や削除の操作は高速。
  - https://qiita.com/BumpeiShimada/items/522798a380dc26c50a50

# KEY=VALUE が並んだファイルの中身を一気に export する
- https://www.koikikukan.com/archives/2013/09/11-005555.php
- https://qiita.com/uraura/items/739602890d7e6c96974a

# DSR (Direct Server Return)
- ロードバランサを介してクライアントがリクエストを実行した場合にレスポンスをロードバランサではなく直接クライアントに送信する方式のことを指す。Layer によって L2DSR, L3DSR と DSR にも色々と種類がある。L2DSR は VIP 宛に送られてきたパケットのMACアドレスをリアルサーバのMAC アドレスに書き換えて実現する。MACアドレスを用いて通信を行うので、ロードバランサとリアルサーバは同じ VLAN に属す必要がある。これに対して L3DSR は MAC アドレスではなく IP アドレスで通信を行うため必ずしも同じ VLAN に属す必要はない。

# ウェルノウンポート
- ウェルノウンポートは一般ユーザの権限で動くデーモンに bind できない。したがって80/443番ポートでWebアプリを起動する場合は何かしらの対応を行う必要がある。
- systemd を使用してデーモンを起動する場合は、ambient capabilities という機能を使用してウェルノウンポートでアプリケーションを起動する権限を付与する。
  - https://nojima.hatenablog.com/entry/2016/12/03/000000
  
# systemd によるプロセスクラッシュ時の挙動  
- systemd で起動すればクラッシュ時に自動的に再起動してくれる。
  - https://tex2e.github.io/blog/linux/systemd-restart-config

# gradle タスクによる rpm 作成
- https://github.com/nebula-plugins/gradle-ospackage-plugin/wiki/RPM-Plugin

# sonarqube
- https://dev.classmethod.jp/articles/sonarqube-source-analytics-1/

# 画面収録
- Shift + Command + 5 でスクショの画面収録が可能。
- PC の内部音声に関しては BlackHole というソフトを使用すれば可能。
  - https://dev.classmethod.jp/articles/record-sounds-on-mac-with-blackhohle/

# Java で HTTPS 通信を行うには
以下コマンドを実行する。keystoreのパスには IntteliJ では 「Project Structure」>「Platform Settings」>「SDKs」>「JDK home path」に設定されているパスに「/lib/security/cacerts」を加えたもの。
```sh
$ keytool -import -file ${接続先のサーバ証明書のパス} -alias server -keystore ${keystoreのパス} 
# 例
$ keytool -import -file sample.crt -alias server -keystore /Users/${ユーザ名}/.sdkman/candidates/java/${使用しているJDKバージョン}/lib/security/cacerts
```

# logger コマンド
logger コマンドの実行結果はデフォルトでは /var/log/messages に出力される。

# TLS 通信実装時のアプリケーションの起動エラー
TLS 通信を実装した場合に以下の例外が発生。

```
Caused by: java.lang.IllegalArgumentException: Key protection algorithm not found: java.security.UnrecoverableKeyException: Encrypt Private Key failed: unrecognized algorithm name: PBEWithSHA1AndDESede
	at org.apache.tomcat.util.net.AbstractJsseEndpoint.createSSLContext(AbstractJsseEndpoint.java:99) ~[tomcat-embed-core-9.0.52.jar:9.0.52]
```

使用しているJDKが原因で例外が発生していたようなので、デフォルトの Mac ではなく OpenJDK11 を使用したところ、起動に成功した。

# tcpdump で取得したダンプファイルを wireshark で確認する
```
sudo tcpdump -s 0 -w ~/Desktop/tcp_dump.out port 8443
```

# TLS 通信のデバッグを出力する
IntelliJ で VMOptions に `-Djavax.net.debug=all` を指定する。

# VS Code のマルチカーソル
option + commnand + 十字キー

# kubeval
kubernetes のマニフェストファイルを検証するツール。以下関連資料。
- https://github.com/instrumenta/kubeval
- https://github.com/garethr/kubernetes-json-schema
- https://github.com/instrumenta/kubeval/issues/301

# curl で実行時間を計測する
curl -so /dev/nul -w "time_total: %{time_total}\n" https://www.google.com/

# Go のテンプレートで変数宣言したい場合
```sh
{{ $alert := index .Alerts 0 }} ({{ .Status }}) HOGE ({{ if eq $alert.Labels.namespace "" -}}Kubernetes{{- else -}}{{ $alert.Labels.namespace }}{{- end }}) {{ $alert.Labels.severity }} - SERVER TROUBLE REPORT
```