---
title: "JavaScriptフレームワークのプロジェクト作成コマンドを列挙した"
emoji: "🔧"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["javascript", "nodejs", "npm", "yarn", "ツール"]
published: true
---

# はじめに

JavaScript のフレームワークには npm や Yarn などのパッケージマネージャーを使い簡単に依存関係を解決し、プロジェクトの雛形を作成してくれるコマンドが用意されていることが多いです。
スクラッチでの作成もできますが、コマンドラインツールによって簡単にプロジェクトの自動作成、開発サーバーでの実行ができ便利だと感じたため、有名な JavaScript フレームワークの雛形作成コマンドを備忘録として挙げてみます。

各コマンドの `<project-name>` の部分が作成されるディレクトリ名、デフォルトのプロジェクト名になります。作成するプロジェクトに応じて適宜置き換えてください。

:::message
公式のドキュメントに沿って基本となるコマンドをまとめています。プロジェクト作成時に対話形式で設定可能な項目については記事内で扱いません。項目の説明や、利用できるオプションについては公式のドキュメントを参照のうえ実行をお願いします。
:::

# 前提条件

- node.js がインストールされていること(バージョン 8 以降、最新の LTS バージョンを推奨)
- npm もしくは Yarn が利用可能であること(npm は node.js にデフォルトで搭載)

# Vue

## npm

```shell
npm install -g @vue/cli

vue create <project-name>
cd <project-name>
npm run serve
```

## Yarn

```shell
yarn global add @vue/cli

vue create <project-name>
cd <project-name>
npm run serve
```

http://localhost:8080 で開発サーバが立ち上がります。

![Vue](https://storage.googleapis.com/zenn-user-upload/cvdlss3ecnw2ilpdjmyqurnvptit)

# Nuxt.js

## npm

```shell
npm init nuxt-app <project-name>
cd <project-name>
npm run dev (yarn dev)
```

## npx

```shell
npx create-nuxt-app <project-name>
cd <project-name>
npm run dev
```

## Yarn

```shell
yarn create nuxt-app <project-name>
cd <project-name>
yarn dev
```

http://localhost:3000 で開発サーバーが立ち上がります。

![nuxt.js](https://storage.googleapis.com/zenn-user-upload/pyqvhr5jtrwzxtxidjcw6cktwtuh)

# React

## npm

```shell
npm init react-app <project-name>
cd <project-name>
npm start
```

## npx

```shell
npx create-react-app <project-name>
cd <project-name>
npm start
```

## Yarn

```shell
yarn create react-app <project-name>
cd <project-name>
yarn start
```

npm、npx でインストールする場合は Yarn がインストールされているとデフォルトで Yarn が使用されます。

http://localhost:3000 で開発サーバーが立ち上がります。

![react](https://storage.googleapis.com/zenn-user-upload/6mttoycvin579n3lis5ehe2nbh7p)

# Next.js

## npm

```shell
npx create-next-app <project-name>
cd <project-name>
npm run dev
```

## Yarn

```shell
yarn create next-app <project-name>
cd <project-name>
yarn dev
```

React と同じく npm、npx でインストールする場合は Yarn がインストールされているとデフォルトで Yarn が使用されます。

http://localhost:3000 で開発サーバーが立ち上がります。

![next.js](https://storage.googleapis.com/zenn-user-upload/91f4t8yih9l5azs2k7yotsmvb3aj)

# Angular

## npm

```shell
npm install -g @angular/cli

ng new <project-name>
cd <project-name>
ng serve --open
```

## Yarn

```shell
npm install -g @angular/cli

ng config -g cli.packageManager yarn
ng new <project-name>
cd <project-name>
ng serve --open
```

`ng serve --open`とすることでデフォルトのブラウザで起動します。
http://localhost:4200 で開発サーバーが立ち上がります。

![angular](https://storage.googleapis.com/zenn-user-upload/p9fb7u0bkh2f6cer69st888s60yo)

# Svelte

公式で公開されているテンプレートから新規プロジェクトを作る。

## npx

```shell
npx degit sveltejs/template <project-name>
cd <project-name>
npm install
npm run dev
```

http://localhost:5000 で開発サーバーが立ち上がります。

![svelte](https://storage.googleapis.com/zenn-user-upload/0egd4abo3zlc9kxd4vjv0fa51g8m)

# おわりに

有名な JavaScript フレームワークの雛形プロジェクト作成コマンドを公式ドキュメントをもとに列挙してみました。簡単なコマンド 1 つですぐに開発を始める環境が作成できるのはとても便利です。フレームワークがここまで発展し、多くの人に選ばれる理由わかった気がします。

公式ドキュメントを見ながら記事を執筆する中で得た気づきが多くあるため、今後 JavaScript フレームワークについて更に理解を深めていきたいです。

以上、ここまで読んでいただきありがとうございました。

# 参考ドキュメント

https://cli.vuejs.org/guide/creating-a-project.html#vue-create
https://ja.nuxtjs.org/docs/2.x/get-started/installation/
https://ja.reactjs.org/docs/create-a-new-react-app.html
https://github.com/facebook/create-react-app
https://nextjs.org/docs/api-reference/create-next-app
https://angular.jp/guide/setup-local
https://qiita.com/koinori/items/be672ddb9d3a7d315b13
https://svelte.dev/blog/the-easiest-way-to-get-started
https://github.com/sveltejs/template
