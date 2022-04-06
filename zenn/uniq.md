
# uniq で差集合を求める

# 背景
- 業務中に「サーバAにアクセスしてきたユーザのリストと、サーバBにアクセスしてきたユーザのリストをそれぞれ作成してほしい。ただしサーバAとBに両方アクセスしたユーザもいるので、重複を避けるために両方にアクセスしてきたユーザはサーバAのリストの方のみに含めて欲しい」という依頼がされました。イメージだと以下のような感じです。


- uniq コマンド使えばできそうだなーとは思っていたのですが、実際にどうやったのかを記事としてまとめておきます。

# ゴール
以下ような集合に対応したリストを作るのが目的になります。
- A 
- A / B

# やったこと
- まずそれぞれのユーザのリストが与えられたとします。サンプルを以下に用意しました。
```sh
# リストの確認
$ cat list_a.txt
AAA
BBB
CCC
DDD
EEE

$ cat list_b.txt
CCC
DDD
EEE
FFF
GGG
```


https://www.headboost.jp/what-is-ensemble/
```sh


# A ∩ B 
$ sort list_a.txt list_b.txt | uniq -d
CCC
DDD
EEE

# A \ B
$ sort list_a.txt list_b.txt | uniq -d > repeated.txt
$ sort list_a.txt repeated.txt | uniq -u
AAA
BBB

# B \ A
$ sort list_b.txt repeated.txt | uniq -u
FFF
GGG
```