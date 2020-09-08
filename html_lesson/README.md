

# 23
- position
  - https://developer.mozilla.org/ja/docs/Web/CSS/position
  - html 要素を背面に配置するか前面に配置するかを指定したい場合は、 position が static 以外かつ z-index の値に数値を指定することで制御可能
  - LocalStackContextとRootStackContextというものが存在して、各要素のZ軸方向の配置は同じContextで行わなければならない。
  - position=sticky の場合は要素が画面をスクロールしても決まった位置に固定される。

# 24
- 擬似要素

各要素について理解する



# CSS の基礎から始めよう

## 





position
- (親要素, 子要素) = (設定なし, absolute) => ページの左上から設定した分だけ移動される。
- (親要素, 子要素) = (relative, absolute) => 子要素は親要素の位置から設定した分だけ移動される。
- fixed => 画面に固定される
- sticky => 親要素内で fixed の挙動を取る。
- overflowhidden が親要素に設定されている場合は, sticky は無効となる。
- z-index が有効となるのは,positionが static 以外の場合有効
- z-index が auto だと、ローカルスタックコンテキストは生成しない。
- root の ローカルスタックコンテキストというものも存在する。
- sticky は height を設定
- position の値が　static 以外で z-index の値が数値である時、ローカルスタックコンテキストが生成され、z-index の値に応じて各要素のz軸方向の位置関係が決定される。

# まとめ
display プロパティについて
- block は幅と高さが指定でき、要素は縦に並ぶ。inline は高さと幅は指定できず、要素は横に並ぶ。inline-block は幅と高さが指定でき、要素は基本的に横に並び、区切りのいいところで改行される。
- 参考文献
  - https://developer.mozilla.org/ja/docs/Learn/CSS/CSS_layout/Introduction

要素を重ねたくない場合、flex,　重ねたい場合は position による配置を行う。


## 擬似セレクタに関して


## VSCode の Emmet のショートカット
### テキストにインデックス情報を含めて複数の要素を生成したい場合
- 例えば、Item n (n = 1, 2, 3...) というテキストを含むspan要素を5つ生成する場合は以下のショートカットを使用する
```
span{Item $}*5
```

### 資料
- https://qiita.com/tedkuma/items/67876e6be3369b0e730c


## 質問事項
46 について
- animation-fill-mode: normal を指定すると、開発者ツールではその値が invalid であるとのことであった。
  - 講座の中でも invalid となっている
- また、normal 以外の値を指定しても、ズームのアニメーションの transitionの duration が効いていないような挙動となった。
- ソースコードは5-8

transform opacity

after => スライドの役割
before => ホバーされたら暗くする役割

# マウスイベント一覧
- https://developer.mozilla.org/ja/docs/Web/API/Element

# DOM (Document Object Model)
- javascript 側から html の要素にアクセスするためのインターフェース

# レンダリングの流れ
1. ブラウザが html を取得
2. 内容に基づいて DOM に変換する
3. DOM を元に CSS に記載されているスタイルを適用したり、画像を表示したりする。

