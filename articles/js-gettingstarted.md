---
title: "JavaScriptフレームワークのプロジェクト作成コマンドを列挙した"
emoji: "🔧"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["javascript", "ツール", "nodejs", "npm", "yarn"]
published: true
---

## はじめに

JavaScriptのフレームワークにはnpmやYarnなどのパッケージマネージャーを使い簡単に依存関係を解決し、プロジェクトの雛形を作成してくれるコマンドが用意されていることが多いです。
コマンドラインツールによって簡単にプロジェクトの自動作成、開発サーバーでの実行ができ便利だと感じたため、有名なJavaScriptフレームワークの雛形作成コマンドを備忘録として挙げてみます。

各コマンドの`<project-name>`の部分が作成されるディレクトリ名、プロジェクト名になります。作成するプロジェクトに応じて適宜置き換えてください。

:::message
公式のドキュメントに沿って基本となるコマンドをまとめています。プロジェクト作成時に対話形式で設定可能な項目については記事内で扱いません。項目の説明や、利用できるオプションについては公式のドキュメントを参照のうえ実行をお願いします。
:::

## 前提条件

- Node.jsがインストールされていること（バージョン8以降、最新のLTSバージョンを推奨）
- npmもしくはYarnが利用可能であるこ（npmはNode.jsにデフォルトで搭載）

## Vue

### npm

```shell
$ npm install -g @vue/cli

$ vue create <project-name>
$ cd <project-name>
$ npm run serve
```

### Yarn

```shell
$ yarn global add @vue/cli

$ vue create <project-name>
$ cd <project-name>
$ npm run serve
```

[localhost:8080](http://localhost:8080)で開発サーバが立ち上がります。

![Vue.jsウェルカムページの画像](/images/js-gettingstarted/image01.png)

## Nuxt.js（2.x）

### npm

```shell
$ npm init nuxt-app <project-name>
$ cd <project-name>
$ npm run dev (yarn dev)
```

### npx

```shell
$ npx create-nuxt-app <project-name>
$ cd <project-name>
$ npm run dev
```

## Yarn

```shell
$ yarn create nuxt-app <project-name>
$ cd <project-name>
$ yarn dev
```

[localhost:3000](http://localhost:3000)で開発サーバーが立ち上がります。

![Nuxt.js2ウェルカムページの画像](/images/js-gettingstarted/image02.png)

## Nuxt.js（3.x）

Nuxt3からはセットアップ手順が大幅に変更されました。
`nuxi`コマンドを用いて従来よりも簡略的かつ高速にセットアップできます。nuxiとはNuxt用の新しいCLIになります。

```shell
$ npx nuxi init <project-name>
$ cd <project-name>
$ yarn install
$ yarn dev -o
```

[localhost:3000](http://localhost:3000)で開発サーバーが立ち上がります。

![Nuxt.js3ウェルカムページの画像](/images/js-gettingstarted/image02_2.png)

## React

### npm

```shell
$ npm init react-app <project-name>
$ cd <project-name>
$ npm start
```

### npx

```shell
$ npx create-react-app <project-name>
$ cd <project-name>
$ npm start
```

### Yarn

```shell
$ yarn create react-app <project-name>
$ cd <project-name>
$ yarn start
```

npm、npxでインストールする場合はYarnがインストールされているとデフォルトのパッケージマネージャーとしてYarnを使用するようになります。

[localhost:3000](http://localhost:3000)で開発サーバーが立ち上がります。

![Reactウェルカムページの画像](/images/js-gettingstarted/image03.png)

## Next.js

### npm

```shell
$ npx create-next-app <project-name>
$ cd <project-name>
$ npm run dev
```

### Yarn

```shell
$ yarn create next-app <project-name>
$ cd <project-name>
$ yarn dev
```

Reactと同じくYarnがインストールされているとデフォルトでYarnが使用されます。

[localhost:3000](http://localhost:3000)で開発サーバーが立ち上がります。

![Next.jsウェルカムページの画像](/images/js-gettingstarted/image04.png)

## Angular

### npm

```shell
$ npm install -g @angular/cli

$ ng new <project-name>
$ cd <project-name>
$ ng serve --open
```

### Yarn

```shell
$ npm install -g @angular/cli

$ ng config -g cli.packageManager yarn
$ ng new <project-name>
$ cd <project-name>
$ ng serve --open
```

`ng serve --open`とすることでデフォルトのブラウザで起動します。
[localhost:4200](http://localhost:4200)で開発サーバーが立ち上がります。

![Angularウェルカムページの画像](/images/js-gettingstarted/image05.png)

## Svelte

公式で公開されているテンプレートから新規プロジェクトを作る。

### npx

```shell
$ npx degit sveltejs/template <project-name>
$ cd <project-name>
$ npm install
$ npm run dev
```

[localhost:5000](http://localhost:5000)で開発サーバーが立ち上がります。

![Svelteウェルカムページの画像](/images/js-gettingstarted/image06.png)

## おわりに

有名なJavaScriptフレームワークの雛形プロジェクト作成コマンドを公式ドキュメントをもとに列挙してみました。簡単なコマンド1つですぐに開発を始める環境が作成できるのはとても便利です。フレームワークがここまで発展し、多くの人に選ばれる理由わかった気がします。

公式ドキュメントを見ながら記事を執筆する中で得た気づきが多くあるため、今後JavaScriptフレームワークについて更に理解を深めていきたいです。

最後まで読んでいただきありがとうございました。

## 参考

https://cli.vuejs.org/guide/creating-a-project.html#vue-create
https://ja.nuxtjs.org/docs/2.x/get-started/installation/
https://ja.reactjs.org/docs/create-a-new-react-app.html
https://github.com/facebook/create-react-app
https://nextjs.org/docs/api-reference/create-next-app
https://angular.jp/guide/setup-local
https://qiita.com/koinori/items/be672ddb9d3a7d315b13
https://svelte.dev/blog/the-easiest-way-to-get-started
https://github.com/sveltejs/template
