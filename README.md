Vue+Nuxt+Vuetifyによるスケジューラーデモです。
サーバーサイドとの連携にはGraphQLを使用しています。

### フレームワーク、ライブラリ
* パッケージマネージャ
  * Yarn 
* ベースフレームワーク
  * Vue
  * Nuxt
  * Vuetify
* UIライブラリ
  * Vuetify
* GraphQLクライアント
  * nuxt-graphql-request
  
### 使用方法

事前にバックエンドサーバーを起動しておく必要があります。

#### 開発用ローカルサーバの起動
下記コマンド実行後、ブラウザから http://localhost:3000 を開いてください。

```
yarn dev
```

#### 本番用データのビルド

distディレクトリ内に必要なstaticファイルが作成されます。
```
yarn generate
```

