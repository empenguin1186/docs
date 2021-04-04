# はじめに
- Kotlin で Nester JWT を生成する処理を実装したので紹介します。

# Nested JWT について
- Nested JWT についてご存じない方もいると思いますので、まずは Nested JWT について基本となる JWT から順を追って説明しようと思います。

## JWT について
- JWT とは **JSON Web Token** の略で、[RFC 7519](https://tools.ietf.org/html/rfc7519) で策定された属性情報(Cliam)を JSON 形式のデータで表現するトークンの仕様のことを指します。特徴として署名/暗号化を行うことができることが挙げられます。署名/暗号化を行う際にデータを変換するフォーマットがそれぞれ決まっており、署名の場合は [JWS (JSON Web Signature)](https://tools.ietf.org/html/rfc7515)、暗号化の場合は [JWE (JSON Web Encryption)](https://tools.ietf.org/html/rfc7516)と呼ばれています。

## JWS
- まずは JWS 形式の JWT について説明します。 JWS 形式の JWT は `${ヘッダ}.${ペイロード}.${署名}`というデータ形式となっています。例を以下に示します。
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IlBlbmd1aW4iLCJpYXQiOjE1MTYyMzkwMjJ9.PPU-YdL9ZxaGX3hHyaT5gi0Ljbpc_HZZRtaCXGXbcjo
```
- それぞれのデータは Base64 URL エンコードされているので、復号するとヘッダとペイロードにどんな値が設定されているのかを確認することができます。復号されたデータを以下に示します。

|  パート  |  復号前の値  |  復号後の値  |
|:---|:---|:---|
|  ヘッダ  |  eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9  |  {"alg": "HS256","typ": "JWT"}  |
|  ペイロード  |  eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IlBlbmd1aW4iLCJpYXQiOjE1MTYyMzkwMjJ9  |  {"sub": "1234567890","name": "Penguin","iat": 1516239022}  |

- また、署名パートに関してはヘッダとペイロードを指定されたアルゴリズム(今回は HMAC SHA256)で署名し、JWS の仕様に基づいて Base64 URL エンコードされたものが設定されています。詳しくは [RFC 7515 - JSON Web Signature (JWS)](https://tools.ietf.org/html/rfc7515#page-8) の項を見てみてください。

## JWE
- 次に JWE 形式の JWT について説明します。 JWE 形式の JWT は `${ヘッダ}.${キー}.${初期ベクター}.${暗号文}.{認証タグ}`というデータ形式となっており、実際に暗号化されたデータは4番目のパートに設定されます。例を以下に示します。
```
eyJlbmMiOiJBMjU2R0NNIiwiYWxnIjoiZGlyIn0.
.
Bul7NMOCe0UJrf0D.ugF1xrptF_slq2-shDa_19XQS97a3lzeCXVqlbYS7j2T_w4Eiz_JU9_X8uRQjyYOduGL2wtlHf5vllLaIj1_92nInCNmbGzat4cKMJW2NZilZIfA9VfZlUUi8ETgKYX5rmDsRs2HqQPI-N23z1OV2uSE4VziZVCI7R898jhYOMPNzQS4sBPeQVFI2KKjriBsvDBOGY2D8ubflTW_8xvSvokYNnkBpmzbX17Y8E7TyNuvEW7igIxwXY08MYtA3ZQdCVsQQN6O05ZXWRnHvyRTVdCRe3eu6wlaTjZIqINfCSH0AHuEkfe0EW6gLsVqKiSagBJS0CQaAbQ9tntWR80rtu3ulTxx3agUFEyDPPEJ7xJWPZ6tVgDRBEuT5xEMYfvnXXk28-Z-kkIMG2ejXSUzCjvRhi4O1M2nbpRoGowLzTixi27Reb3JDtXS8HRRptR4O_5hVqZGTxFrPOjGGJvEP62zFdZzRUlHNoH_JBHpZZz3qftnpRrh2Aqm8OxPBqTmpS-EYC7xtyAji1eRps731g7FcYlR-K62XXOKH0hm9qwBjutEniJtCynirQpS-l08V9PkS3OBCXRA483h2-0XY9IAtcpoPhrDUbJR24bbBlvWcjd6FxNYJ25UZzxHrJKvRf1Of5rFSxaZGgnwSTwAjBHIfE_kUv-OV7IZXDreXEzcAkmV0yJDtIMa3ddDJPTpyfeK7cQ1fuCBqrWwU7LDMXpGa4FSe2nAA5m9b9_0CT57nRQhvmc9BLmgHdsc-ViSlCDlXN0KYyvvE-ndZn8Utrif99BiGrC0AOwaLZa47qLKtK7g7HLyKekfMAIpTBo-oDfsWqyTuwYne5VU1918oPKI5f570wolOfqom-ZJ3GBj8RMbn7cLak6cIl41eyR85lMSZCYzJNLesA6ZKNgiky7u-1GVYnrs-AmKy9wU7zznZ8eQUfa0CAVYEOLCllRk3RHaKSJU7NfIdskVbYi2m2aJG4kinGnb_RPeyw.
i86w754SyrEpxV6Mh6I7Tw
```
- ヘッダ部分を Base64 URL デコードするとデータの暗号化に使用されたアルゴリズムと鍵の種類を確認することができます。今回はデコードすると `{"enc":"A256GCM","alg":"dir}` となり、これは 256bitの共有鍵で AES GCM アルゴリズムを使用してデータが暗号されたことを示しています。データを受け取ったアプリケーションはその情報を基にデータの復号を行うことになります。

## Nested JWT
- JWS は署名、JWE は暗号化を行うための JWT のデータ形式であると説明しました。Nested JWT とは、JWS と JWE の両方を用いて、データの署名かつ暗号化を行った JWT のことを指します。処理の順序としては 署名 -> 暗号化 という流れになります。したがってデータの形式としては JWE となります。簡略化した図を以下に示します。

# 実装
- Nested JWT についての説明を行ったので、ここからは Kotlin による実装例を紹介します。

## 使用するライブラリ
- まずは JWT の作成に必要なライブラリをインストールします。今回は [connect2id/nimbus-jose-jwt](https://bitbucket.org/connect2id/nimbus-jose-jwt/src/master/) というライブラリを使用しました。このライブラリを使用することで JWS による署名、及び JWE による暗号化を行うことができます。`build.gradle.kts` を使用している場合は、以下を記述します。ライブラリのバージョンは 2021/4/3 時点での最新のバージョンを設定しています。

```:build.gradle.kts
...
dependencies {
  ...
	implementation("com.nimbusds:nimbus-jose-jwt:9.7")
  // 追加で必要なライブラリ
	implementation("org.bouncycastle:bcpkix-jdk15on:1.68")
	implementation("org.bouncycastle:bcprov-jdk15on:1.68")
}
```

## Nested JWT 生成処理実装
- それでは実際に Nested JWT を生成する処理を実装します。今回は `{"userName": "<ユーザ名>"}` という属性情報を用いて Nested JWT を生成することを想定しています。実装は以下となります。

```kt:NestedJwtCreator.kt
class NestedJwtCreator {

    /**
     * Nested JWT を生成するメソッド
     * @param signatureKey 署名を行う際に使用する鍵
     * @param encryptionKey 暗号化する際に使用する鍵
     * @param userName "userName" フィールドに設定される値
     * @return NestedJWT
     */
    fun create(
        signatureKey: RSAKey,
        encryptionKey: SecretKey,
        userName: String
    ): String {

        // 署名で使用する Signer を生成
        val signer: JWSSigner = RSASSASigner(signatureKey)

        // JWS 形式の JWT 生成
        val signedJwt = try {
            val jwtClaimsSet = JWTClaimsSet
                .Builder()
                .issueTime(Date())
                .claim("userName", userName)
                .build()

            SignedJWT(JWSHeader.Builder(JWSAlgorithm.RS256).build(), jwtClaimsSet).also {
                it.sign(signer)
            }
        } catch (ex: JOSEException) {
            throw IllegalStateException("jwt signature failed")
        }

        // 暗号化で使用する Encryptor 生成
        val encryptor = DirectEncrypter(encryptionKey)

        // JWE 形式の JWT を生成
        return try {
            JWEObject(JWEHeader.Builder(JWEAlgorithm.DIR, EncryptionMethod.A256GCM).build(), Payload(signedJwt)).also {
                it.encrypt(encryptor)
            }.serialize()
        } catch (ex: JOSEException) {
            throw IllegalStateException("jwt encryption failed")
        }
    }
}
```
- 今回署名には SHA-256 を使用した RSASSA-PKCS1-v1_5 アルゴリズム、暗号化には共通鍵を使用した AES GCM アルゴリズムを採用しています。

## データの復号テスト

- 上記の処理が想定した挙動となっているかのテストを実装しました。実装は以下となります。
```kt:NestedJwtCreatorTest.kt
internal class NestedJwtCreatorTest {

    @Test
    fun `テスト`() {
        // 秘密鍵生成
        val privateKey = RSAKeyGenerator(4096).keyUse(KeyUse.SIGNATURE).generate()

        // 暗号鍵生成
        val encryptionKey = KeyGenerator.getInstance("AES").let {
            it.init(EncryptionMethod.A256GCM.cekBitLength())
            it.generateKey()
        }

        val userName = "Penguin"
        val nestedJwtCreator = NestedJwtCreator()

        // NestedJWT 生成
        val nestedJwt = nestedJwtCreator.create(privateKey, encryptionKey, userName)

        // 検証
        val jweObject = JWEObject.parse(nestedJwt)

        // 暗号化方式と暗号アルゴリズムの検証
        jweObject.header.also {
            Assertions.assertEquals(JWEAlgorithm.DIR, it.algorithm)
            Assertions.assertEquals(EncryptionMethod.A256GCM, it.encryptionMethod)
        }

        // デコード
        jweObject.decrypt(DirectDecrypter(encryptionKey))

        // 署名検証および "userName" に正しい値がセットされているかどうか検証
        val publicKey = privateKey.toPublicJWK()
        jweObject.payload.toSignedJWT().also {
            Assertions.assertEquals(JWSAlgorithm.RS256, it.header.algorithm)
            Assertions.assertTrue(it.verify(RSASSAVerifier(publicKey)))
            Assertions.assertEquals(userName, it.jwtClaimsSet.getClaim("userName"))
        }
    }
}
```
- 自分の環境でテストを実行したところ、パスすることが確認できました。
