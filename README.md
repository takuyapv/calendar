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
  
### ビルド、セットアップ

```
# 依存関係モジュールのインストール
$ yarn install

# 開発用サーバ localhost:3000 (事前にバックエンド(API)サーバーを起動しておく必要があります。)
$ yarn dev

# プロダクト用サーバ server
$ yarn build
$ yarn start

# 静的プロジェクトの書き出し
$ yarn generate
```