- 2. の後に発火するイベントが `DOMContentLoaded`, 3. のあとに発火するイベントが `load


インスタンス生成じに class="char" の要素が生成されている
https://leap-in.com/ja/category/blog/

JSDoc 書き方
https://daa-kitamura.ssl-lolipop.jp/web/jsdoc/


1. 判定線と交差したら、.inview クラスを追加
2. 

- swiper(ヒーロースライダー)
  - https://swiperjs.com/get-started/
- pace(プログレスバー)
  - https://github.hubspot.com/pace/docs/welcome/
- bootstrap-reboot.css, normalize.css を最初に適用するのがベスト
- ブラウザのデフォルトフォントの差異をなくすためにサーバからフォントをあらかじめダウンロードしておいて共通のフォントを使用する

<link href="https://fonts.googleapis.com/css2?family=Kameron:wght@400;700&display=swap" rel="stylesheet">
<link href="https://fonts.googleapis.com/css2?family=Kameron:wght@400;700&family=Noto+Sans+JP&display=swap" rel="stylesheet">

font-family: 'Kameron', 'Noto Sans JP', sans-serif;

タブレットサイズの時、2*2
デスクトップサイズの場合、1*4
image-p1~p4.jpg

# em (emphasis)
- 現在適用されている親要素のフォントサイズを基準として計算される大きさのことを指す。例えば 1em とは現在適用されているフォントサイズと同じ大きさを適用するという意味である。この設定を適用すると、基準となる要素の大きさが変更されるに伴い、emを設定している要素の大きさも変更されるため、個別に値を再設定しなくてよくなる。

# rem (root emphasis)
- 現在適用されている HTML 要素のフォントサイズを基準として計算される大きさのことを指す。

lesson 109 の11:15 から再開


minify 
webpack
gulp
netlify

- udemy の復習
- bootstrap の学習
- 


# コースの復習
- header
- content
  - hero-slider
  - side-menu
  - travel
  - houses
  - popular
- footer
- mobile-menu

# bootstrap の学習
- https://manablog.org/code-life-start/
- dotinstall での学習


# 復習が必要な箇所
- display プロパティについて
- z-index のスタックコンテキストについて
- position: relative と absolute の関係

1:59勉強

durattion 1.6s
ease-in-out 
fill-mode normal

eaebe6

https://qiita.com/7968/items/1d999354e00db53bcbd8#no07-animation-fill-mode-%E3%83%97%E3%83%AD%E3%83%91%E3%83%86%E3%82%A3



https://w3g.jp/css/guide/target


curl -v -X POST localhost:8080/ -H "Method: hoge" -H "ServerType: fuga" -d '{"hoge": "foo", "fuga": "bar"}'


擬似要素にホバー時の処理を実装したい場合は、以下のように親要素`:hover::{擬似要素}`という書き方をする

```scss
.hover-darken {
  ...

&:hover::before {
      opacity: 1;
      background-color: rgba(0, 0, 0, 0.4);
  }
}
```

justify-content は親要素に付与する
flex-basis は子要素に付与する(子要素によっては取りうる値が変化するため)
flex-wrap: wrap は要素を配置するときに折り返して配置するための設定。親要素に付与。


# 要素を下揃えにしたい場合、親要素に以下を付与する。
display: flex;
align-items: flex-end;

# 画像を親要素と同じ高さにしつつ、アスペクト比を保ちたい場合は以下のように設定する。
&__img {
    height: 300px;

    & > img {
        object-fit: cover;
        height: 100%;
        width: 100%;
    }
}


上下中央揃えは align-items: center とする
inline 要素の中央揃えは text-align: center とする


内部SEO対策として、目的にあったタグを使用する。

定められた高さで aligin-items を用いて上下中央揃えを行いたい場合は、要素の height プロパティの値をその高さに設定してやる必要がある。そうしないと height: auto; となりうまくいかない。

とりあえず困ったら display: flex; を指定すればいい感じに中央に配置してくれる。

aligin-itemsのデフォルト値は stretch で、これは子要素の集合の高さが親要素の高さになる。aligin-items: center とすると、子要素の高さは要素の高さを計算し、親要素の高さと同じになるように上下にマージンが付与される。

テキストを大文字にしたい場合は、親要素に text-transform: uppercase を付与する。



display のデフォルト値は inline
vertical-align は inline 要素の上下の配置を決定するプロパティ
text-align はブロック要素または表セルボックスの水平方向の配置を設定する


margin: 0 auto; で左右に均等に余白を設けたい場合は、親要素の幅を指定してあげる必要がある。

vertical-align プロパティは inline 要素にしか効かない。
flexbox の要素を順序を逆にしたいときは、flex-direction: row-reverse; を指定する。flex-wrap 要素とまとめて指定したい場合は、flex-flow: row-reverse wrap; といったように指定することできる。

scss には少数をパーセンテージの値に変換するためには、 percentage() 関数を使用する。


- li 要素のインラインテキストを上下中央揃えにしたい場合、line-height プロパティを使用する。
https://developer.mozilla.org/ja/docs/Web/CSS/line-height
https://developer.mozilla.org/ja/docs/Web/CSS/display


今日の積み上げ(50日目) 2.15h

- 色々実装

【Tips】
- li 要素のインラインテキストを上下中央揃えにしたい場合、line-height プロパティを使用する。
https://developer.mozilla.org/ja/docs/Web/CSS/line-height
https://developer.mozilla.org/ja/docs/Web/CSS/display


今日の積み上げ(51日目) 2.75h

- footer 途中まで実装

【Tips】
- background-image による背景画像のアスペクト比を保つためには、擬似要素にpadding-topプロパティを指定して高さを指定する必要がある。

今日の積み上げ(52日目) 1.5h

- footer 途中まで実装

【Tips】
- inline 要素をvertical-align: middle で上下中央揃えにしたい場合は、とりあえずマージンを取っておけば大丈夫


今日の積み上げ(53日目) 1.75h

- footer 途中まで実装

【課題】
- 要素の中央, 左右の揃え方。


今日の積み上げ(54日目) 2.1h

- footer 完成

【Tips】
- 入力フォームには input タグ、プルダウンメニューには select タグを使用する。


復習する項目
7
17
18
20
34
76
78


今日の積み上げ(55日目) 1h

- Effective Java 復習

【Tips】
- 使用されなくなったオブジェクト参照をGCの対象にするには、WeakHashMap を使用する。


三角形はcssだけで実装できる

今日の積み上げ(56日目) 2h

- ポートフォリオサイト作成(ヘッダ部分)

【Tips】
- 斜めの線を描写するには、transform プロパティを skewYに設定する。
- margin: 0 auto; による中央寄せが効かない場合は、親要素のwidthが設定されていないことが原因として存在する。

今日の積み上げ(57日目) 1.5h

- ポートフォリオサイト作成(ヘッダ部分)


今日の積み上げ(58日目) 2.0h

- ポートフォリオサイト作成(ヘッダ部分)

transition プロパティの対象に display プロパティを指定しても思った通りの挙動とならない場合が存在する。

transition プロパティの対象に display プロパティを指定しても思った通りの挙動とならない場合が存在する。
http://bashalog.c-brains.jp/19/10/02-095448.php

visibility: hidden; と opacity: 0 の違いは、visibility: hidden; を設定すると、要素が存在しなくなるためにボタンやリンクをクリックした時に発火するイベントが機能しなくなる。それに対し、opacity: 0; を設定すると、要素が見えなくなるだけでクリックするとイベントは発火する。
http://var.blog.jp/archives/51539053.html

今日の積み上げ(59日目) 2.0h

- ポートフォリオサイト作成(hero部分)

【Tips】
上向きの正三角形を作るには、border-left,right,bottom の border の太さの比を 1:1:√3 とし、border-bottom に色をつけその他は透明にする。


今日やること
- アコーディオン実装
  - 480px の場合のみ。それ以外は表示しない
- Service セクション実装
  - 480pxの場合、アイコンと文字は縦に並べる
  - 600pxの場合、アイコンと文字は横に並べる
  - 960pxの場合、アイコン同士を横に並べる

今日の積み上げ(60日目) 2.5h

- ポートフォリオサイト作成(service部分)

【Tips】
- アコーディオンを実装する場合は、margin-height プロパティを使用して表示する要素の高さに制限をかけ、ボタンがクリックされた段階で制限を外す。
- linear-gradient 関数で色のグラデーションを指定することが可能。


今日の積み上げ(61日目) 2.5h

- ポートフォリオサイト作成(service部分)

【詰まった点】
- inline 要素の上下中央揃え
- 複雑な flex 要素の設定



背景の大きさと初期位置を設定する場合は、background-size, background-position プロパティを使用する。
div や img 要素のアスペクト比を1:1 に固定したい場合は、before 要素の padding-top の値を 100%にする。


< 480 px 画像1つ
< 960 px 画像2つ
>= 960 px 画像3つ

う
今日の積み上げ(66日目) 2.25h

- ポートフォリオサイト作成

【Tips】
- Swiper.js でスライドショーを作成する場合は、親要素に overflow: hidden; をつけないと予期しない挙動となる
- スライドショーでフォーカスしている要素を中央に持ってきたい場合は、centeredSlides オプションを true に指定する




今日の積み上げ(66日目) 2.25h

- ポートフォリオサイト作成

pタグのテキスト上下中央揃えは display: flex; aligin-items: center; で行うのがベスト。
span はinline要素なので、左右中央揃えにする場合は inline-block 要素に変換して width を設定し、text-align: center を指定する。


今日の積み上げ(67日目) 2h

- ポートフォリオサイト作成

【詰まった点】
親要素に display: flex; を指定している場合、三角形が表示されない事象が発生した。


今日の積み上げ(68日目) 1.5h

- ポートフォリオサイト作成

【Tips】
- レスポンシブ対応でレイアウトを変更する場合、要素の指定は flex-direction/wrap か flex-flow　どちらかに統一したほうがいい


今日の積み上げ(69日目) 2.5h

- ポートフォリオサイト作成

【Tips】
- クリックするとページのトップにスクロールするボタンを実装する場合は、jQuery の animate() メソッドのプロパティに {scrollTop: 0} を設定する。


Domain Driven Development 
VR Applicaiton
Golang
Kotlin
Java
Kubernetes
Linux
Chef
Spring framework
SCSS
HTML
Javascript
Python
Machine Learning
MySQL
Oracle


"https://udemy-utils.herokuapp.com/api/v1/events/9?token=token123"


0.20.0

yarn add material-ui@0.20.0

bindメソッドを使用する場合はどのようなケースかを調べる