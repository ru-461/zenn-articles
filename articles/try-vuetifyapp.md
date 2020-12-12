---
title: "[Vuetify]即興でそれっぽいTweet文字カウンターを作る"
emoji: "💭"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Vue", "Vuetify", "初心者", "個人開発"]
published: false
---

# はじめに

Twitter では 1 回のツイートに文字制限があります。最大 140 文字でツイートするというのは有名な制約でもあり、Twitter の特徴です。
JavaScript を使って文字をカウントするアプリを作るサンプルは多くありますが、どうしてもコードが長くなってしまい見にくいと感じることがありました。JavaScript のフレームワークである Vue.js ではデータをリアクティブに扱えます。Vue.js のもつリアクティブを使うことでより簡単に記述できます。しかしテキストエリアの下に文字を表示するだけでは物足りないので、デザインもモダンなデザインでおしゃれにしてみました。今回はサンプルとして Twitter 風の文字カウンターアプリを Vuetify を使って作ってみました。少しのコードで画面のデザインが一気に変わっていくので作っていて楽しいです。VueCLI と Vuetify を組み合わせることで作業効率が上がりいい感じに爆速で開発できるようになります。

簡単な記述でコンポーネントを使ってアプリを構築できることに重点をおいて作成しました。即興で作成したものなのでこれがベストプラクティスではない可能性もあります。

# 完成イメージ

# 前提条件

- Node.js がインストールされており npm コマンドが使えること
- VueCLI がインストールされていること

# Vuetify とは？

