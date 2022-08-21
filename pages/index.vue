<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card class="logo py-4 d-flex justify-center">
        <NuxtLogo />
        <VuetifyLogo />
      </v-card>
      <v-card>
        <v-card-title class="headline">
          SPAデモアプリケーション
        </v-card-title>
        <v-card-text>
          <p>APIにGraphQLを用いた簡易デモアプリケーションです</p>
          <h3>サーバサイド</h3>
          <ul>
            <li>言語 Go</li>
            <li>フレームワーク Gin+gqlgen</li>
            <li>ORMライブラリ GORM</li>
            <li>データベース SQLite</li>
          </ul>
          <br>
          <h3>クライアントサイド</h3>
          <ul>
            <li>言語 TypeScript(Javascript)</li>
            <li>フレームワーク Vue+Nuxt</li>
            <li>UIフレームワーク Vuetify</li>
            <li>GraphQLクライアント nuxt-graphql-request</li>
          </ul>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import GqlgenLogo from '~/components/GqlgenLogo.vue';
export default {
    name: "IndexPage",
    components: { GqlgenLogo }
}
</script>
