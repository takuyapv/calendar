# Calendarデモ

Vue+Nuxt+Vuetifyによるスケジューラーデモです。
サーバーサイドとの連携にはGraphQLを使用しています。

![mstsc_PDjOay2ZwI](https://user-images.githubusercontent.com/111565106/185811726-5306382a-0340-4b34-9f9a-52f01a2e5c10.png)

### 動画デモ

https://user-images.githubusercontent.com/111565106/185811655-68d8ebbd-8d34-44ba-adc7-660cbbce9f65.mp4

### フレームワーク、ライブラリ
* パッケージマネージャ
  * Yarn 
* ベースフレームワーク
  * Vue
  * Nuxt
* UIライブラリ
  * Vuetify
* GraphQLクライアント
  * nuxt-graphql-request

### 使用言語

TypescriptとJavascriptを併用しています。

メインとなるpages/calendar.vueではTypescriptでコーディングしています。

components/Calendar.vueではVuetifyのサイトがjavascriptをベースに解説しているので、Typescriptに変換する手間やメンテナンス性を考慮してjavascriptのままにしてあります。
  
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

