Golang による Strategy パターン

# この記事について
GoF のデザインパターンの一つである Strategy パターンについて解説してきます。実装は Go です。

# 概要
Strategy パターンの Strategy は「戦略」を意味しますが、ここでいう「戦略」は「アルゴリズム」のことを指します。Strategy パターンは状況に応じて同じ問題を別のアルゴリズムを使用して解く際に有用なデザインパターンとなります。

# 実装
実際に実装例を見ていきます。今回は木構造のデータを探索するために深さ優先探索と幅優先探索のアルゴリズムを用意して Strategy パターンの実装を行っていきます。<br>
例えば以下のような木構造のデータが与えられたとします。
```sh
         0
       /   \
      1     2
     / \   / \
    3   4 5   6
```
深さ優先探索、幅優先探索をそれぞれ行った場合、探索を行ったノードの順番は以下のようになります。

|  アルゴリズム  |  探索順序  |
|:--:|:--:|
|  深さ優先探索  |  0,1,3,4,2,5,6  |
|  幅優先探索  |  0,1,2,3,4,5,6  |

今回はこの探索アルゴリズムを切り替えて処理できるような実装を行っていきます。まずは Strategy を表現するインターフェースを定義します。

```go
package strategy

type SearchStrategy interface {
	Search(graph map[int][]int, start int, seen []bool) []int
}
```
`SearchStrategy` インターフェースの `Search()` メソッドが実際に探索を行うメソッドとなります。具体的な探索アルゴリズムはこのメソッドに実装していくことになります。<br>
続けて `SearchStrategy` の実装クラスを以下に示します。
```go
type DepthFirstSearchStrategy struct {}

func NewDepthFirstSearchStrategy() *DepthFirstSearchStrategy {
	return &DepthFirstSearchStrategy{}
}

func (d *DepthFirstSearchStrategy) Search(graph map[int][]int, start int, seen []bool) []int {
	seen[start] = true
	var result []int
	result = append(result, start)

	list := graph[start]
	for _, e := range list {
		if seen[e] {
			continue
		}
		children := d.Search(graph, e, seen)
		result = append(result, children...)
	}
	return result
}

type BreadthFirstSearchStrategy struct {
	queue []int
}

func NewBreadthFirstSearchStrategy() *BreadthFirstSearchStrategy {
	return &BreadthFirstSearchStrategy{queue: make([]int, 0)}
}

func (b *BreadthFirstSearchStrategy) Search(graph map[int][]int, start int, seen []bool) []int {
	seen[start] = true
	b.queue = append(b.queue, start)

	var result []int
	for len(b.queue) != 0 {
		v := b.queue[0]
		b.queue = b.queue[1:]

		result = append(result, v)
		list := graph[v]
		for _, e := range list {
			if seen[e] {
				continue
			}
			b.queue = append(b.queue, e)
		}
	}
	return result
}
```
`DepthFirstSearchStrategy` は深さ優先探索、`BreadthFirstSearchStrategy` は幅優先探索を行う構造体になります。<br>
最後に探索アルゴリズムを選択することができる `Searcher` 構造体を実装します。
```go
type Searcher struct {
	searcherName string
	strategy SearchStrategy
}

func NewSearcher(searcherName string, strategy SearchStrategy) *Searcher {
	return &Searcher{searcherName: searcherName, strategy: strategy}
}

func (c *Searcher) Execute(graph map[int][]int, start int, seen []bool) {
	result := c.strategy.Search(graph, start, seen)
	str, _ := json.Marshal(result)
	fmt.Printf("name: %s, result: %s\n", c.searcherName, string(str))
}
```
`strategy` フィールドに使用する探索アルゴリズムに対応した `SearchStrategy` の実装クラスをセットします。また `Execute()` メソッドで探索結果を標準出力に出力する処理を実装しています。<br>
最後に `main()` の実装を以下に示します。

```go
package main

func main() {
	graph := map[int][]int {
		0: []int{1, 2},
		1: []int{3, 4},
		2: []int{5, 6},
	}

	dfsSearcher := strategy.NewSearcher("Depth First Search", strategy.NewDepthFirstSearchStrategy())
	bfsSearcher := strategy.NewSearcher("Breadth First Search", strategy.NewBreadthFirstSearchStrategy())

	dfsSearcher.Execute(graph, 0, make([]bool, 7))
	bfsSearcher.Execute(graph, 0, make([]bool, 7))
}
```
実行結果は以下となります。
```sh
name: Depth First Search, result: [0,1,3,4,2,5,6]
name: Breadth First Search, result: [0,1,2,3,4,5,6]
```

# メリット
深さ優先探索、幅優先探索に加えて新たにアルゴリズムを実装したい場合、`SearchStrategy` インターフェースを実装するようにすればそれ以外の箇所は修正を行わずに済みます。これは具体的なアルゴリズムの実装をインターフェースに委譲しているため、低結合なコード構成となっているためです。また様々なアルゴリズムを実装した `SearchStrategy` の実装クラスを複数用意していれば、プログラム実行中に使用する実装クラスを切り替えることが可能となり、状況に応じた柔軟な対応が可能となります。