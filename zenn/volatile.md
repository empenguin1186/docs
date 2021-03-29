
# はじめに
- 最近 Effective Java の第3版の項目78「共有された可変データへのアクセスを同期する」を読んだので、内容をまとめました。

# 要約
- マルチスレッドで可変データを共有するのは極力避けましょう。必要な場合はちゃんと同期を行いましょう。

# 内容
- 以下のようなプログラムがあったとします。
```java
class StopThread {
    private static boolean stopRequested;

    public static void main(String[] args) throws InterruptedException {
        Thread backgroundThread = new Thread(() -> {
            int i = 0;
            while (!stopRequested)
                i++;
        });
        backgroundThread.start();

        TimeUnit.SECONDS.sleep(1);
        stopRequested = true;
    }
}
```
- 一見このプログラムは1秒経過したのちに終了するように見えますが、実際には終了しません。これは `backgroundThread` が `stopRequested` の値を最初にキャッシュしており、いつまでも `false` のままであるため永久にループから抜け出せないためです。解決策としては `syncronized` 修飾子を使用して `stopRequested` の値をスレッド間で同期を行うことです。

```java
class StopThread {
    private static boolean stopRequested;

    private static synchronized void requestStop() {
        stopRequested = true;
    }

    private static synchronized boolean stopRequested() {
        return stopRequested;
    }

    public static void main(String[] args) throws InterruptedException {
        Thread backgroundThread = new Thread(() -> {
            int i = 0;
            while (!stopRequested())
                i++;
        });
        backgroundThread.start();

        TimeUnit.SECONDS.sleep(1);
        requestStop();
    }
}
```
- ここでは書き込みと読み込み処理どちらも同期を行っていますが、実際でも両方の操作を同期する必要があります(書き込み中にデータを読み込むのを防ぐため。またその逆も然り)。
- ただ、このプログラムに限っていえば `volatile` 修飾子を使用することでよりシンプルに実装することも可能です。

```java
class StopThread {
    private static volatile boolean stopRequested;

    public static void main(String[] args) throws InterruptedException {
        Thread backgroundThread = new Thread(() -> {
            int i = 0;
            while (!stopRequested)
                i++;
        });
        backgroundThread.start();

        TimeUnit.SECONDS.sleep(1);
        stopRequested = true;
    }
}
```
- `volatile` はフィールド(ここでは `stopRequested`)を読み込むスレッドが、そのフィールドに最後に書き込まれた値を取得することを保証します。したがって `stopRequested` が true になることを検知できるのでこのプログラムは無限ループに陥ることなく処理が終了します。
- ただ、`volatile` はフィールドに最後に書き込まれた値を取得することを保証するだけで、排他制御を行うわけではありませんので目的によっては使わない方がいい場合もあります。以下のコードがその例です。

```java
private static volatile int NEXT_SERIAL_NUMBER = 0;

private static int generateSerialNumber() {
    return NEXT_SERIAL_NUMBER++;
}
```
- この場合一見 `generateSerialNumber()` メソッドは呼び出されるたびにユニークな値を返すよう見えますが、実際にはそうではありません。これはインクリメント演算子を使用しているのが原因です。インクリメント演算子は `NEXT_SERIAL_NUMBER` を読み出し、その後読み出した値に1を加えたものを再び `NEXT_SERIAL_NUMBER` に書き込みを行います。先述した通り、`volatile` はフィールドに最後に書き込まれた値を取得だけで排他制御は行わないので、あるスレッドが `NEXT_SERIAL_NUMBER` を読み出して書き込みを行うまでの間に別のスレッドが `generateSerialNumber()` を呼び出してしまうと、それぞれのスレッドは同じ値の  `NEXT_SERIAL_NUMBER` を取得することになってしまい、実装意図とは異なる挙動になってしまいます。
- この問題は `synchronized` 修飾子を使用するか、[java.util.concurrent.atomic](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/atomic/package-summary.html) パッケージに含まれる [AtomicLong](https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/atomic/AtomicLong.html) クラスを使用することで解決できます。`java.util.concurrent.atomic` パッケージはロックフリー、スレッドセーフ、そしてアトミックな値の操作を提供しています。したがって `synchronized` 修飾子を使用した値の同期を行うよりもパフォーマンスが優れています。実装例を以下に示します。

```java
private static final AtomicLong NEXT_SERIAL_NUMBER = new AtomicLong();
public static long generateSerialNumber() {
    return NEXT_SERIAL_NUMBER.getAndIncrement();
}
```

# 感想
- この項目を読んで初めて `volatile` 修飾子という存在を知りましたが、内容を読む限りだと使用するケースは結構稀なのかなーと感じました。ただ、非同期処理での共有データの同期処理の実装方針は参考になったので、いざ自分で実装したりチーム内でレビューするとき際にはこの項目の内容を思い出してシステム不具合を発生させないようにしたいですね。