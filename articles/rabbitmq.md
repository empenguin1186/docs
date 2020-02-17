# rabbit mq 事始め

インストールしたら以下のようなエラーが発生
```
$ brew install rabbitmq
...
Error: The `brew link` step did not complete successfully
The formula built, but is not symlinked into /usr/local
Could not symlink sbin/cuttlefish
/usr/local/sbin is not writable.
```

確認したらそもそも/usr/local/sbinなんてフォルダは存在しなかった。
```
$ ls /usr/local/sbin
$ 
```

なので/usr/local/sbinフォルダを作成すればインストールされると考え実行
```
$ sudo mkdir /usr/local/sbin
```

再びインストールしようとしたらエラーが発生
```
$ brew install rabbitmq
Updating Homebrew...
Error: The following directories are not writable by your user:
/usr/local/sbin

You should change the ownership of these directories to your user.
  sudo chown -R $(whoami) /usr/local/sbin
```
所有者を自分に変更してくれとのこと。sbinフォルダを作成するときにsudo実行をしたので現在の所有者はrootになっている
一応確認

```
$ ls -l /usr/local/
...
drwxr-xr-x    2 root     wheel     64  3 20 09:28 sbin
```
したがってエラーメッセージに記載されているコマンドを実行して所有者を変更する

```
sudo chown -R $(whoami) /usr/local/sbin
```
確認

```
$ ls -l /usr/local/
drwxr-xr-x    2 kodaira  wheel     64  3 20 09:28 sbin
```

ちゃんと自分が所有者になっていることが確認できた。
晴れてinstall実行

```
brew install rabbitmq
Updating Homebrew...
==> Downloading https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.7.12/rabbitmq-server-generic-unix-3.7.12.tar.xz
Already downloaded: /Users/kodaira/Library/Caches/Homebrew/downloads/61c948d68d0b7125665cc1429cb78f66f086489a42ab3b2382f361441e942e26--rabbitmq-server-generic-unix-3.7.12.tar.xz
==> /usr/bin/unzip -qq -j /usr/local/Cellar/rabbitmq/3.7.12/plugins/rabbitmq_management-3.7.12.ez rabbitmq_management-3.7.12/priv/www/cli/rabbitmqadmin
==> Caveats
Management Plugin enabled by default at http://localhost:15672

Bash completion has been installed to:
  /usr/local/etc/bash_completion.d

To have launchd start rabbitmq now and restart at login:
  brew services start rabbitmq
Or, if you don't want/need a background service you can just run:
  rabbitmq-server
==> Summary
🍺  /usr/local/Cellar/rabbitmq/3.7.12: 236 files, 13.8MB, built in 14 seconds
$
$ ls /usr/local/sbin/
cuttlefish            rabbitmq-diagnostics  rabbitmq-plugins      rabbitmqadmin
rabbitmq-defaults     rabbitmq-env          rabbitmq-server       rabbitmqctl
```
ちゃんとinstall されてるっぽい

起動
```
/usr/local/sbin/rabbitmq-server
ERROR: epmd error for host localhost: nxdomain (non-existing domain)
```
なんかエラッた

status 確認
```
/usr/local/sbin/rabbitmqctl status
Status of node rabbit@localhost ...
Error: unable to perform an operation on node 'rabbit@localhost'. Please see diagnostics information and suggestions below.
```
なんかlocalhostのDNSが設定されていないからエラってるっぽい
/etc/hostsを確認したら確かにlocalhostのDNSが設定されていなかったので追加

```
127.0.0.1 localhost
```
再度実行

```
/usr/local/sbin/rabbitmq-server

  ##  ##
  ##  ##      RabbitMQ 3.7.12. Copyright (C) 2007-2019 Pivotal Software, Inc.
  ##########  Licensed under the MPL.  See http://www.rabbitmq.com/
  ######  ##
  ##########  Logs: /usr/local/var/log/rabbitmq/rabbit@localhost.log
                    /usr/local/var/log/rabbitmq/rabbit@localhost_upgrade.log

              Starting broker...
 completed with 6 plugins.
 ```
起動している？