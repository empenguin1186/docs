
# やったこと

## モックのAPIを作成

* ただレスポンスをモックにしただけで、特別な処理は行なっていないので割愛

## Mybatis でSQLのデータを扱えるように

### 参考しにしたサイト
 https://github.com/mybatis/spring-boot-starter/wiki/Quick-Start
 https://qiita.com/kazuki43zoo/items/ea79e206d7c2e990e478#mybatistestmybatis-spring-boot-starter-test%E3%81%AE%E5%88%A9%E7%94%A8

 * 使用したデータベースは今回はh2
 * 開発中に以下のエラーが発生した。

 ```
 org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'profileController': Unsatisfied dependency expressed through field 'userMapper'; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'userMapper' defined in file
    ...
Caused by: org.apache.ibatis.builder.BuilderException: Parsing error was found in mapping #{comment), #{place}.  Check syntax #{property|(expression), var1=value1, var2=value2, ...} 
	...
Caused by: java.lang.StringIndexOutOfBoundsException: String index out of range: 18
	...
 ```

* 結論としてはクエリの記法に間違いがあり、そのクエリを定義しているMapperクラスがBeanに登録できないということであった。したがってクエリを直せばこのエラーは発生しなくなった。教訓としてはMapperクラスがBeanに登録されない場合はクエリの記法が間違っていないかどうかを確認する。

## どんな機能を持たせるか

### ユーザ関連
- データ（これは現状のTwitterの機能から決めました。）
    - id
    - ユーザ名
    - 紹介文
    - 場所
    - 生年月日
    - プロフィール画像(URL)
    - ※フォロー数とフォロワー数はフォローフォロワーテーブルを外部参照して算出する。
- APIの機能
    - × Create
    - × Read（id をキーとして）
    - × Update
    - × Delete
###  ツイート関連
- データ
    - ツイートid 
    - ツイート内容
    - ツイートしたユーザ
    - 添付画像のURL
    - いいね数
    - リツイート数
    - 返信先ツイートid
    - ツイート時刻
- APIの機能
    - × Read(1つのツイート) 
    - × Read(フォローしているユーザのツイート)
    - × Read(キーワード検索の結果) (優先度低め)
    - × Update(いいね)
    - × Update(リツイート)
    - × Create
    - × Delete(ツイート)
### フォローフォロワー関連
- データ
    - フォローユーザid
    - フォロワーid
    - ブロックフラグ(優先度低め)
    - ミュートフラグ(優先度低め)
- APIの機能
    - × CREATE(フォロー追加)
    - × UPDATE(ブロック＆ミュートフラグ更新)
    - × DELETE(フォロー解除)
    - × SELECT(フォロー数取得)
    - × SELECT(フォロワー数取得)

### 反省点＆学んだこと
- 単純なケアレスミスで時間を取られることがあった(SELECTメソッドのアノテーションを@Insertとしていたり、単純にクエリの記法が間違っていたり)ので、今後はエラーにハマった時は最初にケアレスミスの可能性を検討する
- 複数のフォローユーザのツイートを取得するにはどうしたらいいかを検討したが、WHERE句でANYを使用することで取得することができた。単一の値で絞り込む時は'='を使用し、複数の値で絞り込む時はANYやALLを使用することを学んだ
- キーワード検索で部分一致の条件をmybatisでどう記述するかがわからなかったが、無理やりCONCATを使用して解決した

```
/* キーワード検索 */
@Select("SELECT tweet_id AS tweetId, content AS content," +
	" tweet_user AS tweetUser, attachment AS attachment," +
	" favorite AS favorite, retweet AS retweet," +
	" reply_to AS replyTo, time AS time FROM tweet" +
	" WHERE content LIKE CONCAT('%', #{keyword}, '%')")
List<Tweet> findTweetsWithKeyword(String keyword);
```

## エンドポイントの設定
- とりあえず適当に実装して後でまとめて整形する

## フロント部分の実装

### つまづいた点
* cssやjsファイルを読み込むときはresources/static以降のパスを記述しなければならない。さらに/始まりで記述する必要がある。

```html:例
<link rel="stylesheet" href="/css/bootstrap.min.css" th:href="@{/css/bootstrap.min.css}">
<script src="/js/bootstrap.bundle.min.js" th:href="@{/js/bootstrap.bundle.min.js}"></script>
```

* application.yml で設定した値をJavaのクラスで扱えるようにするには、設定値を使用したいクラスにSetter、Getterメソッドを追加しておき、static修飾子はつけてはいけない。

<設定ファイル>

```application.yml
path:
  key1: value1
  key2: value2
```

<設定値を使用するクラス>

```Sample.java
@Getter
@Setter
@ConfigurationProperties(prefix = "path")
public class Sample {

    private var key1; <- value1の値がセットされている

    private var key2; <- value2の値がセットされている

    ....
}
```

@Value で設定する場合は以下の通り

