---
title: "npxコマンドとは？ 何ができるのか？"
emoji: "💬"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["nodejs", "npm", "npx", "javascript"]
published: true
---

# はじめに

[Nuxt.js](https://nuxtjs.org/ja/)のプロジェクトを作成する際に、[公式ドキュメント](https://ja.nuxtjs.org/docs/2.x/get-started/installation/)を読みながら進めていたのですが。

```shell
$ npx create-nuxt-app <project-name>
```

という記述があり、おなじみの`install`みたいな何かという勝手な認識だけで進めていました。

しかし、何回かプロジェクト作成を繰り返す中で`npx`について理解していないことに気づき、調べてみたところ便利だと感じたのでまとめてみます。

間違っている部分もあるかと思われますので、ご指摘いただけると幸いです。

# What？ npx

`npx`はnpmバージョン5.2.0より同梱されているコマンドで、ローカルインストールしたコマンドを実行するために使われます。npmのバージョン5.2.0がリリースされたのは、今から約3年ほど前になるため、今主に使われているnpmではほとんど使える状態にあります。

便利な機能を使える状態であえて使わないのはもったいないので是非使っていきたいコマンドの１つです。

https://www.npmjs.com/package/npx

もし`npx`が存在しない場合は以下のコマンドでグローバルにインストールできます。

```shell
$ npm install -g npx
```

# Why？ npx

なぜ、`npx`なのでしょうか。

npmを使ってプロジェクトを作成、初期化するコマンドは以下の通りです。

```shell
$ npm init <project-name>
```

これはnpmを使いプロジェクトを管理配下へ置くために必要なpackage.jsonを生成します。

npmを使ってスクリプトを実行するためにはpackage.jsonの`scripts`へ予めスクリプトを定義しておく必要があります。

```json
 "scripts": {
    "dev": "nuxt",
    "build": "nuxt build",
    "start": "nuxt start",
    "generate": "nuxt generate"
  },
```

上記は[nuxt/create-nuxt-app](https://github.com/nuxt/create-nuxt-app)で作成したプロジェクトから抜粋。

`npx`を使うことでインストールされていないコマンドであっても自動的に探してインストール、実行まで行ってくれます。

実行したあとはパッケージの除去まで行ってくれるため、環境が汚れる心配はないそうです。

**インストールするまでも無いけど手軽に試したい**、**環境を汚したくない**そんなニーズにぴったりだと感じました。

# How Use？ npx

npxコマンドの基本的な使い方は次のとおりです。

**インストール済みモジュールの実行**

```shell
$ npx <インストール済みモジュール名>
```

**インストールしていないモジュールを実行（実行後に自動削除）**

```shell
$ npx <未インストールのモジュール名>
```

**GitHub上の特定リポジトリを明示的に指定して実行**

```shell
$ npx github:<リポジトリ名>
```

GitHubのリポジトリを直接指定して実行もできるのが便利ですね。

npxコマンド実行が実行されるとローカル内で指定されたNodeモジュール格納パスの探しに行き、見つからない場合はインターネット上から探して自動的にインストールするようです。
実行したNodeモジュールは実行後に自動的に削除されるため、環境を大きく汚すことはありません。

前程挙げたNuxt.jsのプロジェクト作成コマンドをもう一度見てみます。

```shell
$ npx create-nuxt-app <project-name>
```

つまりプロジェクトを作る際、[create-nuxt-app](https://github.com/nuxt/create-nuxt-app)というモジュールを実行することで各モジュール間の依存関係を解決しながらプロジェクトを作成できるというイメージです。

Nuxt.jsを使ったプロジェクトはスクラッチで１から始めることもできますが、create-nuxt-appを使うことで**簡単に始められるのは大きなメリット**だと感じます。

# まとめ

`npx`を使うことで、インストールせずともスクリプトの実行が簡単に行えることが分かりました。

普段何気なく叩いているコマンド1つでも意味が分かると**点と点がつながるようにプログラムへの理解がぐっと深まるように感じます。**

>「百聞は一見にしかず」

最後まで読んでいただきありがとうございました。

# 参考

- [nuxt/create-nuxt-app: Create Nuxt.js App in seconds.](https://github.com/nuxt/create-nuxt-app)
- [npx-npm](https://www.npmjs.com/package/npx)
- [npm 5.2.0 の新機能！ 「npx」でローカルパッケージを手軽に実行しよう - Qiita](https://qiita.com/tonkotsuboy_com/items/8227f5993769c3df533d#comments)
- [npm と npx。なにが違う？ - Qiita](https://qiita.com/sivertigo/items/622550c5d8ec991e59a6)