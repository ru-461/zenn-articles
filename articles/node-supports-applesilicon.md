---
title: "Node.jsがv16でAppleSiliconに対応していた"
emoji: "🍎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["nodejs","applesilicon"]
published: false
---

# はじめに

Node.js v16 系となるバージョンが 2021 年の 4 月 20 日にリリースされました。v14 に次ぐ [LTS( Long Term Support )](https://nodejs.org/ja/about/releases/)となるバージョンになります。Node.js はメジャーバージョンが偶数になるタイミングで LTS となり、約 30 ヶ月間のサポートがされる形となっています。

またリリースしてすぐ LTS のアクティブリリースになるわけではなく、約半年後にアクティブ LTS リリースとなります。

そして今回最リリースされた新の LTS、Node.js 16 が AppleSilicon をサポートする最初のバージョンとなります。つまり M1 チップを始めとする SOC を搭載した MacBook や iMac 上にてネイティブ動作するようになったということです。これはすごいですね。

リリースノート(GitHub)はこちらになります。

https://github.com/nodejs/node/blob/master/doc/changelogs/CHANGELOG_V16.md

# さっそくインストールしてみる

それでは、Node.js 16 を実際に AppleSilicon な Mac へインストールしていきます。今回はローカルと Docker でそれぞれインストールして検証しました。

この記事の内容は執筆時点で M1 チップを搭載した Macbook Pro にて検証したものになります。

## ローカルにnodenvを使ってインストール

まず、ローカルな環境に Node.js をインストールします。公式サイトからバイナリをダウンロードしてきてもいいのですが、すでに anyenv を使って node のバージョン切り替えができる環境を構築しているため、nodenv を使用します。

anyenv のインストールは[以前投稿したこちらの記事](https://zenn.dev/ryuu/articles/use-anyversions)で詳しく紹介しているのでこの記事では割愛します。

nodenv anyenv 経由で以下のコマンドを使ってインストールできます。

```shell
# anyenvでnodenvをインストール
$ anyenv install nodenv

# シェルを再起動
$ exec $SHELL -l
```

最新のリリースに対応するため[anyenv update](https://github.com/znz/anyenv-update) という更新プラグインを使用し、最新の状態にアップデートしておきます。

```shell
$ anyenv update
```

アップデートが完了したら nodenv から最新の Node.js をインストールします。記事執筆時点では **v16.2.0** がリリースされていたのでこちらを指定します。

```shell
# 最新のNode.jsをインストール
$ nodenv install 16.2.0

# rehash処理
$ nodenv rehash

# バージョン確認
$ node -v
  v16.20.0
```

バージョンの表示ができれば成功です。

# おわりに