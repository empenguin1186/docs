# 動機
- 最近 Effective Java の第3版の項目80「スレッドよりもエグゼキュータ、タスク、ストリームを選ぶ」を読んだので、内容をまとめました。

# 要点
- Java で非同期処理を実装する場合は、Thread クラスよりも ExecutorServce の使用を検討しましょう。

# ExecutorService とは
- ExecutorService とは、`java.util.concurrent` パッケージに含まれる非同期処理を行うインターフェースです。実際に非同期処理は以下のように実装することで可能となります。

```java
// ExecutorService 作成
ExecutorService exec = Executors.newSingleThreadExecutor();

// 非同期タスク作成
Runnable runnable = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello, World");
    }
};

// 実際に非同期処理を実行
exec.execute(runnable);
```

# ExecutorService を使用するメリット
- ここからは Thread よりも ExecutorService を使用することのメリットについて紹介します。

## 多機能である
- まず ExecutorService やその実装クラスには様々なユーティリティが備わっており、多くのことができるようになるというメリットがあります。具体例としては以下になります。
  - [submit](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/ExecutorService.html#submit-java.lang.Runnable-) メソッドで取得した [Future](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/Future.html) オブジェクトの get メソッドを呼び出すことで、特定のタスクの完了を待つことができる。
  - [invokeAll](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/ExecutorService.html#invokeAll-java.util.Collection-long-java.util.concurrent.TimeUnit-) メソッドで引数に渡された全てのタスクの完了を待つことができる(制限時間つき)。
  - [invokeAny](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/ExecutorService.html#invokeAny-java.util.Collection-) メソッドで引数に渡されたタスクのうち、例外が発生せずに正常に完了したものがあれば、その結果を返す。
  - [awaitTermination](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/ExecutorService.html#awaitTermination-long-java.util.concurrent.TimeUnit-) メソッドで ExecutorService の完了を待つことができる。
  - [ExecutorCompletionService](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/ExecutorCompletionService.html) でタスクが完了するごとにタスクを取り出して結果を取得することができる。
  - [ScheduledThreadPoolExecutor](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/ScheduledThreadPoolExecutor.html) で特定の時刻や周期的にタスクを実行するようにスケジューリングができる。

## 用途に応じたチューニングを行うことができる
- 実装するシステムの状況に応じてチューニングが行えるのも ExecutorService を使用するメリットの一つです。ExecutorService のチューニングは [Executors](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/Executors.html) の static ファクトリメソッドを使用することで可能になります。例えば、タスクを処理するスレッド数を2で固定したい場合は、以下のように実装することが可能です。

```java
ExecutorService exec = Executors.newFixedThreadPool(2);
```

- また、必要に応じてタスクを処理するスレッド数を増やしたい場合は newCachedThreadPool を呼び出します。

```java
ExecutorService exec = Executors.newCachedThreadPool();
```

## 処理の単位と実行する機構を分離できる
- Thread による非同期処理は Thread そのものが処理の単位になっており、加えて実行の機構も Thread 自身でした。これに対して ExecutorService による非同期処理は、処理の単位は [Runnable](https://docs.oracle.com/javase/jp/8/docs/api/java/lang/Runnable.html)/[Callable](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/Callable.html)、実行の機構は ExecutorService と役割が分担されています。これによって非同期処理を実行する際のポリシーに何らかの変更や変更を行う場合に、修正を柔軟に行うことが可能です。
- ちなみに Runnable は実行結果を返さず、チェック例外もスローすることができないのに対し、Callableは処理が正常に完了した場合は実行結果を、そうでなければ例外をスローするという違いがあります。

# ForkJoinPool について
- Java 7 からは `java.util.concurrent` パッケージに fork-join タスクを定義した [ForkJoinTask](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/ForkJoinTask.html) クラスと、それを実行するためのクラスである [ForkJoinPool](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/ForkJoinPool.html) クラスが追加されました。fork-join タスクとは、自身が小さなサブタスクに分割される可能性があるタスクのことを指します。簡単な実装例を以下に示します。

```java
public static void main(String[] args) throws Exception {

    // 親タスクの作成. RecursiveAction クラスは ForkJoinTask を継承している
    ForkJoinTask<?> forkJoinTask = new RecursiveAction() {
        private static final int NUM_OF_SUBTASKS = 5;

        @Override
        protected void compute() {
            
            // 子タスクの生成
            ForkJoinTask<?>[] tasks = new ForkJoinTask<?>[NUM_OF_SUBTASKS];
            for (int i = 0; i < NUM_OF_SUBTASKS; i++) {
                tasks[i] = new RecursiveAction() {
                    @Override
                    protected void compute() {
                        System.out.println("Hello World");
                    }
                };
            }

            // 全子タスクを実行し、待機する。内部では fork-join が行われている
            invokeAll(tasks);
        }
    };

    // ForkJoinPool インスタンス作成。スレッドプール数はデフォルトでは利用可能な最大プロセッサ数。
    ForkJoinPool forkJoinPool = new ForkJoinPool();

    // タスクの実行
    forkJoinPool.invoke(forkJoinTask);

    // スレッドプールの終了
    forkJoinPool.shutdown();
    forkJoinPool.awaitTermination(10, TimeUnit.SECONDS);
}
```
- ForkJoinPool は全てのスレッドが活動していることを保証するため、処理を行っていないスレッドは他のスレッドからタスクを取得するという挙動をとります。この挙動は work-stealing と呼ばれているようです。したがって全てのスレッドがタスクを実行している状態になるため、処理効率はよくなります。

# まとめ
- Effective Java の第3版の項目80「スレッドよりもエグゼキュータ、タスク、ストリームを選ぶ」についてまとめました。非同期処理を実装する際に ExecutorService を採用するメリットは以下の3つです。
  - 多機能である
  - 用途に応じたチューニングを行うことができる
  - 処理の単位と実行する機構を分離でき、実装に柔軟性を持たせることができる
- 何かご指摘などがあればしていただけると嬉しいです。

# 参考資料
- [ExecutorService (Java Platform SE 8 )](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/ExecutorService.html#awaitTermination-long-java.util.concurrent.TimeUnit-)
- [Future (Java Platform SE 8 )](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/Future.html)
- [ExecutorCompletionService (Java Platform SE 8 )](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/ExecutorCompletionService.html)
- [ScheduledThreadPoolExecutor (Java Platform SE 8 )](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/ScheduledThreadPoolExecutor.html)
- [Executors (Java Platform SE 8 )](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/Executors.html)
- [Runnable (Java Platform SE 8 )](https://docs.oracle.com/javase/jp/8/docs/api/java/lang/Runnable.html)
- [Callable (Java Platform SE 8 )](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/Callable.html)
- [ForkJoinPool (Java Platform SE 8 )](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/ForkJoinPool.html)
- [ForkJoinTask (Java Platform SE 8 )](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/ForkJoinTask.html)
- [ForkJoinPoolとForkJoinTaskの特徴と使い方 - seraphyの日記](https://seraphy.hatenablog.com/entry/20140504/p1)