`Vuetify`とは Vue 向け UI ライブラリになります。Google が近年採用しているマテリアルデザインを採用しており、誰でも簡単にモダンなデザインのコンポーネントを使うことができます。公式サイトは[Vuetify](https://vuetifyjs.com/en/)になります。サイト内にもサンプルが多くあるため、一通り見るだけでもどんなコンポーネントがあるかわかり見やすいドキュメントになっています。

# プロジェクト作成

VueCLI を使いおなじみの `vue create` で Vue のプロジェクトを作成します。

```shell
$ vue create counter-app
```

プロジェクト名はとりあえず `counter-app` とします。

```shell
Vue CLI v4.5.9
? Please pick a preset: (Use arrow keys)
❯ Default ([Vue 2] babel, eslint)
  Default (Vue 3 Preview) ([Vue 3] babel, eslint)
  Manually select features
```

上記のようにプリセットの選択を求められますが、そのまま Default で進めます。

プロジェクトが作成されます。

```shell
🎉  Successfully created project counter-app.
👉  Get started with the following commands:

 $ cd counter-app
 $ npm run serve
```

このような表示がでればプロジェクトの作成に成功しています。
画面の指示に従い、作成された `couter-app` ディレクトリに移動します。

```shell
$ npm run serve
```

上記の npm コマンドで開発サーバーが立ち上がります。
開発サーバーには http://localhost:8080 でアクセスできます。
ブラウザでアクセスし、以下の画面が表示されれば Vue のプロジェクトが正しく生成されています。
![Vue Welcome Page](https://storage.googleapis.com/zenn-user-upload/1jx3hff6qgxmo3ldt0jvzvkpn1lt)
上の Welcome ページで表示されているテンプレートは src ディレクトリ内の App.vue ファイルに記述されている内容になります。

```vue:src/App.vue
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

ソースを見ると、Helloworld.vue というコンポーネントをインポートして表示しているのがわかります。
見慣れない独特な書き方になっていますが、これは単一コンポーネントファイルと呼ばれ、最終的にビルドされるときに Html へと変換されて表示されるものとなります。`<template>` の中に HTML やコンポーネントを配置できます。`<script>` はおなじみの JabvaScript を記述する場所、`<style>` も CSS などのスタイルを定義する場所となります。

次に、UI ライブラリである Vuetify を導入していきます。

# Vuetify の導入

作成したプロジェクトに Vuetify プラグインを追加します。導入は簡単でプロジェクトのディレクトリで以下のコマンドを入力するだけです。

```shell
$ vue add vuetify
```

インストールに成功すると以下が表示されます。

```shell
✔  Successfully installed plugin: vue-cli-plugin-vuetify

? Choose a preset: (Use arrow keys)
❯ Default (recommended)
  Prototype (rapid development)
  Configure (advanced)
```

ここでもプロジェクト生成時と同じく色々と聞かれますが推奨となっている `Default` を選択します。

これで Vuetify を使う準備ができました。

この状態でサーバーを立ち上げアクセスすると Vuetify の Welcome ページに置き換わっているのがわかります。
![Vuetify Welcom Page](https://storage.googleapis.com/zenn-user-upload/bjjgmmfm5ekv34vmqd6p08cfslsr)

このページのソースを確認しようと `App.vue`を開くと中身が Vuetify のものに変わっています。

```vue:src/App.vue
<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Latest Release</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <HelloWorld/>
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';

export default {
  name: 'App',

  components: {
    HelloWorld,
  },

  data: () => ({
    //
  }),
};
</script>
```

ぱっと見るととても複雑に見えますが、`<template>`の中には Vuetify ですでに定義されているコンポーネントを配置して表示しているものになります。実際にコーディングしたほうが理解しやすいので、とりあえずページをまっさらな状態に戻して進めます。
`App.vue`を以下のように書き換えてサーバーで確認します。

```vue:src/App.vue
<template>
  <v-app>
    <v-main> </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  components: {},

  data: () => ({

  }),
};
</script>

```

これで http://localhost:8080 にアクセスしたとき表示されていたページが消えてなにもないページが表示されるようになります。

# アプリの機能を作る

次にアプリのメインとなる機能を Vuetify を使わずにそのままの形で構築していきます。
`<template>`の中を次のように書き換えます。

```vue:src/App.vue
<template>
  <v-app>
    <v-main>
      <h1>文字カウンター for Twitter</h1>
      <p>文字数をカウントします。ツイート前の文字数確認に便利です。</p>
      <textarea></textarea>
      <h2>あと◯文字入力できます。</h2>
    </v-main>
  </v-app>
</template>
```

このような表示になります。
完成版としては、テキストエリアのしたに配置した `◯文字` の部分に入力できる残り文字を表示したいので、どうするかを考えます。
Vue.js にはリアクティブに値を書き換える機能があり、その機能を使うことで簡単にリアルタイムで値を変更したり、計算できます。
今回は文字を動的に変更したいので以下のように書き換えます。

```vue:src/App.vue
<template>
  <v-app>
    <v-main>
      <h1>文字カウンター for Twitter</h1>
      <p>文字数をカウントします。ツイート前の文字数確認に便利です。</p>
      <textarea v-model="tweet"></textarea>
      <h2>あと{{ 140 - tweet.length }}文字入力できます。</h2>
      <button type="button">ツイートしてみる？</button>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  components: {},

  data: () => ({
    tweet: "", // 初期は空白
  }),
};
</script>
```

こうすることでテキストエリアに文字が入力されたときに、data()の中にある tweet プロパティをもとに文字数を計算してくれるようになります。
`<h2>`の中の文章に `{{ }}` という見慣れない記法が含まれていますが、これは `マスタッシュ記法` と呼ばれ、定義した data の変数をコンポーネントやテンプレートに埋め込んで表示できる Vue の特徴的な記法になります。Twitter では 140 文字の制限があるため、残り入力可能な文字数を求めるために、`{{ 140 - tweet.length }}` としています。変数名.length とすることで入力されている文字数を取得できます。入力されている文字数はリアルタイムで tweet に反映されるため、リアルタイムに残り文字数を計算してくれるようになります。

ここまでで機能の基礎部分が完成したので、今回導入した Vuetify を使って画面をリッチに仕上げていきます。

# Vuetify コンポーネントで置き換える

Vuetify の基本として、template 内では一番親要素に `<v-app>` メインとなるコンテンツに `<v-main>`を使います。
