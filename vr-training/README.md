# VR iOS アプリ作成

<!-- TOC -->

- [VR iOS アプリ作成](#vr-ios-アプリ作成)
    - [参考書](#参考書)
    - [章ごとのまとめ](#章ごとのまとめ)
        - [3章](#3章)
            - [用語](#用語)
            - [オブジェクトの操作モード](#オブジェクトの操作モード)
            - [GameObject と Component](#gameobject-と-component)
            - [カメラの投影方法](#カメラの投影方法)
        - [4章](#4章)
        - [5章](#5章)
            - [Asset ストアに関して](#asset-ストアに関して)
            - [一人称視点を実現するには](#一人称視点を実現するには)
            - [スクリプトについて](#スクリプトについて)
        - [6章](#6章)
            - [プレハブについて](#プレハブについて)
            - [Shooter スクリプト](#shooter-スクリプト)
            - [物理エンジンを用いた弾の発射](#物理エンジンを用いた弾の発射)
            - [不要なオブジェクトの削除](#不要なオブジェクトの削除)
            - [敵を倒せるようにする](#敵を倒せるようにする)
            - [衝突時の処理を実装する](#衝突時の処理を実装する)
            - [敵をランダムに出現させる](#敵をランダムに出現させる)
    - [遭遇したエラー](#遭遇したエラー)
        - [This iPhone ~~ , which may not be supported by this version of Xcode.](#this-iphone---which-may-not-be-supported-by-this-version-of-xcode)
        - [Development cannot be enabled while your device is locked.](#development-cannot-be-enabled-while-your-device-is-locked)
        - [Preparing debugger support for iPhone](#preparing-debugger-support-for-iphone)
    - [Tips](#tips)
        - [任意のオブジェクトにフォーカスしたい](#任意のオブジェクトにフォーカスしたい)
        - [](#)
    - [参考文献](#参考文献)

<!-- /TOC -->

## 参考書
作って学べる　Unity VR アプリ開発入門

## 章ごとのまとめ

### 3章

#### 用語

|  用語  |  意味  |
|:--:|:---|
|  Scene  |  UnityのUI上に映し出されるゲーム画面。    |
|  Asset  |  キャラクターオブジェクトやテクスチャなどゲームで使用する素材。  |
|  meta ファイル  |  プロジェクトの Asset フォルダに存在するファイルやフォルダに対して生成されるファイル。Asset を識別するためのIDや、エディタで使用する設定を格納している。したがってAsset フォルダ内のファイルやフォルダを移動させた場合にはこの meta ファイルも移動させなければ、ファイルやフォルダは初期化されてしまう。  |

#### オブジェクトの操作モード

オブジェクトの操作モードを切り替えるには以下のショートカットを使用する。  

|  操作モード  |  ショートカット  |
|:--:|:---|
|  移動  |  W  |
|  回転  |  E  |
|  拡大・縮小  |  R  |
|  全操作モード  |  Y  |

#### GameObject と Component
GameObject とは文字通りゲームで使用するキャラクターなどの素材。その素材にどういった特性(位置、物理的な制約)を持たせるのかを定義したのが Component である。代表的な Component は以下の通り

|  コンポーネント  |  説明  |
|:--:|:---|
|  Transform  |  位置、大きさ  |
|  Rigidbody  |  物理的な性質を付与する(重力 etc...)  |
|  Collider  |  値判定を表現する  |
|  MeshFilter  |  見た目を定義する  |
|  Renderer  |  ものを描画する(消したりする？)  |
|  Camera  |  カメラ  |
|  Light  |  照明  |
|  AudioListener  |  音声を拾う。一つのシーンにつき一つ  |
|  AudioSource  |  音源を表す  |

#### カメラの投影方法
カメラの投影方法は２種類存在し、透視投影は、遠近法的な見え方をし、平行投影は同じ大きさのオブジェクトが遠近関係なく同じ大きさに見える。

### 4章
issue と 「遭遇したエラー」の項を参照。

### 5章

#### Asset ストアに関して
Unity ではフリー素材を使用して開発をすることが可能である。この場合、Asset ストアから素材をダウンロードする。Asset ストアは UI からアクセスすることができる。

#### 一人称視点を実現するには
Main カメラオブジェクトの子要素にキャラクター(今回は銃を持ったキャラクター)を配置することで実現できる。

#### スクリプトについて

|  関数名  |  説明  |
|:--:|:---|
|  Awake  |  オブジェクトが生成された時に一度だけ呼ばれる。  |
|  Start  |  Awake が呼ばれるタイミングの後に呼ばれる。有効なオブジェクトの最初のフレームで一度だけ呼ばれる。  |
|  Update  |  フレームごとに呼び出される。  |
|  FixedUpdate  |  Updateよりも頻繁に正確なタイマーで呼び出される。物理演算の時などに用いられる。  |
|  OnDestroy  |  コンポーネントが破棄される時に呼ばれる。  |

### 6章

#### プレハブについて
プレハブとは、オブジェクトのテンプレートのことであり、シューティングゲームにおける弾のような同じような特性を持ったオブジェクトを何度も生成する時に役立つ。オブジェクトをプレハブ化するには、シーンに存在するオブジェクトを　Asset/<Project名>/Prefabs/ 配下にドラッグ&ドロップする。アイコンが青くなればプレハブとなっている証拠である。

#### Shooter スクリプト

```cs
public class Shooter : MonoBehaviour
{
    [SerializeField] GameObject bulletPrefab; // 弾のプレハブ
    [SerializeField] Transform gunBarrelEnd; // 弾の銃口 (発射位置)

    void Update()
    {
        // 入力に応じて弾を発射する
        if (Input.GetButtonDown("Fire1"))
        {
            Shoot();
        }
    }

    void Shoot()
    {
        /* プレハブを元に、シーン上に弾を生成
        gunBarrelEnd.position: 銃の位置
        gunBarrelEnd.rotation: 銃の向き
        */

        Instantiate(bulletPrefab, gunBarrelEnd.position, gunBarrelEnd.rotation);

    }
    ...
```
```cs
[SerializeField] GameObject bulletPrefab; // 弾のプレハブ
[SerializeField] Transform gunBarrelEnd; // 弾の銃口 (発射位置)
```
`[SerializeField]` を付与すると、UIで設定できるようになる。  
`Input.GetButtonDown("Fire1")` でボタンが入力されたかどうかを検証する。この場合、マウスの右クリックや、右のCtrl キーのが入力される時、if の分岐に入る。  
```
Instantiate(bulletPrefab, gunBarrelEnd.position, gunBarrelEnd.rotation);
```
`bulletPrefab` を　`gunBarrelEnd.position` の位置に `gunBarrelEnd.rotation` の方向を向いた状態で生成する。  
しかし、これだけだと弾が生成されるだけで発射されない。

#### 物理エンジンを用いた弾の発射

弾を発射したい場合は、 bullet オブジェクトに `Rigidbody`属性に加えて以下のスクリプトを付与する。
```cs
[RequireComponent(typeof(Rigidbody))]
public class Bullet : MonoBehaviour
{
    [SerializeField] float speed = 20f; // 弾速 [m/s]
    [SerializeField] ParticleSystem hitParticlePrefab; // 着弾時演出プレハブ

    // Start is called before the first frame update
    void Start()
    {
        // ゲームオブジェクト前方向の速度ベクトルを計算
        var velocity = speed * transform.forward;

        // Rigidbody コンポーネントを取得
        var rigidbody = GetComponent<Rigidbody>();

        /*
        Rigidbody コンポーネントを使って初速を与える
        ForceMode.VelocityChange 指定した速度変化に相当する力 (ここでは velocity)
        */

        rigidbody.AddForce(velocity, ForceMode.VelocityChange);
    }
```

```cs
[RequireComponent(typeof(Rigidbody))]
```
`RequireComponent` はオブジェクトに Component が付与されていることを要求する。付与されていない場合は自動的に付与される。

#### 不要なオブジェクトの削除

不要なオブジェクトを削除したい場合は、プレハブに以下のスクリプトを付与する。
```cs
public class AutoDestroy : MonoBehaviour
{
    [SerializeField] float lifetime = 5f; // ゲームオブジェクトの寿命

    // Start is called before the first frame update
    void Start()
    {
        // lifetime 秒後に gameObject が削除される
        Destroy(gameObject, lifetime);
    }
```

#### 敵を倒せるようにする
敵を倒せるようにするには、まず当たり判定をつける必要がある。当たり判定は、オブジェクトに Collider と呼ばれるコンポーネントを付与することにより実現できる。今回は Capsule  Collider というコンポーネントを付与し、球状の当たり判定をオブジェクトに付与した。この際、ゲームオブジェクトに付与したコンポーネントはプレハブに反映することが可能である。

#### 衝突時の処理を実装する
衝突は Collider コンポーネントが付与されたオブジェクト間で行われる。また、それに加えてオブジェクトはそれぞれどのレイヤーに属しているかはあらかじめ定義されているが、決まったレイヤー間でしか衝突は発生しない。衝突を発生させるレイヤー間は設定より変更可能。  
衝突には Collision と Trigger の２種類のイベントが存在し、 Collision イベントは実際にオブジェクトが他のオブジェクトと衝突した時に発生するイベントで、 Trigger イベントはあるオブジェクトが Collider で定義された領域に侵入した時に発生するイベントである。  

例) 定義されたレイヤー間で実装クラスのオブジェクトがトリガー領域侵入時に呼び出されるメソッド
```cs
void OnTriggerEnter(Collider other)
{
    ...
}
```

#### 敵をランダムに出現させる
まずは敵を出現させる Spawner クラスを作成する
```cs
public class EnemySpawner : MonoBehaviour
{
    [SerializeField] Enemy enemyPrefab; // 出現させる敵のプレハブ

    Enemy enemy; // 出現中の敵を保持

    public void Spawn()
    {
        // 出現中でなければ敵を出現させる
        if (enemy == null)
        {
            enemy = Instantiate(enemyPrefab, transform.position, transform.rotation)
        }
    }
}
```
その後空のオブジェクトの`EnemySpawner`を生成し、`EnemyPrefab` フィールドにプレハブ化したオブジェクトを設定する。  
複数の`EnemySpawner`オブジェクトをコントロールする `SpawnController` オブジェクトを作成する。
```cs
public class SpawnController : MonoBehaviour
{
    [SerializeField] float spawnInterval = 3f;

    EnemySpawner[] spawners;
    float timer = 0f;

    void Start()
    {
        // 子オブジェクトに存在するEnemySpawnerのリストを取得
        spawners = GetComponentsInChildren<EnemySpawner>();
    }

    void Update()
    {
        timer += Time.deltaTime

        if (spawnInterval < timer)
        {
            // ランダムに Spawner を選択
            var index = Random.Range(0, spawners.Length);
            spawners[index].Spawn();

            // タイマーリセット
            timer = 0f;
        }
    }
}
```
このスクリプトを空のオブジェクトに付与し、子要素に `EnemySpawner`を追加する。これで敵がランダムに生成される。

## 遭遇したエラー

### This iPhone ~~ , which may not be supported by this version of Xcode.
- https://qiita.com/1901drama/items/838831c13da5f59a9792

### Development cannot be enabled while your device is locked.
- https://qiita.com/ukandori/items/ea2554d18ae480d43741  

### Preparing debugger support for iPhone
- https://www.crossroad-tech.com/entry/Xcode_deploypending 

## Tips 

### 任意のオブジェクトにフォーカスしたい
ヒエラルキーから対象のオブジェクトを選択し、ダブルクリックすることで一気にそのオブジェクトを確認できる位置まで視点が移動する。

### 

## 参考文献

ショートカットとか  
http://tsubakit1.hateblo.jp/entry/2015/04/21/031048

