---
title: "即興でツイート文字数カウンターを作る [Vuetify]"
emoji: "🎨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Vue", "Vuetify", "初心者", "個人開発"]
published: true
---

この記事は[Vue Advent Calendar 2020](https://qiita.com/advent-calendar/2020/vue)へ投稿したものになります。

# はじめに

Twitter では 1 回のツイートに文字制限があります。最大 140 文字でツイートのが Twitter の特徴でもあり特徴的なところです。
JavaScript を使って文字をカウントするアプリを作るサンプルは多くありますが、どうしてもコードが長くなってしまい見にくいと感じることがありました。JavaScript フレームワークである Vue.js を使うことでリアクティブなデータ表示ができます。Vue.js のリアクティブシステムを使うことでより簡単に記述できます。しかしテキストエリアの下に文字を表示するだけでは物足りないので、デザインもモダンなデザインでおしゃれにしてみました。今回はサンプルとして Twitter 風の文字カウンターアプリを Vuetify を使って作ってみました。少しのコードで画面のデザインが一気に変わっていくので作っていて楽しいです。VueCLI と Vuetify を組み合わせることで作業効率が上がりいい感じに爆速で開発できるようになります。

簡単な記述でコンポーネントを使ってアプリを構築できることに重点をおいて作成しました。Vue.js と Vuetify を使って初めてアプリを作る人、Vuetify がどういうものかを知りたい人向けに作成しているため、これがベストプラクティスではない可能性もあります。あらかじめご了承ください。

# 完成イメージ

![完成イメージ](https://i.gyazo.com/13a9f3438c632fafead61860ee382092.gif)
テキストエリアに文字を入力すると下のカウンターが動的に変化します。
コードはこちらで公開しています。
https://github.com/ryu-461/tweet-counter

# 前提条件

- Node.js がインストールされており npm コマンドが使えること
- VueCLI がインストールされていること

# Vuetify とは？

`Vuetify`とは Vue 向け UI ライブラリになります。Google が近年採用しているマテリアルデザインを採用しており、誰でも簡単にモダンなデザインのコンポーネントを使うことができます。公式サイトは[Vuetify](https://vuetifyjs.com/en/)になります。サイト内にもサンプルが多くあるため、一通り見るだけでもどんなコンポーネントがあるかわかり見やすいドキュメントになっています。

# プロジェクト作成

VueCLI を使い `vue createコマンド` で Vue のプロジェクトを作成します。

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

ブラウザでアクセスし、以下の画面が表示されればプロジェクトが正しく生成されています。
![Vue Welcome Page](https://storage.googleapis.com/zenn-user-upload/587mvjvowie18447ki2arjz89ldi)
上の Welcome ページで表示されているテンプレートは src ディレクトリ内の `App.vue` ファイルに記述されている内容になります。

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
見慣れない独特な書き方になっていますが、これは単一コンポーネントファイルと呼ばれ、最終的にビルドされるときに Html へと変換されて表示されるものとなります。`<template>` の中に HTML やコンポーネントを配置できます。`<script>` はおなじみの JavaScript を記述する場所、`<style>` も CSS などのスタイルを定義する場所となります。

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

この状態でサーバーを立ち上げアクセスすると Vue.js App の Welcome ページから Vuetify の Welcome ページに置き換わっているのがわかります。
![Vuetify Welcom Page](https://storage.googleapis.com/zenn-user-upload/rx00t2tqd9nbeooi7m7ta3yiultz)

`App.vue`を開くと中身が Vuetify のものに変わっています。

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

ぱっと見るととても複雑に見えますが、`<template>`の中に Vuetify で定義されているコンポーネントを配置して表示しているものになります。実際にコーディングしたほうが理解しやすいので、とりあえずページをまっさらな状態に戻して進めます。
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

これで http://localhost:8080 にアクセスしたとき、なにもないページが表示されるようになります。

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

ブラウザ上で確認すると以下のようになります。
![初期状態](https://storage.googleapis.com/zenn-user-upload/3nj9bwhuxgcc5j6r6dsc95fm6m7e)
最終的に、テキストエリアのしたに配置した `◯文字` へ入力できる残り文字を表示したいです。
Vue.js にはリアクティブに値を書き換える機能があり、その機能を使うことで簡単にリアルタイムで値を変更したり、計算できます。
今回は文字を動的に変更したいので以下のように書き換えます。

```vue:src/App.vue
<template>
  <v-app>
    <v-main>
      <h1>文字カウンター for Twitter</h1>
      <p>文字数をカウントします。ツイート前の文字数確認に便利です。</p>
      <textarea v-model.trim="tweet"></textarea>
      <h2>あと{{ 140 - tweet.length }}文字入力できます。</h2>
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
`<h2>`の中の文章に `{{ }}` という見慣れない記法が含まれていますが、これは `マスタッシュ記法` と呼ばれ、定義した data の変数をコンポーネントやテンプレートに埋め込んで表示できる Vue の特徴的な記法になります。Twitter では 140 文字の制限があるため、残り入力可能な文字数を求めるために、`{{ 140 - tweet.length }}` としています。変数名.length とすることで入力されている文字数を取得できます。入力されている文字数はリアルタイムで tweet に反映されるため、リアルタイムに残り文字数を計算してくれるようになります。`v-model` に対して `.trim` とすることで空白を取り除くことができます。

ここまでで基礎部分が完成したので、今回導入した Vuetify を使ってスタイリングしていきます。

# Vuetify コンポーネントで置き換える

Vuetify の基本として、template 内では一番親要素に `<v-app>` メインとなるコンテンツに `<v-main>`を使います。この中にページ固有のコンテンツを配置していきます。Vuetify にはテキストエリアコンポーネントがあります。ドキュメントは[こちら](https://vuetifyjs.com/en/components/textareas/#usage)になります。Vuetify のドキュメントにはサンプルのコードに加えて、Prop(プロパティ)がまとめられているので便利です。それでは先程の `<textarea>` をテキストエリアコンポーネントで置き換えていきます。

```vue:src/App.vue
 <v-textarea
      v-model.trim="tweet">
  </v-textarea>
```

このようにして保存するとテキストエリアが画面全体に広がり、少しおしゃれなテキストエリアになります。コンポーネントを置き換えても先程のテキストエリアと同じように動作するのがわかります。ここに Prop(プロパティ) を追加してテキストエリアを拡張していきます。

```vue:src/App.vue
<v-textarea
  dense
  single-line
  v-model.trim="tweet"
  background-color="#ffffff"
  rounded
  outlined
  rows="10"
  required
  label="いまどうしてる？"
  maxlength="140"
  class="mx-10"
></v-textarea>
```

上のようにすることでテキストエリアが以下のようになります。

![](https://storage.googleapis.com/zenn-user-upload/vhw7kzwem01eo7t4znxaiuiz8o49)

これだけで見た目がかなり変わり、よりおしゃれになりました。

ここでは背景色を白にして最大文字数を 140 文字にしています。`rounded` や `dense`、`outlined`をつけることでテキストエリアのスタイルを変えることができます。ドキュメントを見ながらどのように変わるのかを見るとより理解が深まります。色々と試してみてください。ここで背景色も変え全体のスタイルを適用します。ここでは背景色を Twitter のロゴの色と同じにし、コンテンツを中央寄せします。App.vue 内で `#app` に対してスタイルを設定することで全体に適用されます。

```vue:src/App.vue
<style>
#app {
  text-align: center;
  background-color: #00aced;
}
</style>
```

## スタイルを簡単に設定する

ここまでくるとかなり Web アプリっぽくなってきました。Vuetify ではスタイルを class で簡単に指定できる機能があり、スタイルシートを記述せずともスタイルを調整できます。背景を青にしたことで文字が少し見にくくなってしまったので文字色を白にしてみます。要素に対して `class="white--text"` とすると文字色を白にできます。ハイフンが 2 つ入るのに注意です。色以外にも class で指定できるスタイルは多くあるため、ドキュメントを参考に調整してみてください。ここでは色に合わせてマージンやパディングも同時に設定できます。これは `Spacing-Helper-Class` と呼ばれ、margin を m、padding を p としてディレクションを設定することで要素に反映されます。

指定できるディレクションは以下のとおりです。

| ディレクション | 方向              | サイズ |
| -------------- | ----------------- | ------ |
| t              | top (上方向)      | 0-12   |
| b              | bottom (下方向)   | 0-12   |
| l              | left (左方向)     | 0-12   |
| r              | right (右方向)    | 0-12   |
| x              | left&right (左右) | 0-12   |
| y              | top&bottom (上下) | 0-12   |
| x              | all (全方向)      | 0-12   |

サイズは、1 にたいして 4px ずつ適用されます。要素をくっつけたい場合は 0 を指定することで実現できます。

以上を踏まえて文字要素のスタイルを微調整していきます。

現在の状態では、要素が全体的に上の方に寄っていて、画面下の空白が気になるため、`<v-main>` を `<v-container fill-height>`で囲み上下に均等の幅を持たせて中央揃えにします。

文字の色を変えて、スタイルを調整を調整すると以下のようになります。
![スタイル後の画面](https://storage.googleapis.com/zenn-user-upload/2h735x9g2acw4afxqwef9y0sd8o0)

ここまでで文字数をカウントする機能とスタイリングができました。

## Twitter へリンクするボタンを作成する

付加機能として Twitter へアクセスできるボタンを配置してみます。API を使うことで Twitter へツイートさせることもできるようですが、今回はツイートを行わず、 Twitter のホームへリンクさせるボタンとして作成します。

引き続き App.vue に記述していきます。Vuetify には [`v-btn`](https://vuetifyjs.com/ja/components/buttons/)というボタンコンポーネントも用意されているのでこちらを使っていきます。今回は `v-btn` を使い以下のようにしました。

```vue:src/App.vue
<v-btn
  elevation="6"
  color="white"
  x-large
  href="https://twitter.com/"
  target="blank"
  class="mt-4">ツイートしてみる？</v-btn>
```

![v-btn追加後](https://storage.googleapis.com/zenn-user-upload/18an71n21bf6v7orxlz5mvhvlm42)
上のボタンをブラウザで見ると以下のようになります。

ボタンの配置ができました。上の記述だけで少し立体感のあるボタンが作成されました。href 対して Twitter の URL 、target="blank" としているため、ボタンをクリックすると Twitter が別タブで開かれます。ツイートする機能はないのであくまでツイートを促すだけのボタンなります。メインの機能は文字数のカウンターのため、ツイートボタンはあくまでオマケ的な位置づけです。

## Twitter のブランドアイコンを表示する

ここまでで一通りの機能ができました。しかしこれだけだと Twitter の文字カウンターというのが少しひと目でわかりにくいのでタイトルの横に Twitter のアイコンを表示してみます。Vuetify にはアイコンを便利に扱えるコンポーネントが用意されているため、活用します。
今回は FontAwesome にある[Twitter アイコン](https://fontawesome.com/icons/twitter?style=brands)を利用します。

Vuetify で FontAwesome を利用するためには、あらかじめ FontAwesome のアイコンをインストールする必要があります。
ターミナル上で npm を使いインストールしていきます。

```shell
npm install  @fortawesome/fontawesome-svg-core
npm install  @fortawesome/free-brands-svg-icons
npm install  @fortawesome/vue-fontawesome
```

インストールが完了したら `src/main.js` に以下の記述をしてアイコンをインポートします。

```javascript:src/main.js
import { library } from '@fortawesome/fontawesome-svg-core'
import { faTwitter } from '@fortawesome/free-brands-svg-icons'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

library.add(faTwitter)

Vue.component('font-awesome-icon', FontAwesomeIcon)
```

今回は Twitter のアイコンを使いたいため、インストールした `free-brands-svg-icons` から Twitter のブランドアイコン `faTwitter` をインポートしてライブラリに追加しています。`FontAwesomeIconコンポーネント`を定義して App.vue で呼び出せるようにします。

最後に `App.vue` の方で FontAwesomeIcon コンポーネントを使いタイトルの横に Twitter のアイコンを描画します。

```vue:src/App.vue
<h1 class="white--text my-8">文字カウンター for Twitter <font-awesome-icon :icon="['fab','twitter']"/></h1>
```

以上のようにすることでタイトルの横に Twitter のアイコンが表示されるようになります。これで Twitter らしさを出すことができました。
![完成後の画像](https://storage.googleapis.com/zenn-user-upload/ma3bdad2chposz072xv4xzo4ka9w)

# さいごに

今回は、Vuetify を使って Twitter 向け文字数カウントアプリを作成しました。Vuetify は Vue 用に作られた UI フレームワークライブラリということもあり、VueCLI と組み合わせることで爆速な開発が実現できます。Vue.js は公式ドキュメントがわかりやすい日本語になっていたり、コミュニティが活発なため情報を手に入れやすいです。Vue.js の基本を理解したあと VueCLI などのツールを使って爆速な開発環境を簡単に作ることができるのも魅力の 1 つなので、今回のようなアプリを通して学ぶのもおすすめです。

Vue.js を使って初めてアプリを作る人の参考になれば幸いです。最後まで読んでいただきありがとうございました。

# 参考

[Vuetify に入門する](https://qiita.com/azukiazusa/items/16ebffd361af8fa58333)

[レイアウト微調整に便利な Spacing Helper (Vuetify) を使ってみる](https://riotz.works/articles/lopburny/2019/08/12/arrange-vuetify-components-with-spacing-helper/)

[Font awesome を Vue.js で使ってみよう](https://qiita.com/kurararara/items/d76776a7dc2d763a068b)
