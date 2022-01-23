Iterator パターンについて

# この記事について
GoF のデザインパターンの一つである Iterator パターンについて解説してきます。実装は Go です。

# 概要
Iterator パターンとは、配列などの集合に対して各要素に決まった操作を繰り返す際に使用されるデザインパターンです。Iterator パターンを使用せずとも繰り返しの処理は実装できますが、Iterator パターンを採用することで仕様変更に強いコード構成にすることが可能です。

# 実装
実際に実装例を見ていきます。今回はショッピングカートに入れている商品の名前を列挙するような処理を実装することを考えてみましょう。まずは基本的なインターフェースの実装を紹介します。

```go
package iterator

type Aggregate interface {
	Iterator() Iterator
}

type Iterator interface {
	HasNext() bool
	Next() (*Item, error)
}
```
後者の `Iterator` インターフェースは Iterator パターンにおける繰り返し処理の実行を担っています。具体的には繰り返し処理を行う際の終了条件を `HasNext()` で行い、次の要素を取得する処理を `Next()` で行います。具体的な実装はこのインターフェースを実装する構造体に委ねています。<br>
`Aggregate` インターフェースは `Iterator` インターフェースを返す `Iterator()` を提供しており、繰り返しの処理を呼び出し側に提供する場合はこのインターフェースを実装する必要があります。<br>

続けて商品、ショッピングカートをデザインパターンに則してモデル化を行います。以下のような実装になります。
```go
type Item struct {
	name string
}

func NewItem(name string) *Item {
	return &Item{name}
}

func (i *Item) GetName() string {
	return i.name
}

type ShoppingCart struct {
	items []*Item
}

func NewShoppingCart() *ShoppingCart {
	return &ShoppingCart{
		items: []*Item{},
	}
}

func (s *ShoppingCart) GetItemAt(index int) (*Item, error) {
	if len(s.items)-1 < index {
		return nil, fmt.Errorf("index out of bounds. index=%d", index)
	}
	return s.items[index], nil
}

func (s *ShoppingCart) AppendItem(item *Item) {
	s.items = append(s.items, item)
}

func (s *ShoppingCart) GetLength() int {
	return len(s.items)
}

func (s *ShoppingCart) Iterator() Iterator {
	return NewShoppingCartIterator(s)
}

type ShoppingCartIterator struct {
	shoppingCart *ShoppingCart
	index        int
}

func NewShoppingCartIterator(shoppingCart *ShoppingCart) *ShoppingCartIterator {
	return &ShoppingCartIterator{shoppingCart: shoppingCart, index: 0}
}

func (s *ShoppingCartIterator) HasNext() bool {
	if s.index < len(s.shoppingCart.items) {
		return true
	}
	return false
}

func (s *ShoppingCartIterator) Next() (*Item, error) {
	if s.index < len(s.shoppingCart.items) {
		item := s.shoppingCart.items[s.index]
		s.index += 1
		return item, nil
	}
	return nil, fmt.Errorf("index out of bounds. index=%d", s.index)
}
```
`ShoppingCart` は `Aggregate` を実装しており、`Iterator()` が呼ばれた場合に `ShoppingCartIterator` を返すような実装としてます。繰り返し処理はこの `ShoppingCartIterator` を使用することで実装することが可能です。実際に繰り返し処理を `main()` で実装してみます。

```go
func main() {
	shoppingCart := iterator.NewShoppingCart()
	shoppingCart.AppendItem(iterator.NewItem("HOGE"))
	shoppingCart.AppendItem(iterator.NewItem("FUGA"))
	shoppingCart.AppendItem(iterator.NewItem("PIYO"))

	i := shoppingCart.Iterator()
	for i.HasNext() {
		item, _ := i.Next()
		fmt.Println(item.GetName())
	}
}
```
実行結果は以下となります。
```sh
HOGE
FUGA
PIYO
```

# メリット
Iterator パターンのメリットに関しては Iterator パターンを採用せずに繰り返し処理を実装する場合にどのようなことが起こるのかを考えるとわかりやすいかもしれません。まず Iterator パターンを採用しなかった場合に繰り返し処理を実装すると以下のようになるかと思います。まず `ShoppingCart` に以下のメソッドを追加して呼び出し側(`main()`)が繰り返し処理を実装できるようにします。
```go
func (s *ShoppingCart) GetItems() []*Item {
	return s.items
}
```
続いて `main()` の実装をは以下のようになります。
```go
func main() {
	shoppingCart := iterator.NewShoppingCart()
	shoppingCart.AppendItem(iterator.NewItem("HOGE"))
	shoppingCart.AppendItem(iterator.NewItem("FUGA"))
	shoppingCart.AppendItem(iterator.NewItem("PIYO"))

	for _, e := range shoppingCart.GetItems() {
		fmt.Println(e.GetName())
	}
}
```
一見問題ないように見えますが、ここで「最後に追加した Item から順に名前を出力する」というように実装に変更が発生したとすると、以下のように `main()` の方を改修しなければなりません。
```go
func main() {
	shoppingCart := iterator.NewShoppingCart()
	shoppingCart.AppendItem(iterator.NewItem("HOGE"))
	shoppingCart.AppendItem(iterator.NewItem("FUGA"))
	shoppingCart.AppendItem(iterator.NewItem("PIYO"))

	items := shoppingCart.GetItems()
	length := len(items)
	for i := length-1; i > -1; i-- {
		fmt.Println(items[i].GetName())
	}
}
```
今は `main()` の一箇所のみ改修が発生しましたが、この繰り返し処理が複数箇所で実装されている場合はそれぞれの箇所を同じように改修しなければなりません。したがって Iterator パターンを採用しなかった場合は仕様の変更に弱い実装になってしまいます。<br>
一方 Iterator パターンを採用した場合は以下のように `ShoppingCartIterator` の改修のみになります。
```go
func NewShoppingCartIterator(shoppingCart *ShoppingCart) *ShoppingCartIterator {
	return &ShoppingCartIterator{shoppingCart: shoppingCart, index: len(shoppingCart.items)-1}
}

func (s *ShoppingCartIterator) HasNext() bool {
	if s.index > -1 {
		return true
	}
	return false
}

func (s *ShoppingCartIterator) Next() (*Item, error) {
	if s.index > -1 {
		item := s.shoppingCart.items[s.index]
		s.index -= 1
		return item, nil
	}
	return nil, fmt.Errorf("index out of bounds. index=%d", s.index)
}
```
このように Iterator パターンを採用した場合のメリットとしては `main()` のような呼び出し側のコードに改修が発生しません。したがって Iterator パターンを採用しなかった場合と比較して仕様変更に強い実装となっています。