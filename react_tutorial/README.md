<!-- TOC -->

- [React チュートリアル](#react-チュートリアル)
  - [公式サイト](#公式サイト)
  - [メモ](#メモ)
    - [MAIN CONCEPTS](#main-concepts)
      - [2. JSX の導入](#2-jsx-の導入)
      - [3. 要素のレンダー](#3-要素のレンダー)
      - [4. コンポーネントと props](#4-コンポーネントと-props)
      - [5. state とライフサイクル](#5-state-とライフサイクル)
      - [6. イベント処理](#6-イベント処理)
      - [8. リストと key](#8-リストと-key)
      - [10. state のリフトアップ](#10-state-のリフトアップ)

<!-- /TOC -->
# React チュートリアル

## 公式サイト
- https://ja.reactjs.org/tutorial/tutorial.html#before-we-start-the-tutorial  
- https://ja.reactjs.org/docs/hello-world.html  

## メモ

### MAIN CONCEPTS

#### 2. JSX の導入

JSX とは javascript の構文を拡張したもの。以下が例。

```js
const element = <h1>Hello, world!</h1>;
```
こんな感じで JS の変数に HTML のタグを代入するイメージ。  
そして実際にHTMLとしてレンダリングするには、以下のような実装を行う。
```js
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```
ちなみに React ではユーザの入力は自動的にエスケープされるため、XSS のようなインジェクション攻撃をデフォルトで防ぐことができる。  
また、React は他のライブラリとの連携を行うためのフックを提供している。例えば、入力された文字を自動的に MarkDown に変換するといった処理が可能である。

#### 3. 要素のレンダー

HTML ファイルに div 要素があったとする。
```html
<div id="root"></div>
```
この中にあるもの全てが `React`によって制御されることになる。デフォルトの React では root 要素は一つだけだが、既存のアプリに React を導入する場合は root 要素を複数持たせることができる。  
また、React 要素はイミュータブルであり、DOM の変更を行うためには`ReactDOM.render()`メソッドを使用する必要がある。  
また、DOM の更新は、前の DOM の状態と比較して、更新が必要な部分のみ更新を行う。

#### 4. コンポーネントと props 
React では UI を独立した部品で構成することができる。この部品のことをコンポーネントと呼ぶ。コンポーネントを作成するには関数を使用した方法と、クラスを使用した方法の2通り存在する。

関数を使用した記法
```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

クラスを使用した記法
```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

このコンポーネントを React DOM で使用するには、以下のように実装する。
```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
ちなみにコンポーネントは大文字始まりで定義する。  
コンポーネントの規模はできるだけ小さいものにする。以下のような JSX を返すコンポーネントがあったとする

```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
これを複数のコンポーネントで構成するように修正する。まず `Avatar` コンポーネントを抽出する。

```js
function Avatar(props) {
    return 
      <img className="Avatar"
          src={props.user.avatarUrl}
          alt={props.user.name}
      />
}
```
これを先ほどの JSX に適用すると、以下のようになる。
```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
このように、小さな部品を組み合わせて大きなコンポーネントを構成していくようにする。  
また、`props` は読み取り専用にすべきであり、コンポーネント内で変更すべきではない。

#### 5. state とライフサイクル
コンポーネントに状態をもたせ、状態の変化に応じて DOM をレンダリングさせることができる。そのためには、クラスを用いたコンポーネントで state を使用する。

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```
処理の流れは以下の通りである。
1. `<Clock />` が ReactDOM.render()に渡されると、コンストラクタが呼ばれ、date state に現在時刻がセットされる。  
2. Clock コンポーネントの render() メソッドが呼ばれ、実際にレンダリングが実行される。  
3. レンダリングされた(DOM にマウントされた)のち、componentDidMount() ライフサイクルメソッドが呼ばれ、１秒ごとに state.date が更新されるようになる。  
4. tick() メソッドの`this.setState()`で state が変更されるので、その変更が React に検知される。  
5. 変更が検知されたら前の DOM との差分を確認し、DOM が再度レンダリングされる。  
6. Clock コンポーネントがDOM から削除されると componentWillUnmount() メソッドが呼ばれる。  

state の注意点としては以下。  
1. state を変更する場合は setState() を使用する。使用しないとレンダリングがされない。  
2. state と props の更新は非同期で行われるため、確実に前の状態の state を使用したい場合は、 setState() メソッドを以下のように使用する。  
```js
// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```
この場合、 第一引数は前の状態の state, props は更新が適用される時点での props を表している。  
3. state には複数の変数を格納することができ、それぞれ個別に更新することが可能。  
4. 親要素の state を使用したい場合は、props として渡してあげる。  

#### 6. イベント処理

イベントを受け取って何か処理を行う場合は以下のような実装をする。
```js
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```
ざっくり言うと、`handleClick` メソッドを実装してそれをバインドさせれば良い。バインドの構文を省略したい場合は、アロー関数を使用することで実現できる。
```js
handleClick = () => {
    console.log('this is:', this);
}
```

#### 8. リストと key

```js
function ListItem(props) {
  // Correct! There is no need to specify the key here:
  return <li>{props.value}</li>;
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // Correct! Key should be specified inside the array.
    <ListItem key={number.toString()}
              value={number} />

  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```
1. 要素を一意に特定できるために、key は指定しておくこと。  
2. key はあくまで兄弟要素において一意であれば良い。  

#### 10. state のリフトアップ
ある state の更新を様々なコンポーネントに反映したい時は、一番近い共通の親コンポーネントを利用して更新を共有する。
