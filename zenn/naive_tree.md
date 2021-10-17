# 経路列挙のデータを Rust で扱ってみた

# この記事について
最近 [SQLアンチパターン](https://www.oreilly.co.jp/books/9784873115894/) の2章の「ナイーブツリー(素朴な木)」を読んでいました。読んだ結果 DB の設計方法については理解できた一方で、「あれ... DB から取ってきたデータでどうやって階層構造のデータモデルを構築するんだろう...?」と気になり、Rust の勉強も兼ねて実装してみることにしました。

# 記事で紹介する内容について
この記事は SQL アンチパターンの第2章の「ナイーブツリー(素朴な木)」で紹介されている経路列挙モデルにおけるデータの参照および登録処理に加えて、取得したデータから階層構造のデータモデルを構築してJSONで出力するまでを Rust で実装したものを紹介します。経路列挙モデルの具体的な説明については SQL アンチパターンや他のネットの記事などをご覧になってください。実装内容については以下のリポジトリに push していますので、適宜確認してみてください(余計な実装なども含まれていますが、ご容赦ください)。<br>
https://github.com/empenguin1186/rust-web-app

# 実装

## DB セットアップ
始めに今回使用するデータベースを構築していきます。今回はローカルの開発環境で MySQL の Docker コンテナを起動してそこにデータを格納していくことにしました。またデータの初期化には Rust のライブラリである [Diesel](https://diesel.rs/) の CLI ツールである `diesel_cli` を使用しました。構築手順については [Diesel の Getting Started](https://diesel.rs/guides/getting-started) の内容を参考にしています。具体的な手順について知りたい方は確認してみてください。最終的に構築したデータは以下になります。

```sql
// MySQL にアクセス
$ mysql -u root -p -h 127.0.0.1 -P 3306
Enter password:
...
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| diesel_demo        |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.27 sec)

// 今回は diesel_demo というデータベースを作成。
mysql> use diesel_demo;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed

// 今回は CommentsPE というテーブルを作成。
mysql> select * from CommentsPE;
+------------+--------+--------+---------+
| comment_id | path   | author | comment |
+------------+--------+--------+---------+
|          1 | 1/     |      1 | hoge    |
|          2 | 1/2/   |      2 | fuga    |
|          3 | 1/2/3/ |      3 | piyo    |
+------------+--------+--------+---------+
3 rows in set (0.00 sec)
```
`path` が各レコードの経路情報となっており、ここでは `comment_id` が 1,2,3 のレコードがそれぞれ親、子、孫の関係となっています。

## Rust のコードから MySQL にアクセスする
データが準備できたので実際に Rust 側の実装を行っていきます。
DB アクセス周りの実装は以下のような実装としています。<br>
https://github.com/empenguin1186/rust-web-app/blob/main/src/infrastructure/repository/comments_repository_impl.rs
```rust
pub struct CommentsRepositoryImpl {
    pub connection: MysqlConnection,
}

impl CommentsRepositoryImpl {
    pub fn new() -> CommentsRepositoryImpl {
        dotenv().ok();

        let database_url = env::var("DATABASE_URL").expect("DATABASE_URL must be set");

        let connection = MysqlConnection::establish(&database_url)
            .expect(&format!("Error connecting to {}", database_url));

        CommentsRepositoryImpl { connection }
    }
}

impl CommentsRepository for CommentsRepositoryImpl {
    fn select_comments(&self, target_path: &String) -> Result<Vec<CommentPE>, Box<dyn Error>> {
        let pattern = format!("{}%", target_path);

        let result = CommentsPE
            .filter(path.like(pattern))
            .order(path.asc())
            .load::<CommentPE>(&self.connection);

        return match result {
            Ok(n) => Ok(n),
            Err(e) => Err(Box::new(e)),
        }
    }

    fn add_comments(&self, id: u64, author: &u64, comment: &str) -> Result<(), Box<dyn Error>> {
        let transaction_result = self.connection.transaction(|| {
            let new_comment = NewCommentsPE { author, comment };
            let insert_result = diesel::insert_into(CommentsPE)
                .values(&new_comment)
                .execute(&self.connection);

            if let Err(_) = insert_result {
                return Err(DieselError::RollbackTransaction);
            }

            let update_result = sql_query(
                "
                UPDATE CommentsPE
                  SET path =
                    (SELECT x.path FROM (
                      SELECT path FROM CommentsPE WHERE comment_id = ?
                    ) AS x) || LAST_INSERT_ID() || '/'
                WHERE comment_id = LAST_INSERT_ID();    
                ",
            )
            .bind::<Unsigned<BigInt>, _>(id)
            .execute(&self.connection);

            return match update_result {
                Ok(_) => Ok(()),
                Err(_) => Err(DieselError::RollbackTransaction),
            }
        });

        return match transaction_result {
            Ok(_) => Ok(()),
            Err(e) => Err(Box::new(e)),
        }
    }
}
```
`select_comments` は引数で渡した `path` 配下に存在するデータを取得するメソッドとなっています。後々の処理を考え `path` で昇順にソートして取得しています。`add_comments` に関しては引数で渡した `id` と一致するデータの子要素にデータを追加するメソッドとなっています。`INSERT` と `UPDATE` を実行していますが、`UPDATE` が失敗した場合にロールバックしたほうがいいと考えたため、トランザクションを張る処理を追加しています(参考: [Connection in diesel::connection - Rust](https://docs.diesel.rs/diesel/connection/trait.Connection.html))。<br>両方のメソッドとも基本的に SQL アンチパターンで紹介されているクエリをそのまま使用しています。

## MySQL から取得したデータを使用して階層構造の構造体を生成する
データアクセスの処理を実装したので、続いて取得したデータから実際に階層構造のデータモデルを生成する処理を実装します。実装内容については以下となります。<br>
https://github.com/empenguin1186/rust-web-app/blob/main/src/domain/model/tree.rs
```rust
/// コメント内容とそのコメントの投稿者の情報を含む構造体
#[derive(Serialize, Deserialize, Debug, PartialEq)]
pub struct Item {
    comment: String,
    author: u64,
}

/// 階層構造のデータを定義する列挙型
#[derive(Serialize, Deserialize, Debug, PartialEq)]
#[serde(untagged)]
pub enum Tree {
    /// 階層構造のデータにおける末端の要素
    Leaf { item: Item },
    /// 階層構造のデータにおける子要素を持つ要素
    Branch { item: Item, children: Vec<Tree> },
}

impl Tree {
    /// 与えられたデータに対応した Tree を生成する
    /// # Arguments
    /// * `comments` - `CommentsPE` テーブルから取得したレコード群
    pub fn new(comments: &Vec<CommentPE>) -> Tree {
        let mut index = 0 as usize;
        let mut depth = comments.get(index).unwrap().path.as_ref().unwrap().len();
        Tree::create_tree(&mut depth, &mut index, comments)
    }

    /// Branch に子要素を追加する
    /// # Arguments
    /// * `tree` - Branch に追加する子要素
    fn add_child(&mut self, tree: Tree) {
        if let Tree::Branch { item: _, children } = self {
            children.push(tree);
        }
    }

    /// Tree を生成する
    /// # Arguments
    /// * `depth` - 現在注目している要素の深さ(pathの文字数で表現)
    /// * `index` - 現在注目している要素の comments における index 番号
    /// * `comments` - `CommentsPE` テーブルから取得したレコード群
    fn create_tree(depth: &mut usize, index: &mut usize, comments: &Vec<CommentPE>) -> Tree {
        // レコードが1つしかないもしくは最後の要素の場合、すぐに Leaf を返す
        if comments.len() == 1 || *index == comments.len() - 1 {
            let author = comments.get(*index).unwrap().author;
            let comment = &comments.get(*index).unwrap().comment;
            *depth = 2 as usize;
            return Tree::Leaf {
                item: Item {
                    comment: comment.to_string(),
                    author,
                },
            };
        }

        let author = comments.get(*index).unwrap().author;
        let comment = &comments.get(*index).unwrap().comment;
        let cur_depth = comments.get(*index).unwrap().path.as_ref().unwrap().len();
        return if cur_depth
            < comments
            .get(*index + 1)
            .unwrap()
            .path
            .as_ref()
            .unwrap()
            .len()
        {
            *index = *index + 1;
            *depth = comments.get(*index).unwrap().path.as_ref().unwrap().len();

            // この時点で子要素が存在するので Branch を作成する
            let mut branch = Tree::Branch {
                item: Item {
                    comment: comment.to_string(),
                    author,
                },
                children: vec![],
            };

            // 子要素が存在する限り探索を続ける
            if let Tree::Branch { .. } = branch {
                while *depth > cur_depth {
                    branch.add_child(Tree::create_tree(depth, index, comments));
                }
            }
            branch
        } else {
            // この時点で末端の要素だと判明しているので Leaf を返す
            let leaf = Tree::Leaf {
                item: Item {
                    comment: comment.to_string(),
                    author,
                },
            };
            *index = *index + 1;
            *depth = comments.get(*index).unwrap().path.as_ref().unwrap().len();
            leaf
        }
    }
}
```
列挙型である Tree には子要素を持つ `Branch`、末端の要素を表す `Leaf` の2つの列挙子が定義されています。`create_tree` メソッドで各レコード間の親子関係を判定し、階層構造のデータを構築しています。引数で渡されている `comments` が実際にデータベースから取得したレコード群を表すデータとなっており、`create_tree` メソッドの処理については `comments` が `path` フィールドで昇順にソートされていることを前提とした内容となっているので、先ほどの `CommentsRepositoryImpl` の `select_comments` メソッドではそのような実装を追加しているという経緯になります。一応テストも実装しており、成功することを確認しています。何かロジックに不備がありましたらご指摘いただけると嬉しいです。

## 検証

実際に取得したデータから階層構造のデータを構築して JSON で出力するまでを実装してみます。src/main.rs を以下のように実装します。
```rust
fn main() {
    let comments_repository = CommentsRepositoryImpl::new();
    let path = String::from("1/");
    let result = comments_repository.select_comments(&path);
    let tree = Tree::new(&result.unwrap());
    let json = serde_json::to_string(&tree).unwrap();
    println!("{}", json);
}
```

`cargo` コマンドの実行結果は以下のようになり、上手くデータが階層構造となっていることが確認できます。
```sh
$ cargo run | jq .
...
{
  "item": {
    "comment": "hoge",
    "author": 1
  },
  "children": [
    {
      "item": {
        "comment": "fuga",
        "author": 2
      },
      "children": [
        {
          "item": {
            "comment": "piyo",
            "author": 3
          }
        }
      ]
    }
  ]
}
```

続いてデータの登録を行っていきます。こちらは単に CommentsRepositoryImpl のメソッドを呼び出すだけです。
```rust
fn main() {
    let comments_repository = CommentsRepositoryImpl::new();
    let author = 4;
    comments_repository.add_comments(1, &author, "hogehoge");
}
```
`CommentsPE` テーブルを確認すると、以下のように新しくデータが追加されていることが確認できます。

```sql
mysql> select * from CommentsPE;
+------------+--------+--------+----------+
| comment_id | path   | author | comment  |
+------------+--------+--------+----------+
|          1 | 1/     |      1 | hoge     |
|          2 | 1/2/   |      2 | fuga     |
|          3 | 1/2/3/ |      3 | piyo     |
|          4 | 1/4/   |      4 | hogehoge |
+------------+--------+--------+----------+
4 rows in set (0.29 sec)
```