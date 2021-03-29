
# はじめに
- Java におけるネストしたクラスの種類とその使用方法について学んだので内容をまとめます。

# ネストしたクラス
- Java におけるネストしたクラスの種類は、以下の4つが挙げられます。それぞれについて実装を交えながら説明していきたいと思います。
  - static のメンバークラス
  - 非staticのメンバークラス
  - 無名クラス
  - ローカルクラス

## static のメンバークラス
- まずは static のメンバークラスについて説明します。static のメンバークラスでは外部クラス(以後エンクロージングクラスと呼ぶことにします)の static フィールドやメソッドを使用することが可能です。一方エンクロージングクラス側はメンバークラスの private フィールドにアクセスすることが可能です。以下に実装例を示します。

```java:static のメンバークラス
class Main {
    private static final String STATIC_ENCLOSING_FIELD = "hoge";

    public static class MemberClass {
        private final String nestedField;

        public MemberClass() {
            this.nestedField = STATIC_ENCLOSING_FIELD;
        }
    }

    // エンクロージングクラスの static メソッド
    public static void main(String[] args) {
        final Main.MemberClass memberClass = new Main.MemberClass();
        // nestedField は private フィールドだが、エンクロージングクラスからは参照可能
        System.out.println(memberClass.nestedField);
    }

    // エンクロージングクラスのインスタンスメソッド
    public String getNestedField() {
        final Main.MemberClass memberClass = new Main.MemberClass();
        return memberClass.nestedField;
    }
}
```
- static のメンバークラスは、エンクロージングクラスとセットで有用な処理を実装することができる場合に使用されます。

## 非 static のメンバークラス
- 次に非 static のメンバークラスについて説明します。非 static のメンバークラスのインスタンスは、エンクロージングインスタンスから作成することが可能です。以下に実装例を示します。

```java:非staticのメンバークラス
class Main {
    private String enclosingField;

    public Main(String enclosingField) {
        this.enclosingField = enclosingField;
    }

    public class MemberClass {
        private String innerField;

        public MemberClass() {
            this.innerField = Main.this.enclosingField;
        }

        public void setEnclosingField(String enclosingField) {
            // <エンクロージングクラス>.this でエンクロージングインスタンスのフィールドやメソッドを呼び出せる
            Main.this.enclosingField = enclosingField;
        }
    }

    public static void main(String[] args) {
        final Main main = new Main("hoge");
        final Main.MemberClass memberClass = main.new MemberClass();
        memberClass.setEnclosingField("fuga");
        System.out.println(main.enclosingField);
    }
}
```
- 上記の実装から見てとれるように、非 static のメンバークラスのインスタンスはエンクロージングインスタンスとの関連を持っています。したがってエンクロージングインスタンスの値を非 static のメンバークラスのインスタンスから操作したい場合などには有効な実装となります。しかし、エンクロージングインスタンスとの関連をもつことはデメリットもあります。関連をもつことでその分メモリを消費しますし、非 static のメンバークラスのインスタンスを適切に管理していないといつまでもエンクロージングインスタンスへの関連が残ったままとなり、 GC の対象とならずメモリリークの原因となる可能性があります。したがって必要がなければ static のメンバークラスで実装することが望ましいでしょう。

## 無名クラス
- 次に無名クラスについて説明します。無名クラスとはその名の通りクラス名を持たず、宣言と同時にインスタンスを生成するような実装のことを指します。実装例は以下です。

```java:無名クラス
class Main {
    public interface NoNameClassBase {
        String getParam();
    }

    public static void main(String[] args) {
        final NoNameClassBase noNameClassBase = new NoNameClassBase() {
            @Override
            public String getParam() {
                return "hoge";
            }
        };
        System.out.println(noNameClassBase.getParam());
    }
}
```
- 無名クラスには以下のような制限があります。
  - 宣言した箇所以外でその無名クラスを使用できない
  - `instance of` などのクラス名を用いた処理を実装できない
  - 複数のインターフェースを実装することができない
  - スーパータイプから継承しているメソッド以外のメソッドを使用することができない
- 上記のような制約や、ラムダ式の存在もあることから、無名クラスは普段は使用することはないように感じます。それでも無名クラスを使用する場合は、可読性を考慮してなるべく少ないコード量で実装することが望ましいとされています。

## ローカルクラス
-　最後にローカルクラスに関して説明します。ローカルクラスとは、メソッド内で変数を宣言する時と同じ要領で宣言されたクラスのことを指します。実装例を以下に示します。

```java:ローカルクラス
class Main {
    public static void main(String[] args) {
        class LocalClass {
            private final String field;

            public LocalClass(String field) {
                this.field = field;
            }
        }

        final LocalClass localInstance = new LocalClass("hoge");
        System.out.println(localInstance.field);
    }
}
```
- 無名クラスもメソッド内で宣言することが可能でしたが、ローカルクラスは無名クラスと異なり、宣言してしまえば何度もインスタンスを生成することが可能です。また、無名クラスと同様に非 static の文脈ではエンクロージングインスタンスへアクセスすることが可能です。ただこれも無名クラスと同様に、可読性を考慮してなるべく少ないコード量で実装することが望ましいとされています。

# まとめ
- Java におけるネストしたクラスについて学びました。static のメンバークラスを使用すべきか、それとも非 static のメンバークラスを使用すべきかを悩んだ場合は、エンクロージングインスタンスへの参照が必要かどうかを一度考えてみると良いかと思います。また、無名クラスを使用すべきか、それともローカルクラスを使用すべきかを悩んだ場合には宣言したクラスを複数回使用するかどうかを一度考えてみると良いかと思います。