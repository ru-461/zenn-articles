---
title: "即興でツイート文字数カウンターを作る【Vuetify】"
emoji: "🎨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Vue", "Vuetify", "初心者", "個人開発"]
published: true
---

この記事は[Vue Advent Calendar 2020](https://qiita.com/advent-calendar/2020/vue)へ投稿したものになります。

## はじめに

Twitterでは1回のツイートに文字制限があります。最大140文字でツイートのがTwitterの特徴でもあり特徴的なところです。

JavaScriptを使って文字をカウントするアプリを作るサンプルは多くありますが、どうしてもコードが長くなってしまい見にくいと感じることがありました。JavaScriptフレームワークであるVue.jsを使うことでリアクティブなデータ表示ができます。Vue.jsのリアクティブシステムを使うことでより簡単に記述できます。しかしテキストエリアの下に文字を表示するだけでは物足りないので、デザインもモダンなデザインでおしゃれにしてみました。今回はサンプルとしてTwitter風の文字カウンターアプリをVuetifyを使って作ってみました。少しのコードで画面のデザインが一気に変わっていくので作っていて楽しいです。Vue CLIとVuetifyを組み合わせることで作業効率が上がりいい感じに爆速で開発できるようになります。

簡単な記述でコンポーネントを使ってアプリを構築できることに重点をおいて作成しました。Vue.jsとVuetifyを使って初めてアプリを作る人、Vuetifyがどのようなものかを知りたい人向けに作成しているため、これがベストプラクティスではない可能性もあります。あらかじめご了承ください。

## 完成イメージ

![完成イメージの画像](https://i.gyazo.com/13a9f3438c632fafead61860ee382092.gif)

テキストエリアに文字を入力すると下のカウンターが動的に変化します。
コードはこちらで公開しています。
https://github.com/ru-461/tweet-counter

## 前提条件

- Node.jsがインストールされておりnpmコマンドが使えること
- Vue CLIがインストールされていること

## Vuetifyとは

VuetifyとはVue向けUIライブラリになります。Googleが近年採用しているマテリアルデザインを採用しており、誰でも簡単にモダンなデザインのコンポーネントを使うことができます。公式サイトは[Vuetify](https://vuetifyjs.com/en)になります。サイト内にもサンプルが多くあるため、一通り見るだけでもどんなコンポーネントがあるかわかり見やすいドキュメントになっています。

## プロジェクト作成

Vue CLIを使い`vue create`でVueのプロジェクトを作成します。

```shell
$ vue create counter-app
```

プロジェクト名はとりあえず「counter-app」とします。

```shell
Vue CLI v4.5.9
? Please pick a preset: (Use arrow keys)
❯ Default ([Vue 2] babel, eslint)
  Default (Vue 3 Preview) ([Vue 3] babel, eslint)
  Manually select features
```

上記のようにプリセットの選択を求められますが、そのままDefaultで進めます。

プロジェクトが作成されます。

```shell
🎉  Successfully created project counter-app.
👉  Get started with the following commands:

$ cd counter-app
$ npm run serve
```

このような表示がでればプロジェクトの作成に成功しています。
画面の指示に従い、作成されたcouter-appディレクトリに移動します。

```shell
$ npm run serve
```

上記のnpmコマンドで開発サーバーが立ち上がります。
開発サーバーには[localhost:8080](http://localhost:8080)でアクセスできます。

ブラウザでアクセスし、以下の画面が表示されればプロジェクトが正しく生成されています。

![Vue Welcome Pageの画像](/images/try-vuetifyapp/image01.png)

上のWelcomeページで表示されているテンプレートはsrcディレクトリ内のApp.vueファイルに記述されている内容になります。

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

ソースを見ると、Helloworld.vueというコンポーネントをインポートして表示しているのがわかります。
見慣れない独特な書き方になっていますが、これは単一コンポーネントファイルと呼ばれ、最終的にビルドされるときにHtmlへと変換されて表示されるものとなります。`<template>`の中にHTMLやコンポーネントを配置できます。`<script>`はおなじみのJavaScriptを記述する場所、`<style>`もCSSなどのスタイルを定義する場所となります。

次に、UIライブラリであるVuetifyを導入していきます。

## Vuetifyの導入

作成したプロジェクトにVuetifyプラグインを追加します。導入は簡単でプロジェクトのディレクトリで以下のコマンドを入力するだけです。

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

ここでもプロジェクト生成時と同じく色々と聞かれますが推奨となっているDefaultを選択します。

これでVuetifyを使う準備ができました。

この状態でサーバーを立ち上げアクセスするとVue.js AppのWelcomeページからVuetifyのWelcomeページに置き換わっているのがわかります。

![Vuetify Welcom Pageの画像](/images/try-vuetifyapp/image02.png)

App.vueを開くと中身がVuetifyのものに変わっています。

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

ぱっと見るととても複雑に見えますが、`<template>`の中にVuetifyで定義されているコンポーネントを配置して表示しているものになります。実際にコーディングしたほうが理解しやすいので、とりあえずページをまっさらな状態に戻して進めます。
App.vueを以下のように書き換えてサーバーで確認します。

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

これで http://localhost:8080 にアクセスしたとき、何もないページが表示されるようになります。

# アプリの機能を作る

次にアプリのメインとなる機能をVuetifyを使わずにそのままの形で構築していきます。
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

![初期表示の画像](/images/try-vuetifyapp/image03.png)

最終的に、テキストエリアのしたに配置した◯文字へ入力できる残り文字を表示したいです。
Vue.jsにはリアクティブに値を書き換える機能があり、その機能を使うことで簡単にリアルタイムで値を変更したり、計算できます。
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

テキストエリアに文字が入力されたときに、`data()`の中にあるツイートプロパティをもとに文字数を計算してくれるようになります。
`<h2>`の中に`{{ }}`という見慣れない記法が含まれていますが、これは「マスタッシュ記法」と呼ばれ、dataの変数をコンポーネントやテンプレートに埋め込んで表示できるVueの特徴的な記法になります。Twitterでは140文字の制限があるため、残り入力可能な文字数を求めるために、`{{ 140 - tweet.length }}`としています。変数名.lengthとすることで入力されている文字数を取得できます。入力されている文字数はリアルタイムでツイートに反映されるため、リアルタイムに残り文字数を計算してくれるようになります。v-modelに対して`.trim`とすることで空白を取り除くことができます。

ここまでで基礎部分が完成したので、今回導入したVuetifyを使ってスタイリングしていきます。

## Vuetifyコンポーネントで置き換える

Vuetifyの基本として、template内では一番親要素に`<v-app>`メインとなるコンテンツに`<v-main>`を使います。この中にページ固有のコンテンツを配置していきます。Vuetifyにはテキストエリアコンポーネントがあります。ドキュメントは[こちら](https://vuetifyjs.com/en/components/textareas/#usage)になります。Vuetifyのドキュメントにはサンプルのコードに加えて、Prop（プロパティ）まとめられているので便利です。それでは先程の`<textarea>`をテキストエリアコンポーネントで置き換えていきます。

```vue:src/App.vue
<v-textarea
  v-model.trim="tweet">
</v-textarea>
```

このようにして保存するとテキストエリアが画面全体に広がり、少しおしゃれなテキストエリアになります。コンポーネントを置き換えても先程のテキストエリアと同じように動作するのがわかります。ここにProp（プロパティ）を追加してテキストエリアを拡張していきます。

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

![カスタマイズ後のテキストエリア画像](/images/try-vuetifyapp/image04.png)

これだけで見た目がかなり変わり、よりおしゃれになりました。

ここでは背景色を白にして最大文字数を140文字にしています。`rounded`や`dense`、`outlined`をつけることでテキストエリアのスタイルを変えることができます。ドキュメントを見ながらどのように変わるのかを見るとより理解が深まります。色々と試してみてください。ここで背景色も変え全体のスタイルを適用します。ここでは背景色をTwitterのロゴの色と同じにし、コンテンツを中央寄せします。App.vue内で`#app`に対してスタイルを設定することで全体に適用されます。

```vue:src/App.vue
<style>
  #app {
    text-align: center;
    background-color: #00aced;
  }
</style>
```

### スタイルを簡単に設定する

ここまでくるとかなりWebアプリっぽくなってきました。Vuetifyではスタイルをclassで簡単に指定できる機能があり、スタイルシートを記述せずともスタイルを調整できます。背景を青にしたことで文字が少し見にくくなってしまったので文字色を白にしてみます。要素に対して`class="white--text"`とすると文字色を白にできます。ハイフンが2つ入るのに注意です。色以外にもclassで指定できるスタイルは多くあるため、ドキュメントを参考に調整してみてください。ここでは色に合わせてマージンやパディングも同時に設定できます。これは「Spacing-Helper-Class」と呼ばれ、marginを`m`、paddingを`p`としてディレクションを設定することで要素に反映されます。

指定できるディレクションは以下のとおりです。

| ディレクション | 方向               | サイズ |
| -------------- | -----------------  | ------ |
| t              | top（上方向）      | 0-12   |
| b              | bottom（下方向）   | 0-12   |
| l              | left（左方向）     | 0-12   |
| r              | right（右方向）    | 0-12   |
| x              | left&right（左右） | 0-12   |
| y              | top&bottom（上下） | 0-12   |
| x              | all（全方向）      | 0-12   |

サイズは、1にたいして4pxずつ適用されます。要素同士をくっつけたい場合は0を指定することで実現できます。

以上を踏まえて文字要素のスタイルを微調整していきます。

現在の状態では、要素が全体的に上の方に寄っていて、画面下の空白が気になるため、`<v-main>`を`<v-container fill-height>`で囲み上下に均等の幅を持たせて中央揃えにします。

文字の色を変えて、スタイルを調整を調整すると以下のようになります。

![スタイル後の画面画像](/images/try-vuetifyapp/image05.png)

ここまでで文字数をカウントする機能とスタイリングができました。

### Twitterへリンクするボタンを作成する

付加機能としてTwitterへアクセスできるボタンを配置してみます。APIを使うことでTwitterへツイートさせることもできるようですが、今回はツイートを行わず、 Twitterのホームへリンクさせるボタンとして作成します。

引き続きApp.vueに記述していきます。Vuetifyには [v-btn](https://vuetifyjs.com/ja/components/buttons)というボタンコンポーネントも用意されているのでこちらを使っていきます。今回はv-btnを使い以下のようにしました。

```vue:src/App.vue
<v-btn
  elevation="6"
  color="white"
  x-large
  href="https://twitter.com/"
  target="blank"
  class="mt-4">ツイートしてみる？</v-btn>
```

![v-btn追加後の画像](/images/try-vuetifyapp/image06.png)

上のボタンをブラウザで見ると以下のようになります。

ボタンの配置ができました。上の記述だけで少し立体感のあるボタンが作成されました。href対してTwitterのURL 、`target="blank"`としているため、ボタンをクリックするとTwitterが別タブで開かれます。ツイートする機能はないのであくまでツイートを促すだけのボタンなります。メインの機能は文字数のカウンターのため、ツイートボタンはあくまでオマケ的な位置づけです。

### Twitterのブランドアイコンを表示する

ここまでで一通りの機能ができました。しかしこれだけだとTwitterの文字カウンターというのが少しひと目でわかりにくいのでタイトルの横にTwitterのアイコンを表示してみます。Vuetifyにはアイコンを便利に扱えるコンポーネントが用意されているため、活用します。

今回はFontAwesomeにある[Twitterアイコン](https://fontawesome.com/icons/twitter?style=brands)を利用します。

VuetifyでFontAwesomeを利用するためには、あらかじめFontAwesomeのアイコンをインストールする必要があります。
ターミナル上でnpmを使いインストールしていきます。

```shell
npm install  @fortawesome/fontawesome-svg-core
npm install  @fortawesome/free-brands-svg-icons
npm install  @fortawesome/vue-fontawesome
```

インストールが完了したらsrc/main.jsに以下の記述をしてアイコンをインポートします。

```javascript:src/main.js
import { library } from '@fortawesome/fontawesome-svg-core'
import { faTwitter } from '@fortawesome/free-brands-svg-icons'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

library.add(faTwitter)

Vue.component('font-awesome-icon', FontAwesomeIcon)
```

Twitterのアイコンを使いたいため、インストールしたfree-brands-svg-iconsからTwitterのブランドアイコンfaTwitterをインポートしてライブラリに追加しています。FontAwesomeIconコンポーネントを定義してApp.vueで呼び出せるようにします。

最後にApp.vueでFontAwesomeIconコンポーネントを使いタイトルの横にTwitterのアイコンを描画します。

```vue:src/App.vue
<h1 class="white--text my-8">文字カウンター for Twitter <font-awesome-icon :icon="['fab','twitter']"/></h1>
```

以上のようにすることでタイトルの横にTwitterのアイコンが表示されるようになります。これでTwitterらしさを出すことができました。

![完成後の画像](/images/try-vuetifyapp/image06.png)

## さいごに

今回は、Vuetifyを使ってTwitter向け文字数カウントアプリを作成しました。VuetifyはVue用に作られたUIフレームワークライブラリということもあり、Vue CLIと組み合わせることで爆速な開発が実現できます。Vue.jsは公式ドキュメントがわかりやすい日本語になっていたり、コミュニティが活発なため情報を手に入れやすいです。Vue.jsの基本を理解したあとVue CLIなどのツールを使って爆速な開発環境を簡単に作ることができるのも魅力の1つなので、今回のようなアプリを通して学ぶのもおすすめです。

Vue.jsを使って初めてアプリを作る人の参考になれば幸いです。最後まで読んでいただきありがとうございました。

## 参考

[Vuetify に入門する](https://qiita.com/azukiazusa/items/16ebffd361af8fa58333)
[Font awesome を Vue.js で使ってみよう](https://qiita.com/kurararara/items/d76776a7dc2d763a068b)
[レイアウト微調整に便利な Spacing Helper (Vuetify) を使ってみる](https://riotz.works/articles/lopburny/2019/08/12/arrange-vuetify-components-with-spacing-helper)