```
public class Sample {

    @Value("${path.key1}")
    private var key1;

    @Value("${path.key2}")
    private var key2;

    ....
}
```

参考サイト：http://tk5-21.hatenablog.com/entry/2018/06/18/000000

* 認証
認証処理を追加するにはまずアプリケーションで認証処理を行うという設定クラスを定義する必要がある。
設定ファイルを定義したい場合は@Configurationアノテーションを付与することにより設定ファイルとして認識される。
1. WebSecurityConfigurerAdapterを継承しConfigクラスを定義する

```
@Configuration
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
```

2. configure(WebSecurity web) で認証処理を行わないリソースを設定することができる

```
    @Override
    public void configure(WebSecurity web) {
        web.ignoring()
                .antMatchers("/resources/**");
    }  
```

3. configure(AuthenticationManagerBuilder auth) で独自の認証処理を実装する。その場合、独自の処理は、UserDetailServiceを実装したクラスのloadUserByUsernameメソッドに記述する。

<Configクラス>
```
    @Autowired
    private UserDetailsService userDetailsService;

    @Autowired
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService);
    }
```

<UserDetailServiceを実装したクラス>
今回はAPIから取得した情報と入力された情報を検証し認証・認可を行う処理を記述している。
以前自分はこのloadUserByUsernameの返り値が認証処理に使われずにただ認証後のユーザ情報を返しているものだと思っていたが、実際にはこの返り値と入力された情報を突合して認証処理を行なっているようである。ここで結構な時間を持って行かれた。

```
public class AuthenticationRealm implements UserDetailsService {
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {

        ...

        try {
            UserCredentials result = restTemplate.getForObject(uri + "user/" + username + "/credentials", UserCredentials.class);
            return User.builder()
                    .username(username)
                    .password(result.getEncodedPassword())
                    .roles(result.getRoles().toArray(new String[0]))
                    .authorities(AuthorityUtils.createAuthorityList(result.getAithorities().toArray(new String[0])))
                    .build();

        } catch (Exception e){
            throw new UsernameNotFoundException("not found : " + username);
        }
    }
}
```

4. void configure(HttpSecurity http) で具体的な認証処理を定義

```
    @Override
    protected void configure(HttpSecurity http) throws Exception {

        String[] permittedUrls = {LOGIN_PATH, CERT_PROCCESSING_PATH};
        http.authorizeRequests()
                .antMatchers(permittedUrls).permitAll()
                .antMatchers("/timeline").hasAnyAuthority(ROLE_USER);

        http.formLogin()
                .loginProcessingUrl(CERT_PROCCESSING_PATH) // 認証処理を行うURIを記述
                .loginPage(LOGIN_PATH) // loginページのhtmlファイル名を指定
                .permitAll()
                .failureUrl(LOGIN_PATH + "?error=notPermitted") // 認証処理が失敗した時のURI
                .defaultSuccessUrl(DEFAULT_SUCCESS_PATH, false) // 認証処理が失敗した時のURI
                .usernameParameter("username")
                .passwordParameter("password").permitAll()
                .and()
                .csrf();
    }
```

* cssが読み込まれなくなった時は、Controllerクラスでちゃんとエンドポイントの設定がされているかどうかを確認する
* Formクラスを実装する際に入力が空になっていないかのチェックを行う際は、Stringクラスには@NotEmpty, Integer(int)クラスには@NotNullを指定する。
* emailを入力チェックを行いたい場合は@Emailでチェック可能
* 参考文献1：https://casual-tech-note.hatenablog.com/entry/2018/10/10/224250
* 参考文献2：https://qiita.com/yu_eguchi/items/5a1b2ae46ff803b020bf

### Issues
- [x] 修正後のUserクラス = 現状のUser(profile)クラス + 自分のツイート一覧(List<Tweet>)にする
- [ ] 正常系と異常系のレスポンスは統一する
- [ ] TweetPost というレスポンスクラスの名前はよろしくない
- [ ] Controllerクラスのメソッド名はHTTPメソッド準拠、MapperクラスのメソッドはSQLの操作準拠
- [ ] RestfulなAPIとなっているか確認
- [ ] Unit Test 作成
- [ ] IntelliJでエラーとなっている、MapperクラスをAutowiredする箇所でstaticにするとエラーは消えるがAPIが正常に機能しない
- [ ] 複数キーワードで検索できるようにする(https://qiita.com/suin/items/2b274ac208eb94ed7c7d)

- [ ] 認証・認可
- [ ] レスポンシブなデザイン（現状はスマートフォン向けのUIしか対応していない）
- [ ] ページネーション
- [ ] テスト開発
- [ ] アカウント情報編集処理
- [ ] タイムラインはSPAにするべき？
- [ ] materialize使ってみる：https://materializecss.com/
- [ ] アカウント作成処理
- [ ] アカウント作成時にメールアドレス宛にメッセージ送信
- [ ] 例外処理を追加


adelypenguin3193@gmail.com
jedi@334force

